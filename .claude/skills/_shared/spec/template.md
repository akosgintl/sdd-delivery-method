---
id: NNNN-feature-slug
title: <concise behavior-focused title>
status: draft            # draft | in-review | ready | in-progress | done | superseded
owner: <handle>
created: YYYY-MM-DD
updated: YYYY-MM-DD
need: <link to PRD/story/problem statement, e.g. docs/product/prd-x.md#section>
supersedes: null
---

# Specification: <title>

> The precise, testable description of intended behavior — the contract.
> Describe **what** and **why**, not **how**.

## 1. Summary & context
<One paragraph: the problem this solves and why now. Link to the originating need/story.>

## 2. Goals
- <The outcomes this feature must achieve.>

## 3. Non-goals (out of scope)
- <What this explicitly does NOT do. Bounding scope is part of the contract.>

## 4. Functional requirements
> Each requirement: a stable ID, singular, unambiguous, testable. EARS notation
> (When / While / Where / If-Then / ubiquitous). See ../_shared/ears.md.

- **FR-1:** When <trigger>, the system shall <observable response>.
- **FR-2:** While <state>, the system shall <response>.
- **FR-3:** If <unwanted condition>, then the system shall <response>.
- **FR-4:** The system shall <ubiquitous always-true behavior>.

## 5. Non-functional requirements
> Quantified. No "fast"/"secure" without numbers. See ../_shared/banned-words.md.

- **NFR-1:** <e.g. 95% of <operation> complete within <N> ms at <load>.>
- **NFR-2:** <e.g. All user-facing screens meet WCAG 2.2 AA.>

## 6. Acceptance criteria / scenarios
> The verifiable definition of satisfaction. Map 1:1 to requirements. Gherkin optional.
> See ../_shared/gherkin.md.

```gherkin
Scenario: <name>            # verifies FR-1
  Given <precondition>
  When <action>
  Then <expected outcome>
```
- [ ] <Criterion verifying FR-2>
- [ ] <Criterion verifying FR-3>

## 7. Edge cases & error behavior
- <Boundary case>: <expected behavior> (FR-?)
- <Failure case>: <expected behavior> (FR-?)

## 8. Data & interfaces
<Schemas/contracts touched, or links to contracts/ files. Or "none".>

## 9. Dependencies & assumptions
- Dependencies: <other features/services/teams>.
- Assumptions: <stated so they can be challenged>.

## 10. Open questions
> MUST be empty before this spec is `ready`.
- [ ] <question> — owner: <who> — needed by: <when>

## 11. Rationale / decisions
<Why key choices were made. Link to ADRs for significant decisions.>
