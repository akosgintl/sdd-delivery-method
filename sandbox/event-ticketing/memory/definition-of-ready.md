# Definition of Ready (DoR) — Tessera

> Gate a feature spec must pass **before** construction. Team-owned; every item checkable.
> Calibrate depth to risk; the items don't change. [auto] = CI-automatable, [human] = judgment.

A spec is **Ready** when all hold:

## A. Intent is clear
- [ ] The need/story is stated (who/what/why) and traces to `prd-event-ticketing.md` (or is marked standalone). [human]
- [ ] A measurable success signal is named. [human]

## B. The specification is sound
- [ ] A `spec.md` exists with complete front-matter (`id, title, status, owner, created, updated, need, supersedes`). [auto]
- [ ] Every `FR-*` has a stable ID and is singular, unambiguous, and in one of the five EARS patterns (testable). [auto]
- [ ] Every `NFR-*` is quantified with a number/condition — no banned vague words. [auto]
- [ ] Acceptance criteria exist and map 1:1 to requirement IDs. [auto]
- [ ] Edge cases and error behavior covered — including the money/inventory failure paths (double-book, timeout, partial payment). [human]
- [ ] Goals **and** non-goals are both stated. [auto]
- [ ] Open Questions section is empty; no `TBD`. [auto]
- [ ] Contested terms align with `glossary.md` (Hold vs Reservation vs Order, Waitlist vs Queue, etc.). [auto]

## C. Feasible & bounded
- [ ] Small enough to deliver in one iteration (else **split**). [human]
- [ ] Dependencies identified and sequenced (e.g. seat-selection depends on ticket-types). [human]
- [ ] Conforms to `constitution.md`, or a recorded ADR exception exists. [human]

## D. Agreed
- [ ] Spec reviewed by engineering and product owner. [human]
- [ ] Significant technical decisions recorded as ADRs where applicable. [human]

## For AI-implemented work, also:
- [ ] The spec is self-contained (an agent with no tribal knowledge could build it). [human]
- [ ] Constitution + glossary are in the agent's context. [human]
- [ ] Acceptance criteria are mechanically checkable. [auto]
- [ ] Interfaces/contracts the agent must honor are explicit. [human]

> Not Ready is a healthy outcome: **refine, spike, split, or defer** — never "start anyway and
> clarify in flight."
