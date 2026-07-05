---
artifact: spec
target: ./spec.md
round: 1
reviewed: 2026-07-04
verdict: approved
---

# Review of ./spec.md — round 01

## Findings

- [MINOR] NFR-1 / NFR-2 / NFR-3 · no §6 acceptance criterion cites these IDs; §6 covers only FR-1..FR-6 · spec.md:45
  rule: gherkin 1:1 — every FR/NFR has ≥1 criterion naming its ID (../_shared/gherkin.md)
  fix: add §6 checklist lines, pl. "A kimutatás betöltése p95 < 1000 ms egy ≤100000 jegyes eseményre (NFR-1)", "CSV-export ≤100000 sorra 30 mp-en belül (NFR-2)", "A kimutatás felülete WCAG 2.2 AA (NFR-3)". Nem blokkoló: a referencia példa is tolerál kritérium nélküli NFR-t.
  resolved: [x]

## Notes (non-blocking, no defect)
- Minden FR (FR-1..FR-6) 1:1 leképeződik §6 kritériumra (FR-1/FR-5 forgatókönyv, FR-2/FR-3/FR-4/FR-6 checklist).
- Front-matter teljes; status `draft`; §10 üres; cél és nem-cél megvan; §7 él- és hibahelyzetek (nulla eladás, folyamatban lévő visszaváltás) lefedve.
- Kereszthivatkozások (`0007`, `0008/FR-5`, `0002/0004/0005`) és a `need` link formailag érvényesek.
- Nincs tiltott vágó szó normatív sorban; a „valós idejű" a célban prózai és FR-2/NFR számmal (≤10 s) kvantifikált.
