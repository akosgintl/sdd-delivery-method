---
id: 0005-penztar-fizetes
artifact: traceability
updated: 2026-07-05
spec: ./spec.md
---

# Nyomonkövethetőségi mátrix: Pénztár és fizetés

> Szükséglet → követelmény → elfogadás → teszt → kód. A stabil ID-kből generálva. A kód még nem
> épült meg, ezért a `Kód` oszlop üres és a `Verified` bejelöletlen (DoD-kapu előtti állapot).

## Előre-nyomon

| Köv ID | Követelmény (röviden) | Forrás szükséglet | Task(ok) | Teszt(ek) | Kód | Verified |
|--------|------------------------|-------------------|----------|-----------|-----|:--------:|
| FR-1 | Jóváhagyás → Holdok `converted` | PRD §6 / spec §1 | T-4 | `test_konverzio__FR1` | — | ☐ |
| FR-2 | Elutasítás → nincs Rend | spec §4 | T-6 | `test_elutasitas__FR2` | — | ☐ |
| FR-3 | Idempotens fizetés | spec §4 | T-3,T-11 | `test_idempotens_fizetes__FR3` | — | ☐ |
| FR-4 | Lejárt Hold → nincs terhelés | spec §4 | T-7,T-11 | `test_lejart_nincsterheles__FR4` | — | ☐ |
| FR-5 | `paid` → jegygenerálás esemény | spec §4 | T-9 | `test_order_paid_event__FR5` | — | ☐ |
| FR-6 | Fizetési audit (PAN nélkül) | alkotmány Q-4 | T-1,T-10 | `test_audit_nopan__FR6` | — | ☐ |
| FR-7 | `paid` Rend létrehozása | spec §4 | T-5 | `test_paid_order__FR7` | — | ☐ |
| FR-8 | Téves terhelés → auto-visszatérítés | spec §4 | T-8 | `test_auto_refund__FR8` | — | ☐ |
| NFR-1 | Véglegesítés p95 < 2000 ms @100/s | spec §5 | T-12 | `perf_checkout_p95__NFR1` | — | ☐ |
| NFR-2 | Nincs PAN nyugalmi állapotban | alkotmány Q-4 | T-2 | `test_no_pan_atrest__NFR2` | — | ☐ |
| NFR-3 | Fizetés–konverzió 100% konzisztencia | spec §5 | T-4,T-12 | `test_konzisztencia__NFR3` | — | ☐ |

## Hátra-nyomon

| Kód / modul | Megvalósítja | Eredő szükséglet |
|-------------|--------------|------------------|
| `CheckoutService` (tervezett) | FR-1,FR-3,FR-4,FR-7 | „túlértékesítés és dupla terhelés nélkül” |
| `PaymentGatewayAdapter` (tervezett) | NFR-2 | „kártyaadat-tárolás nélküli fizetés” |
| `Outbox` (tervezett) | FR-5,FR-8 | „megbízható jegygenerálás és korrekció” |

## Hiányok
- Teszt nélküli követelmény: nincs.
- Követelmény nélküli kód: nincs (a kód még nem épült meg).
- `Verified`: a megvalósítás + a DoD-kapu után jelölhető be.
