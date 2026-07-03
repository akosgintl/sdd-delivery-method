---
artifact: gate
gate: DoD
target: ./ (0003-seat-selection)
round: 1
reviewed: 2026-07-03
verdict: blocked
---

# DoD gate — round 01 (0003-seat-selection)

Evaluated against `../../memory/definition-of-done.md`. The feature has been specified, designed,
and planned but **not built** — so DoD correctly blocks.

## Failing criteria (BLOCKER)

- [BLOCKER] A · no acceptance criterion demonstrated; no code exists · spec.md §6
  fix: implement tasks T-1..T-12, then re-run.
- [BLOCKER] A · every FR/NFR lacks an implemented test referencing its ID · traceability.md Gaps
  fix: land tests `__FR1..__FR9`, `__NFR1..__NFR4`.
- [BLOCKER] B · traceability Gaps section is non-empty (all 13 requirements untested) · traceability.md:38
  fix: build + test until Gaps empty; regenerate the matrix.
- [BLOCKER] B · spec `status: ready`, not `done`; `Verified` boxes all ☐ · spec.md:3
- [BLOCKER] C · no CI run: tests/lint/type/security/coverage unmet (constitution Q-1..Q-4) · —

## Passing so far
- Spec/design/tasks exist, reviewed & approved; DoR passed; ADR-0001 accepted; design maps every
  FR/NFR to a verify path; traceability matrix is forward-complete and drift-free.

**Verdict: blocked** — bounces to the build (tasks T-1..T-12). This is the expected result for an
unbuilt feature; the gate refuses to pass without demonstrated, tested behavior.
