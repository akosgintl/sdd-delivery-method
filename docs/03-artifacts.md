# 03 — The Artifact Catalog

This chapter is the reference for **every artifact SDD produces**: what it is, what it contains,
who owns it, when it is created, and how it relates to the others. Templates for the core
artifacts live in [`templates/`](../templates/); a fully worked set lives in
[`examples/`](../examples/).

## 1. The artifact map

```
                         ┌─────────────────────┐
                         │   CONSTITUTION       │  project-level, stable, applies to all
                         │   (standing law)     │
                         └─────────┬───────────┘
                                   │ governs
   USER NEEDS                      ▼
 ┌──────────────┐        ┌─────────────────────┐
 │ Problem stmt │        │  per-FEATURE bundle  │
 │ Vision / PRD │──────► │                      │
 │ User stories │ feeds  │  spec.md  (contract) │◄───┐
 └──────────────┘        │  design.md + ADRs    │    │ verified
        │                │  tasks.md            │    │ against
        │ traces to      │  test-spec / scenarios│   │
        ▼                └──────────┬───────────┘    │
 ┌──────────────────────────────────┘                │
 │  TRACEABILITY MATRIX  need → requirement → test ───┘
 └─────────────────────────────────────────────────
                                   │ gates
                         ┌─────────┴───────────┐
                         │  DoR  /  DoD          │  checklists (project-level)
                         └─────────────────────┘
```

## 2. Project-level artifacts (one per project)

### Constitution
- **What:** the non-negotiable principles, constraints, and quality bars every feature must honor.
- **Contains:** engineering principles, architectural constraints, quality/security/perf bars,
  technology constraints, process rules, amendment procedure.
