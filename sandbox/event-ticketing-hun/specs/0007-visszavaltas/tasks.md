---
id: 0007-visszavaltas
artifact: tasks
status: ready            # draft | ready | in-progress | done
updated: 2026-07-05
spec: ./spec.md
design: ./design.md
---

# Feladatbontás: Visszaváltás és visszatérítés

> A terv rendezett, függőség-tudatos felbontása függetlenül igazolható feladatokra.

## Jelmagyarázat
- **ID** `T-n` · **Függ** függőség · **Köv** előrevitt követelmény · **Becs** becslés · `[P]` párhuzamosítható

| ID | Feladat | Függ | Köv | Becs | Státusz |
|----|---------|------|-----|------|---------|
| T-1 | `Refund` séma + `Event.refund_rate` oszlop és migráció | — | FR-2 | S | todo |
| T-2 | Összegszámítás (ár × visszatérítési arány) | T-1 | FR-2 | S | todo |
| T-3 | RefundService: állapot-ellenőrzés (valid/used/void, esemény előtt) | T-1 | FR-3, FR-4 | M | todo |
| T-4 | Lokális atomi tranzakció: jegy `void` + kontingens +1 | T-1,T-3 | FR-1, FR-9, NFR-2 | L | todo |
| T-5 | Idempotencia-réteg a visszaváltásra [P] | T-1 | FR-7 | S | todo |
| T-6 | RefundOutbox: szolgáltatói visszatérítés, `refund_pending`, ≥24 h retry | T-4 | FR-6, FR-8 | M | todo |
| T-7 | EventCancellationHandler: tömeges 100% visszatérítés | T-2,T-4 | FR-5 | M | todo |
| T-8 | `ticket.voided` esemény a 0006 felé [P] | T-4 | FR-1 | S | todo |
| T-9 | Elfogadási + terheléses + atomicitás tesztek FR/NFR ID-kre | T-4,T-5,T-6,T-7 | FR-1..FR-9, NFR-1..NFR-3 | L | todo |

## Kritikus út
T-1 → T-3 → T-4 → T-6 → T-9 (a leghosszabb függőségi lánc).

## Párhuzamosítható
T-5 a T-1 után; T-8 a T-4 után; T-2 korán indulhat a T-1 után.

## Megjegyzések
- A T-4 atomicitása (NFR-2) a `0004` QuotaStore-jával közös lokális tranzakció (`ADR-0003`).
- A T-6 outbox-újrapróbálása a szolgáltatói hibát (FR-6) és a kényszer-visszatérítést (`0005/FR-8`)
  egyaránt kiszolgálja.
