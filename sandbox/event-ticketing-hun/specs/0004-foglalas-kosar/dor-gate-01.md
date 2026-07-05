---
artifact: gate
gate: DoR
target: ./spec.md
round: 1
reviewed: 2026-07-05
verdict: pass
---

# DoR-kapu: 0004-foglalas-kosar — 1. futás

A `memory/definition-of-ready.md` és a `gate-criteria.md` (DoR-1..10) ellen ellenőrizve.

## Eredmény: PASS

- A. Szándék világos: a szükséglet a PRD §6-ra vezet, sikermutató a problem statementben. ✓
- B. Specifikáció megalapozott: `spec.md` létezik, `status: ready`; FR-1..FR-11 stabil ID, magyar
  EARS, tesztelhető; NFR-1..NFR-3 számszerűsített; elfogadási kritériumok 1:1 (spec-review r2
  approved); élhelyzetek §7-ben; célok + nem-célok; Open Questions üres; fogalmak a glosszáriumhoz
  igazítva (Elérhető kontingens definiálva). ✓
- C. Megvalósítható és körülhatárolt: becsülhető taskokra bontva (`tasks.md`); függőségek (`0002`,
  `0005`) azonosítva; alkotmány-megfelelés, a lejárat-döntés ADR-0001-ben rögzítve. ✓
- D. Egyeztetett: a spec független reviewer által jóváhagyva (2 kör); a technikai megközelítés a
  `design.md`-ben egyeztetve, ADR-0001 rögzítve. ✓
- AI-munkához: a spec önhordó, az elfogadási kritériumok gépiesen ellenőrizhetők, az interfészek
  explicitek. ✓

Nincs blokkoló kritérium.
