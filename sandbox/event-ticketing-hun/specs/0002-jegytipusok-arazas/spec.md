---
id: 0002-jegytipusok-arazas
title: Jegytípusok és árazás
status: ready            # draft | in-review | ready | in-progress | done | superseded
owner: szervezoi-csapat
created: 2026-07-04
updated: 2026-07-04
need: ../../discovery/prd-event-ticketing.md#6-feature-bontás--specifikációk
supersedes: null
---

# Specifikáció: Jegytípusok és árazás

> A szándékolt viselkedés pontos, tesztelhető leírása — a szerződés.

## 1. Összefoglaló és kontextus
Egy eseményhez a szervezőnek eladható Jegytípusokat kell definiálnia névvel, árral és
kontingenssel (pl. „Állóhely”, „VIP”). A jegytípusok kontingensei együtt nem léphetik túl az
esemény teljes kapacitását. Származó szükséglet: `../../discovery/prd-event-ticketing.md`.

## 2. Célok
- A szervező eseményenként több Jegytípust tud létrehozni árral és kontingenssel.
- A jegytípusok kontingenseinek összege garantáltan az esemény kapacitásán belül marad.

## 3. Nem-célok (hatókörön kívül)
- Dinamikus, keresletalapú árazás (PRD nem-cél).
- Kedvezménykódok és kuponok (későbbi feature).
- Ülőhelyhez kötött árazás (a helyfoglalás–jegytípus párosítást a `0003` definiálja).

## 4. Funkcionális követelmények
- **FR-1:** Amikor a szervező Jegytípust hoz létre egy eseményhez névvel, egész forintáras árral és
  kontingenssel, a rendszernek létre kell hoznia a Jegytípust, és egyedi azonosítót kell rendelnie.
- **FR-2:** Ha egy új vagy módosított Jegytípus kontingense miatt az esemény összes jegytípus-
  kontingensének összege meghaladná az esemény teljes kapacitását, akkor a rendszernek el kell
  utasítania a műveletet.
- **FR-3:** Ha a megadott ár nem 0 és 5000000 forint közötti egész szám, akkor a rendszernek el kell
  utasítania a Jegytípus létrehozását.
- **FR-4:** Amíg egy Jegytípusból van eladott vagy foglalt jegy, a rendszernek el kell utasítania a
  Jegytípus törlését.
- **FR-5:** Amikor a szervező módosítja egy Jegytípus árát, a rendszernek az új árat kizárólag a
  módosítás utáni foglalásokra kell alkalmaznia, és nem szabad visszamenőleg módosítania a már
  létrejött Foglalások vagy Rendek árát.
- **FR-6:** A rendszernek minden Jegytípushoz nyilván kell tartania az elérhető kontingenst mint a
  kontingens és a lekötött (eladott + foglalt) jegyek különbségét.

## 5. Nem funkcionális követelmények
- **NFR-1:** Az elérhető kontingens lekérdezésének p95 válaszideje < 200 ms 1000 RPS mellett.
- **NFR-2:** A Jegytípus-szerkesztő felület megfelel a WCAG 2.2 AA szintnek.

## 6. Elfogadási kritériumok / forgatókönyvek
```gherkin
Forgatókönyv: Jegytípus létrehozása kontingenssel          # igazolja FR-1
  Adott egy 500 kapacitású esemény
  Amikor a szervező létrehoz egy „VIP” jegytípust 20000 forint áron, 50 kontingenssel
  Akkor létrejön a jegytípus egyedi azonosítóval, és az elérhető kontingens 50

Forgatókönyv: Kapacitás túllépésének megakadályozása        # igazolja FR-2
  Adott egy 500 kapacitású esemény, amelyben már 480 kontingens van kiosztva
  Amikor a szervező egy 30 kontingensű új jegytípust próbál létrehozni
  Akkor a rendszer elutasítja a műveletet, mert 510 > 500
```
- [ ] 5000001 forintos ár megadásakor a rendszer elutasítja a létrehozást (FR-3).
- [ ] Eladott jeggyel rendelkező jegytípus törlése elutasításra kerül (FR-4).
- [ ] Ármódosítás után a korábbi Rendek ára változatlan marad, az új foglalások az új árral jönnek
  létre (FR-5).
- [ ] Egy jegy lefoglalása után az adott jegytípus elérhető kontingense pontosan eggyel csökken
  (FR-6).
- [ ] Az elérhető kontingens lekérdezésének p95 válaszideje < 200 ms 1000 RPS terhelési teszt alatt
  (NFR-1).
- [ ] A Jegytípus-szerkesztő automatizált WCAG 2.2 AA ellenőrzése hiba nélkül fut le (NFR-2).

## 7. Élhelyzetek és hibaviselkedés
- Egyidejű kontingens-módosítás két jegytípuson: a rendszernek sorosítania kell az összeg-
  ellenőrzést, hogy az FR-2 invariáns ne sérüljön (FR-2).
- Kontingens csökkentése a már lekötött mennyiség alá: elutasítás (FR-2, FR-6).

## 8. Adatok és interfészek
- `TicketType { id, event_id, name, price_huf, quota, reserved_count }`.
- `POST /events/{id}/ticket-types`, `PATCH /ticket-types/{id}`, `DELETE /ticket-types/{id}`.
- Függőség: Esemény (`0001`) kapacitás-invariánsa; az elérhető kontingenst a `0004` foglalás írja.

## 9. Függőségek és feltételezések
- Függőségek: `0001-esemeny-kezeles` (esemény és teljes kapacitás).
- Feltételezés: az árak forintban, ÁFA-val együtt értendők; egy pénznem az MVP-ben.

## 10. Nyitott kérdések
*(nincs)*

## 11. Indoklás / döntések
- Az árváltozás nem visszaható jellege (FR-5) védi a már fizetett vásárlókat és a szervező
  elszámolását.
- Az elérhető kontingens származtatott értéke (FR-6) az egyetlen igazságforrás a túlértékesítés
  ellen (alkotmány A-3).
