---
artifact: spec
target: ./spec.md
round: 3
reviewed: 2026-07-04
verdict: changes-requested
reviewer: independent (fresh-context sub-agent — no access to prior reviews or the writer's rationale)
---

# Review of ./spec.md — round 03 (independent)

> Produced by a separate agent given only the spec + the `_shared` rulebook. Round-02 (same-context)
> had *approved* this spec; the independent pass returned changes-requested with a BLOCKER — another
> independence-gap demonstration.

## Findings

- [BLOCKER] §7 · latent open question hedged as "(design detail; flagged here)" while `status: ready` and §10 says "(none)" · spec.md:76
  rule: spec checklist (hedge outside §10 while ready) + conventions (ready ⇒ no unresolved choice)
  fix: resolve as a governing FR — "If an organizer edits a published event's start while a Reservation exists, then the system shall reject the edit." Remove the hedge.
  resolved: [x]

- [MAJOR] FR-6 · compound (display state + enforcement), and its criterion verifies only the enforcement half · spec.md:41
  rule: EARS (split distinct behaviors) + gherkin 1:1
  fix: split into a presentation FR (present as not-yet-on-sale before the window) and an enforcement FR (do not accept ticket purchases outside the window); give each its own criterion.
  resolved: [x]

- [MAJOR] §9 / FR-4, FR-5 · dependency contradiction — FR-4/FR-5 gate on the ticket-type concept (0002), which §9 declares "none upstream" · spec.md:83
  rule: spec checklist (dependencies must be accurate; internal contradiction)
  fix: record `0002` as an upstream conceptual dependency in §9 (the event must test "has ≥1 ticket type") while keeping ticket-type management a non-goal.
  resolved: [x]

- [MINOR] FR-6 · soft verb "present" and an "or" in a normative line · spec.md:42
  rule: EARS (observable response; avoid loose "or")
  fix: split the before-window and after-window cases into determinate FRs.
  resolved: [x]

- [MINOR] §8 · `state` includes `closed` but no FR transitions an event into it · spec.md:80
  rule: spec checklist (every declared state should be reachable/governed)
  fix: add an FR that transitions a published event to `closed` when the on-sale window ends.
  resolved: [x]

- [MINOR] FR-6 · "seat selections" is undefined for a capacity-only event, which §8 supports · spec.md:42
  rule: glossary alignment / coverage
  fix: generalize the prohibition to cover capacity-only admissions, or scope it to seated events and add the capacity-only equivalent.
  resolved: [x]

## Summary

- BLOCKER: 1 · MAJOR: 2 · MINOR: 3 — all resolved by the rewrite (see `spec.review-04.md`).

**Verdict: changes-requested.**
