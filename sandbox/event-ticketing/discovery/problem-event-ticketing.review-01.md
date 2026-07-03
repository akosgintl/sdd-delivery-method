---
artifact: problem-statement
target: ./problem-event-ticketing.md
round: 1
reviewed: 2026-07-03
verdict: changes-requested
---

# Review of ./problem-event-ticketing.md — round 01

## Findings

- [MAJOR] §Impact + §Success metric · states the funnel as "add-to-cart→paid", naming "cart" — a UI/solution mechanism, and a term the glossary deliberately excludes (the agreed term is Order) · problem-event-ticketing.md:36
  rule: problem-statement checklist — "states a problem, not a solution (no screens/tech named)"; glossary alignment (../_shared/glossary + banned-words)
  fix: phrase the funnel in problem terms — e.g. "of buyers who begin a purchase, 38% never complete payment" and set the metric on "begin-purchase → paid".
  resolved: [x]

- [MINOR] §What the problem is · same "add a ticket to a cart" wording leaks the mechanism into the narrative · problem-event-ticketing.md:22
  rule: problem-statement checklist — solution-in-disguise noun
  fix: "start a purchase" instead of "add a ticket to a cart".
  resolved: [x]

- [MINOR] §User stories · four stories are given but the problem names four pains; the ticket-types/pricing and check-in features have no story here (they surface in the PRD) · problem-event-ticketing.md:60
  rule: problem-statement checklist — stories point to specs
  fix: acceptable — this problem statement scopes the four pains it evidences; note that the PRD carries the full 7-feature breakdown. No change required unless you want a story per feature.
  resolved: [x]  # acknowledged; no change (author's discretion, MINOR)

## Summary

Strong: states a problem before a solution, names a specific who (independent 50–2,000-seat
organizers), evidences impact with numbers (duplicate sales, 38% drop-off, 15–20% lost demand,
6-min manual recovery), a quantified success metric, and explicit scope/out-of-scope. Stories carry
benefit clauses and point to spec folders. One MAJOR: the "cart" mechanism leaks into the funnel
metric and conflicts with the glossary.

- BLOCKER: 0
- MAJOR: 1
- MINOR: 2

**Verdict: changes-requested** — resolve the MAJOR (drop "cart" from the metric/narrative).