- **Owner:** engineering leadership / architecture (amended deliberately).
- **Lifecycle:** created once; amended rarely via reviewed change.
- **Template:** [`templates/constitution.md`](../templates/constitution.md). See [01 §2](01-principles.md#2-the-constitution).

### Definition of Ready (DoR) / Definition of Done (DoD)
- **What:** the two gate checklists.
- **Owner:** the whole team (agreed and owned collectively).
- **Lifecycle:** stable; revised in retrospectives.
- **Templates:** [`templates/definition-of-ready.md`](../templates/definition-of-ready.md),
  [`templates/definition-of-done.md`](../templates/definition-of-done.md).
- **Detail:** [06](06-definition-of-ready.md), [07](07-definition-of-done.md).

### Glossary / Ubiquitous language
- **What:** agreed definitions of domain terms, shared by specs, code, and tests.
- **Why:** ambiguity in terms is ambiguity in the spec. A shared glossary is a force multiplier
  for every other artifact (and for AI agents).
- **Owner:** product + engineering jointly.

## 3. Discovery artifacts (needs → intent)

### Problem / Need statement
- **What:** a crisp statement of the user/business problem, independent of solution.
- **Contains:** who has the problem, what the problem is, the impact/cost of not solving it,
  success metrics, constraints, assumptions.
- **Owner:** product / business analyst.
- **When:** Phase 0 (Discover).

### Vision / PRD (Product Requirements Document)
- **What:** the product-level "why and what" for a larger initiative — context, goals, target
  users, scope, success metrics, non-goals.
- **Owner:** product management.
- **When:** for initiatives larger than a single feature. Optional for small changes.
- **Note:** a PRD is *upstream* of specs. One PRD typically spawns several feature specs. Keep
  the PRD at the "why/what for the product" altitude; keep behavioral precision in the specs.

### User stories
- **What:** user-centered statements of value — *"As a `<role>`, I want `<capability>` so that
  `<benefit>`."*
- **Contains:** the story, its acceptance criteria, and a link to the spec that formalizes it.
- **Owner:** product, refined with the team.
- **Note:** stories express *desire and value*; specs express *precise behavior*. A story is the
  bridge from need to specification, not a replacement for it.

## 4. The per-feature bundle (the heart of SDD)

Each feature/capability/change gets a small, self-contained bundle. This is the unit that flows
through the lifecycle.

### `spec.md` — the Specification *(the central artifact)*
- **What:** the precise, testable description of intended behavior — the contract.
- **Contains (typical):**
  - Summary and context; link to the need/story it satisfies.
  - **Functional requirements**, each testable, often in EARS or Gherkin.
  - **Non-functional requirements** (performance, security, accessibility, reliability, etc.).
  - **Acceptance criteria** (the conditions of satisfaction).
  - **Edge cases and error behavior** (often the highest-value content).
  - **Explicit non-goals / out of scope.**
  - **Open questions** (must be empty to be Ready).
  - Assumptions, dependencies, and rationale (the *why*).
- **Owner:** the spec author (PM/BA/architect/senior engineer), reviewed by stakeholders + eng.
- **When:** Phase 1; updated whenever behavior changes (P4).
- **Template:** [`templates/specification.md`](../templates/specification.md).
- **Detail:** [04](04-from-needs-to-spec.md).

### `design.md` — the Technical Design
- **What:** how the team intends to satisfy the spec.
- **Contains:** chosen approach and alternatives considered, component/interface/data-model
  design, integration points, the test strategy, risks, and links to ADRs.
- **Owner:** architect / lead engineer.
- **When:** Phase 2.
- **Template:** [`templates/technical-design.md`](../templates/technical-design.md).

### ADR — Architecture Decision Record
- **What:** a short, immutable record of one significant decision: context, decision,
  alternatives, consequences, status.
- **Why separate from design:** ADRs are *append-only history*; the design doc is the *current*
  picture. ADRs answer "why did we decide this, back then?"
- **Owner:** whoever makes the decision.
- **When:** whenever a significant or hard-to-reverse choice is made (incl. exceptions to the
  constitution).
- **Template:** [`templates/adr.md`](../templates/adr.md).

### `tasks.md` — the Task Breakdown
- **What:** the ordered, dependency-aware decomposition of the design into verifiable tasks.
- **Contains:** task list with IDs, dependencies, the requirement(s) each task advances, and
  (optionally) estimates.
- **Owner:** the delivery team.
- **When:** Phase 3.
- **Template:** [`templates/tasks.md`](../templates/tasks.md).

### Test specification / acceptance scenarios
- **What:** the concrete, executable expression of the acceptance criteria — Gherkin scenarios,
  contract tests, or a test plan that maps 1:1 to spec requirements.
- **Why:** this is where the spec becomes *verifiable*. In mature SDD this is partly the same
  artifact as the spec (BDD style) and partly automated tests in the codebase.
- **Owner:** QA + engineering.
- **When:** authored alongside or immediately after the spec (Phase 1–2), executed in Phase 4–5.

### Supporting artifacts (Spec Kit-style, optional but useful)
Larger or research-heavy features often add:
- `research.md` — findings, spikes, and options analysis feeding the spec/design.
- `data-model.md` — entities, relationships, and constraints.
- `contracts/` — API/interface contracts (e.g. OpenAPI files, schemas).
- `quickstart.md` — how to run/verify the feature; doubles as living documentation.

## 5. Cross-cutting artifact: the Traceability Matrix
- **What:** the mapping that links **need → requirement → spec section → test → (code)**.
- **Why:** it is the connective tissue that makes [P5](01-principles.md#p5--traceability-is-maintained-end-to-end)
  real — enabling impact analysis ("what breaks if I change this?") and coverage audits ("is
  every requirement tested?").
- **Form:** can be a literal matrix file, or emergent from stable IDs cross-referenced across
  artifacts (the lighter, recommended approach for most teams — see [09](09-quality-and-traceability.md)).
- **Template:** [`templates/traceability-matrix.md`](../templates/traceability-matrix.md).

## 6. Artifact summary table

| Artifact | Scope | Phase | Primary owner | Lives in | Living? |
|----------|-------|-------|---------------|----------|---------|
| Constitution | Project | — | Eng leadership | `/.specify/` or `/docs/` root | Rarely changes |
| DoR / DoD | Project | — | Team | `/docs/` root | Stable |
| Glossary | Project | — | Product + Eng | `/docs/` root | Grows |
| Problem statement | Initiative | 0 | Product | feature folder | Snapshot |
| Vision / PRD | Initiative | 0 | Product | `/docs/product/` | Living |
| User stories | Feature | 0 | Product | feature folder / tracker | Snapshot→ |
| **`spec.md`** | **Feature** | **1** | **Spec author** | **feature folder** | **Living** |
| `design.md` | Feature | 2 | Architect/lead | feature folder | Living |
| ADR | Decision | 2 | Decider | `/docs/adr/` | Immutable |
| `tasks.md` | Feature | 3 | Team | feature folder | Living→done |
| Test spec / scenarios | Feature | 1–5 | QA + Eng | feature folder / tests | Living |
| Traceability | Cross | all | Team | matrix or via IDs | Living |

## 7. The minimum viable artifact set

You do **not** need all of the above for every change. The irreducible core is:

1. A **need/story** (why we're doing this), and
2. A **spec** with testable acceptance criteria (what "done" means), and
3. The two **gates** (DoR, DoD) applied to it.

Everything else (PRD, separate design doc, ADRs, formal traceability matrix) is added *as the
risk and size of the work justify it* — [P8](01-principles.md#p8--specify-in-proportion-to-risk).
A one-line bug fix may collapse the spec into the PR description; a payment subsystem warrants
the full bundle.

> Continue to [04 — From Needs to Specification](04-from-needs-to-spec.md).
