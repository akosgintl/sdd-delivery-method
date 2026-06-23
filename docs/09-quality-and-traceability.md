# 09 — Quality & Traceability

Traceability is the connective tissue of SDD: the chain that links **why** (need) → **what**
(requirement) → **how-verified** (test) → **what-built** (code). It is what lets you answer two
questions that are otherwise unanswerable from a repo:

1. **Forward:** "Is every requirement actually implemented and tested?" (coverage)
2. **Backward:** "Why does this code exist, and what breaks if I change this requirement?"
   (impact analysis)

## 1. The traceability chain

```
 NEED / PRD          REQUIREMENT          ACCEPTANCE          TEST              CODE
 (Phase 0)           spec.md FR-3         scenario            test id           module/fn
 ──────────  ──────► ──────────────  ───► ──────────────  ──► ────────────  ──► ──────────
 "cart loss"         FR-3: restore        "restored after   test_cart_        cart.restore()
 PRD §2.1            cart on reopen        crash"            restore_FR3
        ▲                  │                                       │                 │
        └── traces up ─────┘                  traces down ─────────┴─────────────────┘
```

Each link is realized by a **stable ID referenced across artifacts** — the lightest mechanism
that actually works (see [05 §5](05-storage-and-organization.md#5-linking-and-traceability-mechanics)).

## 2. Two ways to maintain traceability

### Lightweight (recommended default): traceability via IDs
No separate matrix to maintain by hand. Instead:
- Requirements have stable IDs (`FR-3`, `NFR-1`).
- The spec's front-matter links **up** to the need/PRD.
- Tasks and tests reference requirement IDs **down**.
- A small script harvests these references and *generates* the matrix/coverage view on demand.

This keeps traceability **a by-product of normal work** rather than a document someone must
remember to update — which is the only way it survives contact with a real team.

### Formal: an explicit traceability matrix
For regulated/safety/audited contexts, maintain an explicit matrix artifact
([`templates/traceability-matrix.md`](../templates/traceability-matrix.md)) with sign-off
columns. It's more overhead but produces audit evidence. Use it only where compliance requires
it — [P8](01-principles.md#p8--specify-in-proportion-to-risk).

## 3. What good traceability enables

- **Coverage audits.** "Show me requirements with no test." A gap is either a missing test or a
  requirement that shouldn't exist.
- **Impact analysis.** "If we change FR-3, which tests, tasks, and modules are affected?"
- **Change justification.** Every requirement points to a need; orphan requirements (no need) are
  candidates for deletion — scope you're carrying for no reason.
- **Incident forensics.** "What was the spec for this behavior when we shipped v2.3?" answered via
  Git history + IDs.
- **Onboarding.** New people (and agents) follow the chain from code back to intent.

## 4. Verification: proving the spec is satisfied

Traceability tells you *what* to verify; verification *does* it. SDD's verification hierarchy:

| Requirement type | Primary verification |
|------------------|----------------------|
| Functional (FR) | Automated acceptance/integration tests referencing the FR ID |
| Non-functional (NFR) | Measurement against the stated number (load test, security scan, a11y audit, etc.) |
| Interface/contract | Contract tests against the schema (OpenAPI, Pact, etc.) |
| Business rule / invariant | Property-based or example tests; sometimes formal checks |

### Tests reference requirements
Adopt a convention so coverage is machine-checkable. Examples:
- Test name: `test_cart_restored_after_crash__FR3`
- Tag/annotation: `@requirement("0001/FR-3")`
- Gherkin tag: `@FR-3` on the scenario.

A CI script then asserts: *every `FR-*`/`NFR-*` in a `ready` or `done` spec is referenced by at
least one test.* This single check turns [P3](01-principles.md#p3--every-requirement-must-be-testable)
and [P5](01-principles.md#p5--traceability-is-maintained-end-to-end) from aspirations into gates.

## 5. CI checks that enforce quality (suggested)

| Check | Enforces |
|-------|----------|
| Spec front-matter valid | Storage conventions ([05](05-storage-and-organization.md)) |
| No open questions / `TBD` in `ready`/`done` specs | DoR/DoD cleanliness |
| Every `FR-*`/`NFR-*` referenced by ≥1 test | Testability + coverage |
| Behavior-bearing code change touches a `spec.md` (warn) | Anti spec-rot ([07](07-definition-of-done.md)) |
| ADRs append-only (no edits to `accepted` decisions) | Decision integrity |
| `specs/README.md` index regenerated and committed | Discoverability |
| Banned-vague-words linter on specs | Requirement quality ([04 §3.3](04-from-needs-to-spec.md#33-words-to-ban-or-define)) |

Start with the coverage check (item 3) — it delivers the most quality per unit of effort.

## 6. Metrics for the method itself

Track a few signals to know whether SDD is healthy (not to weaponize against the team):

- **Spec coverage:** % of requirements with a verifying test (target: 100% for `done`).
- **Spec freshness:** age since `updated` vs. last behavior change in the feature's code.
- **Clarification rate:** ambiguities found *during build* per feature — high means DoR is weak.
- **Rework rate:** PRs reverted/redone due to misunderstood intent — should fall under SDD.
- **Lead-time split:** time in Specify/Design vs. Build — to see if you're over- or
  under-investing upstream.

A *rising* clarification/rework rate is the early-warning sign that the Ready gate has gone soft.

> Continue to [10 — AI-Assisted SDD](10-ai-assisted-sdd.md).
