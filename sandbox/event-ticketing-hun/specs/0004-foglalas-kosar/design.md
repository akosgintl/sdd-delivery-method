---
id: 0004-foglalas-kosar
artifact: design
status: agreed            # draft | in-review | agreed | done
owner: vasarloi-csapat
updated: 2026-07-04
spec: ./spec.md
---

# Technikai terv: Foglalás és kosár lejárati idővel

> Hogyan elégítjük ki a `spec.md`-t. Ez a **hogyan**; a viselkedés a specifikációban van.

## 1. Áttekintés
A Foglalás szerver-vezérelt: a szerver dönt a létrehozásról, a lejáratról és a felszabadításról. Az
elérhető kontingens jegytípusonként egyetlen, verziózott számláló, amelyet a Foglalás létrehozása,
lejárata és törlése **ugyanabban az adatbázis-tranzakcióban**, optimista zárolással módosít. Egy
háttér-söprő a lejárt Foglalásokat 5 másodpercen belül felszabadítja. A lejárat fix 10 perc
(`ADR-0001`).

## 2. Megközelítés és mérlegelt alternatívák
| Opció | Előnyök | Hátrányok | Döntés |
|-------|---------|-----------|--------|
| Verziózott kontingens-számláló + tranzakciós CAS | pontos túlértékesítés-védelem, magas átbocsátás | odafigyelést igényel a versenyhelyzetnél | ✅ választott |
| Elosztott zár jegytípusonként (Redis lock) | egyszerű mentális modell | a zár a fizetés idejére is fennállna → rontja az NFR-1 átbocsátást | elvetve |
| Csak ülőhely-soronkénti foglalás | ülőhelyes eseményre elég | állóhelyes kontingensre nem működik | elvetve |

A lejárat fix vs. jegytípusonként állítható döntése: `../../adr/0001-foglalas-lejarat-strategia.md`.

## 3. Komponensek és felelősségek
- **HoldService** — Foglalás létrehozása (FR-1), a 10 perces lejárat beállítása
  (`expires_at = created_at + 10 perc`, FR-2), elvetése (FR-6), idempotencia (FR-9), a 10 jegyes
  korlát ellenőrzése (FR-7).
- **QuotaStore** — az elérhető kontingens verziózott számlálója; csökkentés (FR-10) és növelés
  (FR-11) CAS-sal, a Foglalás-tranzakció részeként; az `active` Holdban lekötött darabszám a
  számlálót foglalja, így más vásárló nem veheti el (FR-5).
- **ExpirySweeper** — háttér-feladat, amely a `expires_at < now()` `active` Foglalásokat `expired`
  állapotba állítja és felszabadítja (FR-3, FR-11, NFR-2).
- **IdempotencyStore** — az idempotencia-kulcs → eredmény leképezés (FR-9).

## 4. Interfészek és szerződések
- `POST /holds` (fejléc: `Idempotency-Key`) → Foglalás létrehozása — megvalósítja FR-1, FR-4, FR-7,
  FR-9, FR-10.
- `DELETE /holds/{id}` → elvetés — megvalósítja FR-6, FR-11.
- `GET /holds/{id}` → állapot lekérdezés.
- Belső esemény: `hold.converted` a fizetés felől (FR-8) — a `0005` bocsátja ki.

## 5. Adatmodell
- `Hold(id, buyer_id, event_id, ticket_type_id, seat_ids[], quantity, status, created_at,
  expires_at, idempotency_key)`, index: `(status, expires_at)` a söprőhöz.
- `TicketType.available_quota INT`, `TicketType.version INT` — optimista zárolás a CAS-hoz.
- Az elérhető kontingens = `quota - lekötött`; a lekötést a számláló tükrözi (FR-6, `0002`).

## 6. Integrációs pontok és függőségek
- `0002-jegytipusok-arazas`: az elérhető kontingens forrása; a QuotaStore közös.
- `0005-penztar-fizetes`: a konverzió (FR-8) a fizetés atomikus tranzakciójából (`ADR-0002`).
- `0003-helyvalasztas`: az ülőhely-kiválasztás Foglalást indít (`0003/FR-6`).

## 7. Teszt-stratégia
- FR-1 → integrációs teszt: foglalás létrehoz `active` Holdot lejárattal.
- FR-2 → egységteszt: `expires_at == created_at + 10 perc`.
- FR-3 → integrációs teszt a söprővel: lejárat után `expired`.
- FR-4 → integrációs teszt: 0 kontingensnél elutasítás.
- FR-5 → konkurrens teszt: `active` Hold jegye nem foglalható újra.
- FR-6 → integrációs teszt: elvetés → `cancelled`.
- FR-7 → integrációs teszt: 11. jegy elutasítva.
- FR-8 → szerződéses teszt a `0005`-tel: fizetés → `converted`, kontingens nem nő.
- FR-9 → teszt: azonos idempotencia-kulcs → egy Hold.
- FR-10/FR-11 → integrációs teszt: számláló csökken/nő a Hold életciklusával.
- NFR-1 → terheléses teszt: p95 < 500 ms 200 RPS.
- NFR-2 → időzítés-teszt: felszabadítás ≤ 5 s a lejárat után.
- NFR-3 → konkurrencia-teszt: 200 párhuzamos foglalás az utolsó jegyekre → 0 túl/alul.

## 8. Kockázatok és mérséklések
| Kockázat | Valószínűség | Hatás | Mérséklés |
|----------|--------------|-------|-----------|
| Óracsúszás a szerverek közt | közepes | lejárat pontatlansága | egyetlen tekintélyes időforrás; a lejáratot a DB `now()` dönti |
| Söprő-késés csúcson | közepes | NFR-2 sérülhet | partícionált söprő, 1 s ciklus, riasztás > 5 s késésre |
| CAS-újrapróbálkozás vihar | alacsony | latencia-tüske | korlátozott újrapróbálás + backoff |

## 9. Kivezetés és üzemeltethetőség
- Feature flag: `hold_expiry_enabled`.
- Migráció: `Hold` tábla + `TicketType.version` oszlop; visszafelé kompatibilis.
- Megfigyelhetőség: metrika a lejárt/felszabadított Holdokra, a söprő késésére, a CAS-ütközésekre;
  riasztás NFR-2 megsértésére. Visszaállítás: a flag kikapcsolása leállítja az új Holdokat, a
  meglévők lejárnak.
