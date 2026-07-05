---
artifact: gate
gate: DoR
target: ./spec.md
round: 1
reviewed: 2026-07-05
verdict: pass
---

# DoR-kapu: 0007-visszavaltas — 1. futás

A `memory/definition-of-ready.md` és a `gate-criteria.md` (DoR-1..10) ellen ellenőrizve.

## Eredmény: PASS

- A. Szándék világos: PRD §6, fogyasztóvédelmi cél. ✓
- B. Specifikáció megalapozott: `spec.md` `ready`; FR-1..FR-9 stabil ID, magyar EARS, tesztelhető;
  NFR-1..NFR-3 számszerűsített; elfogadás 1:1 (spec-review r2 approved); élhelyzetek §7; célok +
  nem-célok; Open Questions üres; fogalmak a glosszáriumhoz igazítva. ✓
- C. Megvalósítható és körülhatárolt: `tasks.md`; függőség (`0005`, `0006`, külső szolgáltató)
  azonosítva; az atomicitás-döntés ADR-0003-ban. ✓
- D. Egyeztetett: független spec-review (2 kör) jóváhagyta; `design.md` egyeztetve, ADR-0003. ✓
- AI-munkához: önhordó, gépiesen ellenőrizhető, explicit interfész (POST /tickets/{id}/refund). ✓

Nincs blokkoló kritérium.
