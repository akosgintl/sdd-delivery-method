---
artifact: spec
target: ./spec.md
round: 1
reviewed: 2026-07-03
verdict: changes-requested
---

# Review of ./spec.md — round 01

## Findings

- [MAJOR] NFR-4 · WCAG 2.2 AA requirement has no verifying acceptance criterion · spec.md:48
  rule: gherkin/acceptance 1:1 (../_shared/gherkin.md)
  fix: add "[ ] Checkout screens pass an axe WCAG 2.2 AA scan (NFR-4)." to §6.
  resolved: [x]

- [MINOR] FR-5 · three-clause compound (reject + not charge + report lost seats) · spec.md:37
  rule: EARS (../_shared/ears.md)
  fix: acceptable as a compound unwanted-behavior requirement; split only if the report becomes its own behavior. Author's discretion.
  resolved: [ ]

## Summary

Price-first flow (FR-1), single-charge idempotency (FR-4/NFR-2), expired-hold guard (FR-5), and PSP
decline/timeout handling (FR-6) are all in testable EARS form. No-PAN is stated as both FR-8 and
NFR-3 with a scan check. One MAJOR: the accessibility NFR lacks an acceptance criterion.

- BLOCKER: 0
- MAJOR: 1
- MINOR: 1

**Verdict: changes-requested** — add the NFR-4 acceptance criterion.
