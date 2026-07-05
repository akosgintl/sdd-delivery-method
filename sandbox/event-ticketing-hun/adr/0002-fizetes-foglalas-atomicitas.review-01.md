---
artifact: adr
target: ./0002-fizetes-foglalas-atomicitas.md
round: 1
reviewed: 2026-07-05
verdict: approved
---

# Review of ./0002-fizetes-foglalas-atomicitas.md — round 1

> Megjegyzés: **self-review** (session-limit miatt a független ADR-reviewer nem futott le végig).
> A független újranézés a `TEST-REPORT.md`-ben jelölt nyitott elem.

## Findings

- Pontosan egy döntés (feltételes/CAS véglegesítés + outbox a lejárat–fizetés versenyhelyzetre). ✓
- Globálisan egyedi szám (0002). ✓
- Legális státusz (`accepted`), dátum, döntéshozók, kapcsolódó feature-ök (`0004`, `0005`). ✓
- A kontextus megnevezi a versengő igényt (túlértékesítés + dupla terhelés a szolgáltatói késés
  alatt) → miért most. ✓
- Valós elvetett alternatívák indokkal (pesszimista zár, Foglalás-hosszabbítás, 2PC). ✓
- Pozitív **és** negatív következmények (ritka „fizettél, de elveszett a hely” út). ✓

Nincs BLOCKER/MAJOR/MINOR.
