---
artifact: gate
gate: DoD
target: ./ (0006-refunds-cancellation)
round: 1
reviewed: 2026-07-03
verdict: blocked
---

# DoD gate — round 01 (0006-refunds-cancellation)

Evaluated against `../../memory/definition-of-done.md`. Specified, designed, planned — **not built** —
so DoD correctly blocks.

## Failing criteria (BLOCKER)

- [BLOCKER] A · no acceptance criterion demonstrated; no code exists · spec.md §6
  fix: implement tasks T-1..T-13, then re-run.
- [BLOCKER] A · every FR/NFR lacks an implemented test referencing its ID · traceability.md Gaps
- [BLOCKER] A · NFR-2 (no reversed-but-reserved state) not verified against reality · design.md §7
  fix: land T-10 fault-injection test.
- [BLOCKER] B · traceability Gaps non-empty (all 12 requirements untested) · traceability.md:36
- [BLOCKER] B · spec `status: ready`, not `done` · spec.md:3
- [BLOCKER] C · no CI: tests/lint/security/coverage unmet · —

## Passing so far
- Spec/design/tasks approved; DoR passed; ADR-0002 accepted; design maps every FR/NFR to a verify
  path; matrix forward-complete and drift-free.

**Verdict: blocked** — bounces to the build (tasks T-1..T-13). Expected for an unbuilt feature; the
two SDD-defining clauses (verified-against-spec, spec-reconciled) cannot pass without code + tests.
