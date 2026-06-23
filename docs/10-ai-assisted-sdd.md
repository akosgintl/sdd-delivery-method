# 10 — AI-Assisted SDD

Specification-Driven Development predates AI coding agents by decades, but the rise of capable
agents (2024 onward) is what made "SDD" a widely-used term. The reason is simple:

> **When a machine writes the code, the specification becomes the highest-leverage thing a human
> can author** — it is both the *prompt of record* for the agent and the *yardstick* for judging
> its output.

This chapter covers how SDD operates with agents, surveys the notable tooling, and gives the
practices that keep agent-built systems faithful to intent. The method is tool-agnostic; the
tools are one way to operate it.

## 1. Why SDD and AI agents fit

Agents are extraordinary at the **mechanical middle** (turning a clear contract into code and
tests) and dangerous at the **ambiguous ends** (they fill gaps with confident guesses rather than
asking). SDD plays exactly to this: it concentrates human effort on producing an unambiguous
contract and on verifying output against it, and hands the agent the part it's good at.

```
 Human leverage ▲
                │  SPECIFY            VERIFY
                │  (precise spec)     (judge vs spec)
                │      ╲                 ╱
                │       ╲               ╱
                │        ╲             ╱
 Agent leverage │         ▼  IMPLEMENT ▼
                │            (spec → code → tests)
                └──────────────────────────────────►
```

Without a spec, an agent optimizes for *plausible*; with a spec, it optimizes for *correct as
defined*. The spec is what converts "vibe coding" into engineering.

## 2. The agent-operated lifecycle

The phases from [02](02-lifecycle.md) map onto an agent workflow with humans at the gates:

```
 Human writes/reviews spec ─► DoR gate ─► Agent plans ─► Agent implements + tests
        ▲                                                        │
        │                                                        ▼
   reconcile spec ◄── DoD gate ◄── Human verifies output against the spec
```

The two human gates — **DoR** (is the spec good enough to hand an agent?) and **DoD** (did the
output satisfy the spec?) — are where quality is won. See the AI-specific additions in
[06 §6](06-definition-of-ready.md#6-dor-for-ai-assisted-sdd) and
[07 §6](07-definition-of-done.md#6-dod-for-ai-assisted-sdd).

## 3. The tooling landscape

> Tools evolve fast; treat the specifics below as orientation and check each project's current
> docs. The *shape* — constitution + per-feature spec/plan/tasks, stored in the repo — is stable
> across all of them and matches this study's [storage layout](05-storage-and-organization.md).

### GitHub Spec Kit
An open-source toolkit (the `specify` CLI) that structures spec-driven work for coding agents
(works with Claude Code, Copilot, Cursor, Gemini CLI, and others). Its workflow is a sequence of
slash commands, each producing a repo artifact:

| Command | Produces | Purpose |
|---------|----------|---------|
| `/constitution` | `constitution.md` | Establish project principles (standing law). |
| `/specify` | `spec.md` | State *what* and *why* (no implementation). |
| `/clarify` | (updates spec) | Drive open questions to zero before planning. |
| `/plan` | `plan.md` (+ `research.md`, `data-model.md`, `contracts/`, `quickstart.md`) | Decide *how*. |
| `/tasks` | `tasks.md` | Decompose into ordered, verifiable tasks. |
| `/analyze` | (report) | Cross-check consistency/coverage across artifacts. |
| `/implement` | code + tests | Execute the tasks against the spec. |

Artifacts live under `.specify/` (project assets incl. the constitution) and numbered
`specs/NNN-feature/` folders. This study's recommended layout is deliberately compatible.

### Amazon Kiro
An agentic IDE with a **spec mode** that generates, per feature under `.kiro/specs/<feature>/`:
- `requirements.md` — user stories with acceptance criteria in **EARS** notation,
- `design.md` — technical design,
- `tasks.md` — implementation tasks.
Project-level guidance lives in **steering** files under `.kiro/steering/` (the constitution
analog). Kiro popularized EARS as the default requirement format for agent workflows.

### Others
- **Tessl** — a spec-centric / "AI-native" framework and registry positioning the spec as the
  durable source from which code is regenerated.
- **General agent CLIs** (Claude Code, Cursor, Copilot, Gemini CLI, Aider) — can operate SDD
  without a dedicated tool: keep a constitution and per-feature specs in the repo and instruct the
  agent to plan, implement, and test *against the spec*, then verify.

The common denominator — and the part worth internalizing — is **not** any CLI; it is:
*constitution (standing rules) + per-feature spec → plan → tasks, all version-controlled in the
repo, with humans gating Ready and Done.*

## 4. Practices that keep agent-built systems faithful

1. **Make the spec self-contained.** The agent has no tribal knowledge. If it isn't written, it
   doesn't exist for the agent. (DoR clause.)
2. **Put standing rules where the agent reads them** — a constitution / steering / `CLAUDE.md`
   so every generation honors the same constraints without re-prompting.
3. **Force clarification before code.** Use an explicit clarify step; an agent that asks first
   beats one that guesses. Drive open questions to zero (DoR).
4. **Spec → tests → code.** Have the agent derive tests from acceptance criteria *first*, so tests
   encode the contract, not the implementation. Watch for the failure mode where generated tests
   merely assert whatever the code does — those verify nothing.
5. **Verify against the spec, not against vibes.** A human (or a separate adversarial agent)
   checks output against each acceptance criterion. Agent confidence is not evidence (DoD).
6. **Reconcile.** When the agent makes a behavior decision mid-build, update the spec in the same
   change, or correct the code. Never let generated code silently redefine intent.
7. **Keep specs small.** Per-feature specs fit an agent's context and keep generations focused —
   another reason [P6](01-principles.md#p6--the-smallest-useful-unit-is-the-feature-not-the-project)
   matters more, not less, with agents.

## 5. Risks specific to AI-assisted SDD

| Risk | Mitigation |
|------|------------|
| Agent fills ambiguity with confident guesses | Strong DoR; explicit clarify step; self-contained spec. |
| Tests mirror implementation, not spec | Spec→test-first; human review of tests vs. acceptance criteria. |
| Spec rot accelerates (agent edits code fast) | DoD reconcile clause; CI spec-touch check. |
| Over-trust of fluent output | Mandatory human verification against the spec; adversarial review. |
| Constitution ignored across generations | Keep it in the agent's standing context; CI-enforce its checkable clauses. |
| Volume of plausible code outpaces review | Smaller specs/tasks; automate mechanical DoD checks to free human attention for judgment. |

## 6. The strategic point

AI agents do not make specification *optional* — they make it *decisive*. The teams that get
leverage from agents are the ones that can hand them an unambiguous, testable contract and judge
the result against it. That is precisely the SDD discipline. The method is the same one
requirements engineers have advocated for decades; agents simply raised the payoff for doing it
well and the cost of doing it badly.

> Continue to [11 — Adoption & Anti-Patterns](11-adoption-and-antipatterns.md).
