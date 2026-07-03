---
artifact: spec
target: ./spec.md
round: 1
reviewed: 2026-07-03
verdict: changes-requested
---

# Review of ./spec.md — round 01

## Findings

- [MAJOR] §2 Goals · "Support both seated ... and capacity-only events" uses the banned word "support" · spec.md:23
  rule: banned-words (../_shared/banned-words.md)
  fix: replace with an observable phrasing, e.g. "Allocate seats for seated events and remaining-count units for capacity-only events" — FR-7 already states the testable behavior.
  resolved: [x]

- [MINOR] FR-7 · references "FR-1 through FR-6" as a range · spec.md:57
  rule: conventions (../_shared/conventions.md) — prefer explicit behavior over cross-ref chains
  fix: acceptable; the range is unambiguous. Author's discretion.
  resolved: [ ]

## Summary

The concurrency core is excellent: FR-2/FR-5 + NFR-2 pin down no-double-booking with a 1,000-way race
test; hold expiry (FR-4/NFR-3) and per-order limit (FR-8) are testable; the seated/capacity-only fork
is handled (FR-7). Every FR and NFR maps to a criterion. One banned word in the goals.

- BLOCKER: 0
- MAJOR: 1
- MINOR: 1

**Verdict: changes-requested** — remove "support" from §2.
