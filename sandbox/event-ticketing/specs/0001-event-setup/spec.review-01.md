---
artifact: spec
target: ./spec.md
round: 1
reviewed: 2026-07-03
verdict: changes-requested
---

# Review of ./spec.md — round 01

## Findings

- [MAJOR] NFR-3 · WCAG 2.2 AA requirement has no verifying acceptance criterion · spec.md:44
  rule: gherkin/acceptance 1:1 (../_shared/gherkin.md) — every NFR needs ≥1 criterion
  fix: add a §6 checklist line, e.g. "[ ] Organizer setup screens pass an axe WCAG 2.2 AA scan (NFR-3)."
  resolved: [x]

- [MINOR] FR-6 · compound + "or" ("not-yet-on-sale or closed and shall not accept seat selections") · spec.md:37
  rule: EARS (../_shared/ears.md) — prefer singular; "and/or" is a compound smell
  fix: split into FR-6a (present as not-on-sale/closed by time) and FR-6b (shall not accept seat selections outside the window). Author's discretion.
  resolved: [ ]

## Summary

Front-matter complete and legal; all FRs in EARS with `shall`; NFRs quantified; goals and non-goals
both present; §7 covers boundaries (on-sale start==end, publish-at-boundary, timezone). One MAJOR:
the accessibility NFR is unverified by any acceptance criterion.

- BLOCKER: 0
- MAJOR: 1
- MINOR: 1

**Verdict: changes-requested** — add the NFR-3 acceptance criterion.
