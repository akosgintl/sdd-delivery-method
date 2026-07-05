---
id: 0004-foglalas-kosar
artifact: traceability
updated: 2026-07-05
spec: ./spec.md
---

# Nyomonkövethetőségi mátrix: Foglalás és kosár lejárati idővel

> Szükséglet → követelmény → elfogadás → teszt → kód. A stabil ID-kből generálva. A kód még nem
> épült meg, ezért a `Kód` oszlop üres és a `Verified` bejelöletlen (ez a DoD-kapu előtti állapot).

## Előre-nyomon (lefedettség: minden követelmény megépült és tesztelt?)

| Köv ID | Követelmény (röviden) | Forrás szükséglet | Task(ok) | Teszt(ek) | Kód | Verified |
|--------|------------------------|-------------------|----------|-----------|-----|:--------:|
| FR-1 | Foglalás létrehozása `active`-ban | PRD §6 / spec §1 | T-1,T-3 | `test_hold_letrehoz__FR1` | — | ☐ |
| FR-2 | 10 perces lejárat | spec §4 | T-4 | `test_lejarat_10perc__FR2` | — | ☐ |
| FR-3 | Lejárat → `expired` | spec §4 | T-7 | `test_lejarat_expired__FR3` | — | ☐ |
| FR-4 | Elégtelen kontingens → elutasítás | spec §4 | T-3 | `test_nincs_kontingens__FR4` | — | ☐ |
| FR-5 | `active` Hold kizárólagos | spec §4 | T-9 | `test_active_kizaras__FR5` | — | ☐ |
| FR-6 | Elvetés → `cancelled` | spec §4 | T-6 | `test_elvetes_cancelled__FR6` | — | ☐ |
| FR-7 | 10 jegyes korlát | spec §4 | T-5 | `test_10jegy_korlat__FR7` | — | ☐ |
| FR-8 | Fizetés → `converted` | spec §4 | T-8 | `test_konverzio__FR8` | — | ☐ |
| FR-9 | Idempotens foglalás | spec §4 | T-3 | `test_idempotens_hold__FR9` | — | ☐ |
| FR-10 | Kontingens csökkentése létrehozáskor | spec §4 | T-1,T-2 | `test_kontingens_csokken__FR10` | — | ☐ |
| FR-11 | Kontingens növelése felszabaduláskor | spec §4 | T-2,T-6,T-7 | `test_kontingens_no__FR11` | — | ☐ |
| NFR-1 | Foglalás p95 < 500 ms @200 RPS | alkotmány Q-3 | T-10 | `perf_hold_p95__NFR1` | — | ☐ |
| NFR-2 | Felszabadítás ≤ 5 s | spec §5 | T-7,T-10 | `test_felszabaditas_5s__NFR2` | — | ☐ |
| NFR-3 | Pontos kontingens 200 párhuzamosnál | spec §5 | T-2,T-10 | `test_konkurrens_0tul__NFR3` | — | ☐ |

## Hátra-nyomon (indoklás: miért létezik?)

| Kód / modul | Megvalósítja | Eredő szükséglet |
|-------------|--------------|------------------|
| `HoldService` (tervezett) | FR-1,FR-2,FR-6,FR-7,FR-9 | „szellemfoglalások felszámolása” |
| `QuotaStore` (tervezett) | FR-5,FR-10,FR-11 | „túlértékesítés megszüntetése” |
| `ExpirySweeper` (tervezett) | FR-3,NFR-2 | „kifizetetlen foglalás felszabadítása” |

## Hiányok
- Teszt nélküli követelmény: nincs (minden FR/NFR-hez tervezett teszt tartozik).
- Követelmény nélküli kód: nincs (a kód még nem épült meg).
- `Verified`: minden sor a megvalósítás + a DoD-kapu után jelölhető be.
