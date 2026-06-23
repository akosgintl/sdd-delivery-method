# 08 — Roles & Workflow

SDD does not require new job titles. It requires that a few **responsibilities** are explicitly
owned. This chapter maps responsibilities to typical roles, gives a RACI for the core artifacts,
and describes the reviews/ceremonies that operate the method.

## 1. Responsibilities (not necessarily separate people)

| Responsibility | What it entails | Often held by |
|----------------|-----------------|---------------|
| **Need owner** | Owns the problem, value, and acceptance | Product Manager / Business Analyst |
| **Spec author** | Writes the testable specification | PM/BA, architect, or senior engineer |
| **Spec reviewer(s)** | Challenge clarity, testability, feasibility | Eng lead + stakeholder + QA |
| **Designer/architect** | Decides the technical approach; writes design + ADRs | Architect / lead engineer |
| **Implementer** | Builds to the spec; writes tests | Engineers (and/or AI agents) |
| **Verifier** | Confirms implementation satisfies the spec | QA + engineers |
| **Accepter** | Signs off against acceptance criteria | Product owner / stakeholder |
| **Constitution keeper** | Maintains project standing law | Eng leadership / architecture guild |

In a small team one person may hold several; in a regulated org they may be distinct with formal
sign-off. The method cares that each is *owned*, not *who* owns it.

## 2. RACI for the core artifacts

`R`=Responsible, `A`=Accountable, `C`=Consulted, `I`=Informed.

| Artifact | Product | Spec author | Architect | Engineers | QA |
|----------|:------:|:-----------:|:---------:|:---------:|:--:|
| Problem / PRD | A/R | C | I | I | I |
| Specification (`spec.md`) | A | R | C | C | C |
| Technical design / ADRs | I | C | A/R | C | I |
| Task breakdown | I | C | C | A/R | C |
| Tests / acceptance | C | C | I | R | A/R |
| Acceptance (DoD) | A | C | I | R | C |

The notable SDD-specific point: **Product is Accountable for the spec** (it is the contract with
the business) while a **spec author is Responsible** for writing it — and engineering and QA are
**Consulted** so the contract is feasible and testable from the start.

## 3. The workflow, with reviews and gates

```
 Discover ──► Specify ──► [SPEC REVIEW] ──► Design ──► [DESIGN REVIEW] ──► Plan
                                │                                            │
                                └──────────────► ◆ DoR gate ◆ ◄──────────────┘
                                                      │
                                                    Build ──► [CODE REVIEW (vs spec)]
                                                      │
                                                   Verify ──► ◆ DoD gate ◆ ──► Accept ──► Release
                                                      │
                                              (reconcile spec)
```

### Spec review (the highest-leverage review in SDD)
A short, focused review of `spec.md` *before* design/construction. Reviewers ask:
- Is every requirement **testable, singular, unambiguous**?
- Are **non-goals** and **edge/error cases** covered?
- Are **NFRs quantified**?
- Any **open questions**? (Must be driven to zero.)
- Does it **conform to the constitution**?

This review is cheap (it's prose) and catches the expensive mistakes. It is the practice that
most distinguishes SDD from ticket-driven work.

### Design review
Confirms the approach satisfies the spec, surfaces risks, and records decisions as ADRs.

### Code review against the spec
PRs are reviewed not only for code quality but for **conformance to the spec**: do the tests
reference the requirement IDs? Does the behavior match the acceptance criteria? Did the spec get
updated if behavior changed?

## 4. Ceremonies (mapped to the method, not prescribed)

SDD slots into whatever cadence you run. The artifacts give each ceremony a concrete anchor:

| Ceremony | SDD purpose |
|----------|-------------|
| Backlog refinement | Move items toward **Ready**: clarify needs, draft/refine specs. |
| Spec review (new) | Approve specs before construction — the key added touchpoint. |
| Planning | Confirm **DoR**, pull Ready specs, decompose into tasks. |
| Daily sync | Surface spec ambiguities found mid-build; route back to author fast. |
| Review / demo | Demonstrate against the spec's **acceptance criteria**. |
| Retrospective | Tune DoR/DoD and the constitution based on what rotted or stalled. |

You don't need a separate "spec review" *meeting* — it can be an async PR review on the spec.
What matters is that the spec is reviewed and agreed *before* build.

## 5. Workflow with AI agents in the loop

When agents implement, the human roles shift toward **specification and verification**:

```
 Humans:   Discover ─ Specify ─ Review spec ───────────────── Verify vs spec ─ Accept
                                       │                            ▲
 Agents:                               └─► Plan ─ Implement ─ Test ─┘
```

Humans concentrate effort on the upstream (a precise spec) and the downstream (judging output
against that spec). The agent does the mechanical middle. This is why SDD and AI coding are a
natural fit — and why the spec author and verifier responsibilities become *more* valuable, not
less. See [10](10-ai-assisted-sdd.md).

> Continue to [09 — Quality & Traceability](09-quality-and-traceability.md).
