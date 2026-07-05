---
id: 0007-visszavaltas
artifact: design
status: agreed            # draft | in-review | agreed | done
owner: fizetesi-csapat
updated: 2026-07-04
spec: ./spec.md
---

# Technikai terv: Visszaváltás és visszatérítés

> Hogyan elégítjük ki a `spec.md`-t. Ez a **hogyan**; a viselkedés a specifikációban van.

## 1. Áttekintés
A visszaváltás a lokális állapotváltozásokat — jegy `void` (FR-1) és kontingens visszanövelése
(FR-9) — egyetlen adatbázis-tranzakcióban végzi (NFR-2 atomicitás), a szolgáltatói visszatérítést
(FR-8) pedig tranzakciós outboxon, idempotencia-kulccsal, legalább-egyszer kézbesítéssel és ≥24 órás
újrapróbálással (FR-6). A kontingens azonnal felszabadul, nem várunk a szolgáltatói megerősítésre
(`ADR-0003`).

## 2. Megközelítés és mérlegelt alternatívák
| Opció | Előnyök | Hátrányok | Döntés |
|-------|---------|-----------|--------|
| Lokális tranzakció (void+kontingens) + outbox-visszatérítés | azonnali újra-eladhatóság, garantált visszatérítés | ritka `refund_pending` tartozás | ✅ választott |
| Kontingens csak a szolgáltatói megerősítés után | soha nincs korai felszabadítás | a szolgáltatói késés bent tartja az eladható helyet | elvetve |
| Szinkron szolgáltatói visszatérítés a tranzakcióban | egyszerű | hosszú DB-zár, részleges hiba | elvetve |

Az atomicitás döntése: `../../adr/0003-visszavaltas-keszletfelszabaditas-atomicitas.md`.

## 3. Komponensek és felelősségek
- **RefundService** — a visszaváltás vezénylése: void (FR-1), összegszámítás (FR-2), állapot-
  ellenőrzés (FR-3, FR-4), idempotencia (FR-7).
- **QuotaStore** (közös a `0004`-gyel) — kontingens visszanövelése a lokális tranzakcióban (FR-9).
- **RefundOutbox** — a szolgáltatói visszatérítés kézbesítése, `refund_pending` követés,
  újrapróbálás (FR-6, FR-8, NFR-3).
- **EventCancellationHandler** — esemény törlésekor tömeges 100%-os visszatérítés (FR-5).

## 4. Interfészek és szerződések
- `POST /tickets/{id}/refund` (fejléc: `Idempotency-Key`) → visszaváltás — megvalósítja FR-1, FR-2,
  FR-3, FR-4, FR-7, FR-8, FR-9.
- Belső: `event.cancelled` fogyasztó (`0001/FR-6`, az esemény törlése) → tömeges visszatérítés (FR-5).
- Kimenő: `provider.refund` az outboxból; `ticket.voided` a `0006/FR-5` felé.

## 5. Adatmodell
- `Refund(id, order_id, issued_ticket_id, amount_huf, status, idempotency_key, created_at)`,
  állapotok: `refund_pending → refunded | failed`.
- `Event.refund_rate` (0–100%) — a szervező által beállított arány (FR-2).
- A `void` jegy és a kontingens-növelés ugyanabban a tranzakcióban (NFR-2).

## 6. Integrációs pontok és függőségek
- `0006-jegykibocsatas`: a jegy `void` állapota (FR-1 → `0006/FR-5`).
- `0002-jegytipusok-arazas`: a QuotaStore közös (FR-9).
- `0001-esemeny-kezeles`: az `event.cancelled` esemény indítja az FR-5-öt.
- Külső: fizetési szolgáltató (részleges/teljes visszatérítés, idempotencia-kulcs).

## 7. Teszt-stratégia
- FR-1 → integrációs teszt: érvényes visszaváltás → jegy `void`.
- FR-2 → egységteszt: 10000 Ft × 80% = 8000 Ft.
- FR-3 → teszt: `used`/`void` jegy visszaváltása elutasítva.
- FR-4 → teszt: esemény után elutasítás, kivéve `cancelled`.
- FR-5 → integrációs teszt: esemény törlése → minden `valid` jegy 100% visszatérítés.
- FR-6 → hibainjektálás: szolgáltatói hiba → `refund_pending`, újrapróbálás.
- FR-7 → teszt: azonos idempotencia-kulcs → egy visszatérítés.
- FR-8 → integrációs teszt: void → outbox `provider.refund`.
- FR-9 → integrációs teszt: void → kontingens +1 ugyanabban a tranzakcióban.
- NFR-1 → terheléses teszt: p95 < 500 ms 50 visszaváltás/s.
- NFR-2 → konkurrencia/konzisztencia-teszt: nincs `void` kontingens-felszabadítás nélkül és fordítva.
- NFR-3 → időzítés-teszt: visszatérítés indul ≤ 5 perc 99%-ban.

## 8. Kockázatok és mérséklések
| Kockázat | Valószínűség | Hatás | Mérséklés |
|----------|--------------|-------|-----------|
| Szolgáltatói visszatérítési hiba | közepes | `refund_pending` tartozás | ≥24 h újrapróbálás, riasztás a kor alapján |
| Visszaváltás–beléptetés verseny | alacsony | dupla érvényesítés | sorosított döntés a jegy során (FR-1/FR-3, `0008`) |
| Esemény-törlés tömeges terhelés | alacsony | outbox-torlódás | kötegelt feldolgozás, backpressure |

## 9. Kivezetés és üzemeltethetőség
- Feature flag: `refunds_enabled`.
- Migráció: `Refund` tábla, `Event.refund_rate` oszlop.
- Megfigyelhetőség: metrika a `refund_pending` korra, a visszatérítési hibaarányra, az atomicitás-őr
  (NFR-2) sértésére; riasztás > 24 h `refund_pending`-re. Visszaállítás: `refunds_enabled`
  kikapcsolása a kézi visszatérítésre vált, a jegy-void tiltása nélkül.
