---
artifact: spec
target: ./spec.md
round: 1
reviewed: 2026-07-03
verdict: changes-requested
---

# Review of ./spec.md — round 01

## Findings

- [MAJOR] NFR-1 · "95% of online validation responses within 500 ms" has no verifying acceptance criterion · spec.md:41
  rule: gherkin/acceptance 1:1 (../_shared/gherkin.md)
  fix: add "[ ] 95% of online validations return < 500 ms under door load (NFR-1)." to §6.
  resolved: [x]

- [MINOR] NFR-3 · phrasing bundles two numbers (60 s detection + 15 min reconnection default) · spec.md:47
  rule: banned-words/clarity — one measurable claim per line
  fix: acceptable (both quantified); consider splitting detection-latency from the reconnection-cadence assumption. Author's discretion.
  resolved: [ ]

## Summary

The exactly-once invariant is well handled online (FR-1/FR-2 + NFR-2, 10-device test) and the
CAP-style offline tradeoff is made explicit with a bounded, detectable double-admit window (FR-5/FR-6
+ NFR-3) — a genuine ADR candidate, correctly flagged in §11. Reject paths (already-in, unknown,
refunded) are complete. One MAJOR: the latency NFR is unverified by a criterion.

- BLOCKER: 0
- MAJOR: 1
- MINOR: 1

**Verdict: changes-requested** — add the NFR-1 acceptance criterion.
