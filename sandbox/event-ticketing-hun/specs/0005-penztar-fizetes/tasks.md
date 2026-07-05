---
id: 0005-penztar-fizetes
artifact: tasks
status: ready            # draft | ready | in-progress | done
updated: 2026-07-05
spec: ./spec.md
design: ./design.md
---

# Feladatbontás: Pénztár és fizetés

> A terv rendezett, függőség-tudatos felbontása függetlenül igazolható feladatokra.

## Jelmagyarázat
- **ID** `T-n` · **Függ** függőség · **Köv** előrevitt követelmény · **Becs** becslés · `[P]` párhuzamosítható

| ID | Feladat | Függ | Köv | Becs | Státusz |
|----|---------|------|-----|------|---------|
| T-1 | `Order`, `IdempotencyRecord`, `OutboxMessage` séma és migráció | — | FR-6, FR-7 | M | todo |
| T-2 | PaymentGatewayAdapter: terhelés, token, PAN-mentesség | — | NFR-2 | M | todo |
| T-3 | Idempotencia-réteg (kulcs → eredmény) | T-1 | FR-3 | S | todo |
| T-4 | CheckoutService: CAS-véglegesítés (Hold-konverzió) | T-1,T-2,T-3 | FR-1 | L | todo |
| T-5 | `paid` Rend létrehozása a véglegesítésben [P] | T-4 | FR-7 | S | todo |
| T-6 | Elutasított terhelés kezelése (Hold megtartás) [P] | T-4 | FR-2 | S | todo |
| T-7 | Lejárt Hold → véglegesítés elutasítása, nincs terhelés | T-4 | FR-4 | M | todo |
| T-8 | Auto-visszatérítés téves terhelésnél (outbox → 0007) | T-4,T-7 | FR-8 | M | todo |
| T-9 | `order.paid` esemény kibocsátása (outbox → 0006) | T-5 | FR-5 | S | todo |
| T-10 | Fizetési audit napló (PAN nélkül) | T-1 | FR-6 | S | todo |
| T-11 | Webhook-kezelő (időtúllépés-feloldás, dedup) | T-2,T-3 | FR-3, FR-4 | M | todo |
| T-12 | Elfogadási + terheléses + konzisztencia tesztek FR/NFR ID-kre | T-4..T-11 | FR-1..FR-8, NFR-1..NFR-3 | L | todo |

## Kritikus út
T-1 → T-3 → T-4 → T-7 → T-8 → T-12 (a leghosszabb függőségi lánc).

## Párhuzamosítható
T-2 a T-1-gyel párhuzamosan indulhat; T-5 és T-6 a T-4 után egymással; T-9/T-10 is párhuzamos.

## Megjegyzések
- A T-4 CAS-véglegesítése a `0004` QuotaStore-jával közös tranzakcióban dolgozik (`ADR-0002`).
- Az NFR-3 (konzisztencia) igazolása a T-4 és a T-12 konzisztencia-őr tesztjét igényli.
