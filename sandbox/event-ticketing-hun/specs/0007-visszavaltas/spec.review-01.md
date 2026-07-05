---
artifact: spec
target: ./spec.md
round: 1
reviewed: 2026-07-04
verdict: changes-requested
---

# Review of ./spec.md — round 01

## Findings

- [MAJOR] FR-1 · compound: three distinct positive behaviors ("`void` állapotba kell állítania" + "vissza kell térítenie" + "vissza kell növelnie a kontingenst") joined by "és" under one ID · spec.md:33
  rule: EARS singular — one behavior per FR (../_shared/ears.md)
  fix: split into three FRs (érvénytelenítés → `void`; visszatérítés indítása; kontingens +1). Az atomicitást NFR-2 már invariánsként rögzíti, így a bontás nem veszíti el az „egyetlen atomi művelet" garanciát; mindhárom effektusnak saját pass/fail ellenőrzés jár.
  resolved: [x]

- [MINOR] NFR-1 / NFR-2 / NFR-3 · no §6 acceptance criterion cites these IDs; §6 covers only FR-1..FR-7 · spec.md:54
  rule: gherkin 1:1 — every FR/NFR has ≥1 criterion naming its ID (../_shared/gherkin.md)
  fix: add §6 checklist lines, pl. "Visszaváltás-kezdeményezés p95 < 500 ms 50 visszaváltás/mp mellett (NFR-1)", egy sor NFR-2 atomicitásra, egy NFR-3 5-perces indulásra. (A referencia példa a teljesítmény-NFR-hez ad kritériumot — érdemes pótolni.)
  resolved: [x]

- [MINOR] FR-6 · If/Then error-recovery joins the "legalább 24 órán át újra kell próbálnia" retry behavior with the state-recording under one ID · spec.md:46
  rule: EARS singular (../_shared/ears.md)
  fix: opcionálisan bontsd külön FR-be az újrapróbálási viselkedést (saját pass/fail: „≥24 órán át újrapróbál"). Az összevont hibakezelő FR a referencia szerint tolerálható, ezért csak MINOR.
  resolved: [x]

- [MINOR] §9 · banned word "támogatja" (support) in a non-normative assumption line · spec.md:94
  rule: banned-words — "támogat" tiltott (../_shared/banned-words.md)
  fix: fogalmazd át mérhetően/konkrétan, pl. "a szolgáltató részleges és teljes visszatérítést kínál idempotencia-kulccsal". Nem normatív sor, ezért MINOR.
  resolved: [x]
