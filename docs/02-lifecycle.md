# 02 — The SDD Lifecycle

This chapter defines the end-to-end flow of work under SDD: the phases, what each produces, and
the **gates** between them. The two most important gates — Ready and Done — get their own
chapters ([06](06-definition-of-ready.md), [07](07-definition-of-done.md)).

## 1. The flow at a glance

```
 ┌──────────┐   ┌──────────────┐   ┌───────────────┐   ┌──────────┐   ┌──────────┐   ┌──────────────┐
 │ DISCOVER │──►│  SPECIFY     │──►│   DESIGN      │──►│   PLAN   │──►│ BUILD &  │──►│  VERIFY &    │
 │ needs    │   │ requirements │   │  technical    │   │  tasks   │   │ IMPLEMENT│   │  RECONCILE   │
 │          │   │ + spec       │   │  approach     │   │          │   │          │   │              │
 └──────────┘   └──────────────┘   └───────────────┘   └──────────┘   └──────────┘   └──────────────┘
      │                │                   │                │              │                │
   need stmt        spec.md            design.md         tasks.md       code+tests     spec reconciled
   user stories     (the contract)     ADRs                            (verified)       DoD met
      │                │                                                                  │
      └─ Gate 0 ───────┴── ◆ DEFINITION OF READY ◆ ─────────────────────► ... ──► ◆ DEFINITION OF DONE ◆
        (worth doing)        (ready to build)                                       (genuinely complete)
```

SDD is **iterative, not waterfall**. The arrows are the dominant flow, but every phase can send
work backward: design may expose an under-specified case (return to Specify); implementation may
reveal an infeasible requirement (return to Specify/Design with a recorded decision). The
discipline is not "never go back" — it is "when you go back, update the authoritative artifact."

## 2. The phases

### Phase 0 — Discover (needs)
**Goal:** understand the problem in the user's and business's terms, before proposing a solution.

- Elicit user needs, pains, jobs-to-be-done; capture business goals and success metrics.
- Identify stakeholders, constraints, and assumptions.
- Produce a **Need / Problem statement** and, where useful, a Vision/PRD and user stories.
- **Gate 0 (worth doing):** is this problem worth solving now? Is it understood well enough to
  specify? If not, run a discovery spike (see [11](11-adoption-and-antipatterns.md)).

**Outputs:** problem statement, vision/PRD (optional), user stories, success metrics.
Detailed in [04 §1–2](04-from-needs-to-spec.md).

### Phase 1 — Specify (requirements → specification)
**Goal:** turn the need into a precise, testable specification — the contract.

- Derive **requirements** (functional and non-functional) from the needs.
- Write each requirement testably, often using **EARS** or BDD/Gherkin (see [04 §4–5](04-from-needs-to-spec.md)).
- Define acceptance criteria, edge cases, error behavior, and explicit non-goals.
- **Clarify**: surface ambiguities and resolve them with stakeholders *now*. (Spec Kit makes
  this an explicit `/clarify` step; do it whether or not you use a tool.)

**Output:** the **specification** (`spec.md`), the central artifact. Plus updated traceability.

### Phase 2 — Design (technical approach)
**Goal:** decide *how* to satisfy the spec, at the architecture/interface level.

- Choose the approach; record significant choices as **ADRs**.
- Define interfaces/contracts (APIs, schemas, data model), integration points, and the test
  strategy.
- Validate feasibility against the spec; if a requirement is infeasible or disproportionately
  costly, negotiate the spec (and record why).

**Output:** **technical design** (`design.md`), ADRs, interface/contract definitions.

### Phase 3 — Plan (tasks)
**Goal:** decompose the agreed design into ordered, independently verifiable tasks.

- Break work into tasks small enough to estimate and verify; sequence by dependency.
- Map each task to the requirement(s) it advances (traceability).
- Identify what can be parallelized and what is on the critical path.

**Output:** **task breakdown** (`tasks.md`).

> **The Definition of Ready gate sits at the end of Plan / start of Build.** Work does not enter
> construction until the spec is clear and testable, the design is agreed, tasks are defined, and
> the team concurs it is feasible. See [06](06-definition-of-ready.md).

### Phase 4 — Build & Implement
**Goal:** construct the solution so that it satisfies the spec.

- Implement tasks; write tests **derived from the spec's acceptance criteria**.
- Prefer specification → test → code (specification-then-test-driven) so the tests encode the
  contract, not the implementation.
- When reality forces a change to intended behavior, **stop and update the spec** (P1), then
  continue. Do not let code silently diverge.
- If AI agents implement, the spec + design + tasks are their inputs; humans review output
  *against the spec*. See [10](10-ai-assisted-sdd.md).

**Output:** code, tests, updated docs.

### Phase 5 — Verify & Reconcile
**Goal:** prove the implementation satisfies the spec, and make the spec match reality.

- Verify every acceptance criterion (automated where possible); confirm NFRs.
- Reconcile: update the spec/design/ADRs to reflect any decisions made during build.
- Confirm traceability is complete (need → requirement → test all linked).
- **Definition of Done gate.** See [07](07-definition-of-done.md).

**Output:** verified, released increment; reconciled specification; complete traceability.

## 3. The two gates (why they carry the method)

Most teams that "tried SDD and it didn't work" had artifacts but no enforced gates. The gates
are where principles become behavior:

- **Definition of Ready** prevents *building the wrong thing* and *building from ambiguity*. It is
  the moment the spec earns the right to consume engineering time.
- **Definition of Done** prevents *declaring victory prematurely* and *spec rot*. It is the moment
  the work earns the right to be called complete — which explicitly includes the spec still being
  true.

Neither gate is a heavyweight ceremony. Each is a checklist (provided in
[`templates/`](../templates/)) applied at a natural transition, ideally partly automated in CI.

## 4. Mapping the lifecycle to common operating models

| SDD phase | Scrum | Kanban | Spec Kit command | Kiro |
|-----------|-------|--------|------------------|------|
| Discover | Backlog refinement | Upstream/"options" | (PRD input) | (prompt) |
| Specify | Refinement → Ready | "Specify" column | `/specify`, `/clarify` | `requirements.md` |
| Design | Refinement / spike | "Design" column | `/plan` | `design.md` |
| Plan | Sprint planning | "Ready" column | `/tasks` | `tasks.md` |
| Build | Sprint | "In progress" | `/implement` | task execution |
| Verify | Review + increment | "Done" column | `/analyze` + review | verification |

SDD is **operating-model-agnostic**: it specifies *what artifacts and gates* exist, not *what
cadence* you run them on. It drops into Scrum sprints, continuous Kanban flow, or an AI-agent
pipeline equally well.

## 5. Iteration and feedback

SDD does not assume you get the spec right the first time. It assumes you make wrongness
**cheap, visible, and recorded**:

- Discovery spikes feed the spec; the spec is not a prerequisite for *learning*.
- Each phase can return work to an earlier one — *with* an update to the authoritative artifact.
- Post-release learnings (incidents, metrics, user feedback) flow back into the spec, which is
  why specs are living (P4).

> Continue to [03 — Artifacts](03-artifacts.md).
