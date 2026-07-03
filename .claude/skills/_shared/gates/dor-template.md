# Definition of Ready (DoR)

> Gate a work item must pass **before** entering construction. A team-owned quality bar, not a
> toll booth. Calibrate depth to risk; the items don't change.

A work item is **Ready** when all hold:

## A. Intent is clear
- [ ] The need / user story is stated and understood (who, what, why).
- [ ] Business value or success metric is articulated.
- [ ] It traces to a goal/PRD (or is explicitly standalone).

## B. The specification is sound
- [ ] A `spec.md` exists.
- [ ] Functional requirements have stable IDs and are singular, unambiguous, and **testable**.
- [ ] Non-functional requirements are specified and **quantified** where they apply.
- [ ] Acceptance criteria exist and map to the requirements.
- [ ] Edge cases and error behavior are covered (not just the happy path).
- [ ] Goals **and** non-goals are stated.
- [ ] **No open questions remain.**
- [ ] Vague terms removed/defined; terms align with the glossary.

## C. Feasible & bounded
- [ ] Small enough to estimate and deliver in the normal unit (else **split**).
- [ ] Dependencies identified and available or sequenced.
- [ ] Technical feasibility sanity-checked (spike run if there was real doubt).
- [ ] Conforms to the constitution, or a recorded ADR exception exists.

## D. Agreed
- [ ] Spec reviewed by engineering and the relevant stakeholder(s)/product owner.
- [ ] The team understands it well enough to start.
- [ ] (If applicable) technical approach agreed; significant decisions recorded.

## For AI-implemented work, also:
- [ ] The spec is self-contained (an agent with no tribal knowledge could build it).
- [ ] Constitution / steering rules are in the agent's context.
- [ ] Acceptance criteria are mechanically checkable.
- [ ] Interfaces/contracts the agent must honor are explicit.

> Not Ready is a healthy outcome: **refine, spike, split, or defer** — never "start anyway and
> clarify in flight."
