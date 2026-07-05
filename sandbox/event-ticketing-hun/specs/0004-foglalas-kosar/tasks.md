---
id: 0004-foglalas-kosar
artifact: tasks
status: ready            # draft | ready | in-progress | done
updated: 2026-07-04
spec: ./spec.md
design: ./design.md
---

# Feladatbontás: Foglalás és kosár lejárati idővel

> A terv rendezett, függőség-tudatos felbontása függetlenül igazolható feladatokra. Minden feladat
> megnevezi a követelmény(eke)t, amelye(ke)t előrevisz.

## Jelmagyarázat
- **ID** `T-n` · **Függ** függőség · **Köv** előrevitt követelmény · **Becs** becslés · `[P]` párhuzamosítható

| ID | Feladat | Függ | Köv | Becs | Státusz |
|----|---------|------|-----|------|---------|
| T-1 | `Hold` tábla + `TicketType.version` séma és migráció | — | FR-1, FR-10 | M | todo |
| T-2 | QuotaStore: elérhető kontingens CAS csökkentés/növelés | T-1 | FR-10, FR-11, NFR-3 | M | todo |
| T-3 | HoldService: Foglalás létrehozása idempotencia-kulccsal | T-1,T-2 | FR-1, FR-4, FR-9 | M | todo |
| T-4 | Lejárati időbélyeg beállítása (10 perc, konfig) [P] | T-3 | FR-2 | S | todo |
| T-5 | 10 jegyes eseményenkénti korlát ellenőrzése [P] | T-3 | FR-7 | S | todo |
| T-6 | Hold elvetése → `cancelled` + kontingens-visszanövelés | T-2,T-3 | FR-6, FR-11 | S | todo |
| T-7 | ExpirySweeper háttér-feladat (lejárat → felszabadítás) | T-2,T-3 | FR-3, FR-11, NFR-2 | M | todo |
| T-8 | Konverziós hook a fizetés felől (`converted`, nincs visszanövelés) | T-3 | FR-8 | S | todo |
| T-9 | `active` Hold kizárása más vásárló elől (konkurrens) | T-2,T-3 | FR-5 | M | todo |
| T-10 | Elfogadási + terheléses/konkurrencia tesztek FR/NFR ID-kre | T-4,T-5,T-6,T-7,T-8,T-9 | FR-1..FR-11, NFR-1..NFR-3 | L | todo |

## Kritikus út
T-1 → T-2 → T-3 → T-7 → T-10 (a leghosszabb függőségi lánc).

## Párhuzamosítható
T-4 és T-5 a T-3 után egymással párhuzamosan haladhat; T-8 és T-9 is indulhat a T-3/T-2 után.

## Megjegyzések
- Az NFR-3 (pontos kontingens 200 párhuzamos foglalásnál) igazolása a T-2 CAS-logikáját és a T-10
  konkurrencia-tesztet igényli.
- A T-8 szerződéses hookja a `0005` fizetési tranzakciójával közösen tesztelendő (`ADR-0002`).
