---
id: 0007-visszavaltas
artifact: traceability
updated: 2026-07-05
spec: ./spec.md
---

# Nyomonkövethetőségi mátrix: Visszaváltás és visszatérítés

> Szükséglet → követelmény → elfogadás → teszt → kód. A stabil ID-kből generálva. A kód még nem
> épült meg, ezért a `Kód` oszlop üres és a `Verified` bejelöletlen (DoD-kapu előtti állapot).

## Előre-nyomon

| Köv ID | Követelmény (röviden) | Forrás szükséglet | Task(ok) | Teszt(ek) | Kód | Verified |
|--------|------------------------|-------------------|----------|-----------|-----|:--------:|
| FR-1 | Érvényes visszaváltás → jegy `void` | PRD §6 / spec §1 | T-4,T-8 | `test_visszavaltas_void__FR1` | — | ☐ |
| FR-2 | Összeg = ár × visszatérítési arány | spec §4 | T-1,T-2 | `test_osszegszamitas__FR2` | — | ☐ |
| FR-3 | `used`/`void` → elutasítás | spec §4 | T-3 | `test_used_elutasit__FR3` | — | ☐ |
| FR-4 | Esemény után → elutasítás | spec §4 | T-3 | `test_esemeny_utan__FR4` | — | ☐ |
| FR-5 | Esemény törlése → 100% visszatérítés | spec §4 | T-7 | `test_torles_100__FR5` | — | ☐ |
| FR-6 | Szolgáltatói hiba → `refund_pending` retry | spec §4 | T-6 | `test_refund_pending__FR6` | — | ☐ |
| FR-7 | Idempotens visszaváltás | spec §4 | T-5 | `test_idempotens_refund__FR7` | — | ☐ |
| FR-8 | Visszatérítés indítása | spec §4 | T-6 | `test_refund_indul__FR8` | — | ☐ |
| FR-9 | Kontingens +1 void-nál | spec §4 | T-4 | `test_kontingens_no__FR9` | — | ☐ |
| NFR-1 | Visszaváltás p95 < 500 ms @50/s | spec §5 | T-9 | `perf_refund_p95__NFR1` | — | ☐ |
| NFR-2 | Void + kontingens atomi | spec §5 | T-4,T-9 | `test_refund_atomi__NFR2` | — | ☐ |
| NFR-3 | Visszatérítés indul ≤ 5 perc 99% | spec §5 | T-6,T-9 | `test_refund_5perc__NFR3` | — | ☐ |

## Hátra-nyomon

| Kód / modul | Megvalósítja | Eredő szükséglet |
|-------------|--------------|------------------|
| `RefundService` (tervezett) | FR-1,FR-2,FR-3,FR-4,FR-7,FR-9 | „vissza akarom kapni a pénzem, ha nem megyek” |
| `RefundOutbox` (tervezett) | FR-6,FR-8,NFR-3 | „garantált visszatérítés szolgáltatói hiba mellett is” |
| `EventCancellationHandler` (tervezett) | FR-5 | fogyasztóvédelmi elvárás esemény-törlésnél |

## Hiányok
- Teszt nélküli követelmény: nincs.
- Követelmény nélküli kód: nincs (a kód még nem épült meg).
- `Verified`: a megvalósítás + a DoD-kapu után jelölhető be.
