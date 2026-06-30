# E — Tooling Comparison Matrix

A side-by-side of the notable spec-driven toolchains, to support a build/buy/adopt decision. It
condenses the landscape narrative in [`10 — AI-Assisted SDD`](../10-ai-assisted-sdd.md#3-the-tooling-landscape);
read that for context and caveats. **Tools move fast** — treat the cells as orientation and check
each project's current docs before committing.

> Rule of thumb: the tool is not the method. Every row below reduces to the same stable shape —
> *constitution + per-feature spec → plan → tasks, version-controlled in the repo, humans gating
> Ready and Done.* Pick for fit and exit cost, not for features.

## The matrix

| Dimension | GitHub Spec Kit | Amazon Kiro | Tessl | Plain agent CLI (Claude Code / Cursor / Copilot / Aider) |
|-----------|-----------------|-------------|-------|-----------------------------------------------------------|
| **Form** | Open-source CLI (`specify`) over your agent | Agentic IDE with a spec mode | Spec-centric / "AI-native" framework + registry | General coding agent, no spec scaffolding of its own |
| **Constitution analog** | `constitution.md` under `.specify/` | **Steering** files under `.kiro/steering/` | Spec is the durable source | A `CLAUDE.md` / rules file you maintain by hand |
| **Per-feature artifacts** | `spec.md` → `plan.md` (+ `research`, `data-model`, `contracts/`) → `tasks.md` | `requirements.md` → `design.md` → `tasks.md` | Spec from which code is regenerated | Whatever you choose to keep (use this study's layout) |
| **Requirement style** | Free-form; EARS encouraged | **EARS by default** | Spec-defined | Whatever you instruct |
| **Workflow** | Slash-command sequence (`/constitution`, `/specify`, `/clarify`, `/plan`, `/tasks`, `/analyze`, `/implement`) | Generate → review → execute inside the IDE | Spec → generate/regenerate | You drive each phase by prompt |
| **Repo storage** | `.specify/` + `specs/NNN-feature/` | `.kiro/specs/<feature>/` | Spec registry + repo | This study's `specs/` (Layout A) |
| **Explicit clarify step** | Yes (`/clarify`) | Review loop | Varies | Only if you add one |
| **Strengths** | Tool-agnostic, repo-native, explicit gates | EARS-first, integrated authoring + execution | Spec-as-source regeneration story | Zero new tooling; total control |
| **Watch-outs** | You still own the gates | IDE/ecosystem coupling | Newer, smaller ecosystem | No guardrails unless you build them |

## How to choose

1. **Already committed to an agent?** A plain CLI plus this study's layout gets you 80% of the value
   with zero adoption cost — start there, add tooling only when a gate is hard to enforce by hand.
2. **Want EARS and integrated authoring?** Kiro is the most opinionated out of the box.
3. **Want repo-native, tool-agnostic structure with explicit `/clarify` and `/analyze` gates?**
   Spec Kit.
4. **Betting on spec-as-source regeneration?** Watch Tessl, but weigh ecosystem maturity.

This study's recommended layout (centralized `specs/` tree, **Layout A**) is **deliberately
compatible** with Spec Kit (`.specify/`, `specs/NNN-feature/`) and Kiro (`.kiro/`), so adopting the
method first and a tool later does not strand your artifacts. See
[`05 — Storage & Organization`](../05-storage-and-organization.md).

> The decision that actually matters is not which CLI — it's whether you enforce **Ready** and
> **Done**. A tool with no gates is theatre ([anti-pattern E](../11-adoption-and-antipatterns.md#e-artifacts-without-gates-theater)).

---

Continue to [`F — SDD Maturity Model`](F-maturity-model.md), or return to the
[README](../../README.md) for the full table of contents.
