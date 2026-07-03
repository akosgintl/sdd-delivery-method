---
artifact: spec
target: ./spec.md
round: 1
reviewed: 2026-07-03
verdict: approved
---

# Review of ./spec.md — round 01

## Findings

- [MINOR] FR-3 · compound (integer minor units + single currency per event) · spec.md:33
  rule: EARS (../_shared/ears.md) — singular requirement
  fix: optional split into "prices are integer minor units" and "one currency per event". Author's discretion; both are testable as written.
  resolved: [ ]

## Summary

All seven FRs in EARS with `shall`/`shall not`; the money and inventory invariants are quantified and
testable (NFR-2 conservation, NFR-3 integer money, NFR-1 1 s freshness). Every FR and NFR has ≥1
acceptance criterion; the last-unit concurrency edge is covered. Goals and non-goals present.

- BLOCKER: 0
- MAJOR: 0
- MINOR: 1

**Verdict: approved** — round 01. The MINOR is discretionary.
