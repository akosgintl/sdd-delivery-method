# 01 — Principles and the Project Constitution

The method rests on a small set of principles. Everything else in this study — the artifacts,
the gates, the storage layout — is a consequence of these.

## 1. The eight principles of SDD

### P1 — The specification is the source of truth
When code and spec disagree, **one of them is a bug**. Either the code is wrong, or the spec is
out of date and must be corrected. There is no third state in which the disagreement is allowed
to persist silently. This principle is what gives the spec its authority; without it the spec is
just documentation.

### P2 — Specify *what* and *why*, not *how*
The specification describes observable behavior, constraints, and rationale. Implementation
choices belong in the design and the code. (See the altitude table in
[00 §7](00-overview.md#7-calibrating-specification-depth-the-right-altitude).) The *why* matters
as much as the *what*: a spec that records intent lets future readers re-derive decisions when
the context changes.

### P3 — Every requirement must be testable
If you cannot describe how you'd verify a statement, it is not a requirement — it is a wish.
"The system should be fast" is a wish. "95% of search requests complete within 300 ms at 1000
RPS" is a requirement. Testability is the single most important quality gate on a spec.

### P4 — Specifications are living and versioned
A spec is a Git-tracked artifact that evolves with the code. It is reviewed in pull requests,
has a history, and is updated as part of the work that changes behavior — never afterward, never
"someday." A frozen spec rots; a living spec compounds.

### P5 — Traceability is maintained end-to-end
Every requirement traces *up* to a user need or business goal and *down* to the tests that
verify it (and ideally to the code that implements it). This is what lets you answer "why does
this exist?" and "what breaks if I change this?" See [09 — Traceability](09-quality-and-traceability.md).

### P6 — The smallest useful unit is the feature, not the project
SDD specifies **per feature** (or per capability/change), in small self-contained artifact sets,
not in one monolithic document. This keeps specs reviewable, parallelizable, and current. It is
the key difference from waterfall "big spec up front."

### P7 — Two gates make the method real: Ready and Done
A **Definition of Ready** gates *entry* into construction (the spec is clear, testable, agreed,
and feasible). A **Definition of Done** gates *exit* (the implementation is verified against the
spec, tested, documented, and the spec reconciled with reality). Without enforced gates, SDD
degrades into "we wrote some docs."

### P8 — Specify in proportion to risk
Depth and formality scale with the cost of being wrong. A safety-critical rule gets a rigorous,
possibly formal spec; a low-risk UI tweak gets a paragraph. Uniform ceremony for all work is
itself an anti-pattern.

## 2. The Constitution

A recurring, powerful idea in modern SDD (notably in GitHub's Spec Kit) is the **constitution**:
a single, stable, project-level document that states the **non-negotiable principles** every
specification and implementation in the project must honor.

Where a spec answers "what should *this feature* do?", the constitution answers "what is *always*
true of how we build here?" It is the project's standing law, and it is short.

### What goes in a constitution

- **Engineering principles** — e.g. "Library-first: every feature is a standalone library with a
  CLI before it is wired into the app." / "No feature ships without automated tests."
- **Architectural constraints** — e.g. "All inter-service calls go through the API gateway." /
  "No direct database access from the presentation layer."
- **Quality bars** — test coverage expectations, performance budgets, accessibility standard
  (e.g. WCAG 2.2 AA), security baseline.
- **Technology constraints** — approved languages/frameworks, banned dependencies, data-residency
  rules.
- **Process rules** — "Specs are reviewed before construction." / "Every behavior change updates
  its spec in the same PR."
- **Decision-making** — how exceptions to the constitution are granted and recorded (usually via
  an [ADR](03-artifacts.md#adr--architecture-decision-record)).

### Properties of a good constitution

- **Stable** — it changes rarely and deliberately (amendments are themselves reviewed).
- **Short** — if it is long, it is being used as a style guide; split that out.
- **Enforceable** — each clause should be checkable, ideally in CI or review.
- **Universal within scope** — it applies to *every* feature, which is why per-feature specs
  don't need to restate it.

A template is provided at [`templates/constitution.md`](../templates/constitution.md).

## 3. How the principles interlock

```
            P8 risk-calibration  ── scales depth of ──┐
                                                       ▼
P6 feature-sized ──► SPECIFICATION ◄── P2 what/why ──► authored & reviewed
                          │  ▲
       P1 source-of-truth │  │ P4 living/versioned
                          ▼  │
                   P3 testable requirements
                          │
       P5 traceability ───┼─── up to needs / down to tests
                          ▼
              P7  Ready gate ──► build ──► Done gate
                          ▲
            CONSTITUTION (standing law, applies to all of the above)
```

The principles are not independent rules to memorize; they are facets of one idea — *the
specification is a precise, testable, living contract, sized to the work and bounded by standing
project law* — pointed at from several directions.

> Continue to [02 — Lifecycle](02-lifecycle.md).
