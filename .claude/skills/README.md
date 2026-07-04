# SDD Agent Skills — the executable layer

This directory turns the study's *craft layer* into **runnable Agent Skills**: writer / reviewer /
rewriter (and generator, gate-runner, maintainer, loop-driver) skills that draft, critique, and
repair the SDD artifacts. Where `docs/` explains *what* an artifact is, `templates/` gives a blank
skeleton, `how-to/` teaches *how to write a good one*, and `examples/` shows one filled in — these
skills **do the writing and the reviewing**.

The skills conform to the [Agent Skills spec](https://agentskills.io/specification): each is a
folder with a `SKILL.md` (name = folder, keyword-rich description). They are **self-contained** —
all the knowledge they need (EARS rules, banned words, checklists, template skeletons, gold
examples, the review-file format) is bundled under [`_shared/`](_shared/) and referenced with
`../_shared/…`, so this tree does not depend on the study's `docs/`/`templates/`/`how-to/`/`examples/`
folders.

## The 26 skills

| Artifact | Writer | Reviewer | Rewriter / other |
|----------|--------|----------|------------------|
| **spec** | `spec-writer` | `spec-reviewer` | `spec-rewriter` |
| **design** | `design-writer` | `design-reviewer` | `design-rewriter` |
| **tasks** | `tasks-writer` | `tasks-reviewer` | `tasks-rewriter` |
| **traceability** | `traceability-writer` *(generator)* | `traceability-reviewer` *(audit)* | — *(regenerate + escalate)* |
| **PRD** | `prd-writer` | `prd-reviewer` | `prd-rewriter` |
| **problem statement** | `problem-statement-writer` | `problem-statement-reviewer` | `problem-statement-rewriter` |
| **constitution** | `constitution-writer` *(bootstrap/amend)* | `constitution-reviewer` | — |
| **ADR** | `adr-writer` *(+ supersede)* | `adr-reviewer` | — *(append-only)* |
| **glossary** | `glossary-writer` *(seed/promote)* | `glossary-maintainer` *(linter)* | — |
| **gates (DoR/DoD)** | `gates-writer` | `gate-runner` *(checkpoint)* | — |
| **orchestration** | — | — | `sdd-loop` *(drives the inner loop)* |

Acceptance criteria, EARS, and NFRs have **no standalone skills** — they're folded into the spec
skills via `_shared/ears.md`, `_shared/gherkin.md`, `_shared/banned-words.md`.

Why not writer/reviewer/rewriter for *everything*? The skill shape matches the artifact's nature:
generated artifacts (traceability) get a generator + audit and fix upstream; append-only artifacts
(ADR, constitution) get writer + reviewer and change by superseding/amending; gates are a pass/blocked
checkpoint, not a review loop; the glossary is a living reference with a maintainer-linter.

## The workflow (runbook)

Full rules live in [`_shared/workflow.md`](_shared/workflow.md). In brief:

```
P0 Foundations (once/project): constitution (writer⟲reviewer) · glossary-writer(seed) · gates-writer
P1 Discovery (per initiative): problem-statement [loop] → prd [loop]
P2 Spec (per feature):         spec [loop] → glossary-maintainer(scan) → ══ DoR gate ══
P3 Design (per feature):       design [loop] + adr(writer⟲reviewer per decision)
P4 Plan (per feature):         tasks [loop]
P5 Trace/verify (per feature): traceability-writer(gen) → traceability-reviewer(audit)
                               → build (outside skills) → ══ DoD gate ══
```

**Inner loop** (`sdd-loop` drives it): `writer → (reviewer ⇄ rewriter)* → approved`. A review writes
the next `<artifact>.review-NN.md`; approval means no unresolved BLOCKER/MAJOR; MINOR is the author's
discretion. The loop caps at **3 rounds**, then surfaces the open review to a human — it never
fabricates approval.

**Outer loops:** a reviewer whose root cause is upstream re-opens the upstream loop and ripples down;
a traceability coverage gap is fixed in spec/tasks then the matrix regenerates; a gate failure bounces
to the offending artifact's loop; changing an upstream artifact marks downstream ones stale for
re-review.

## `_shared/` — the bundled knowledge

`review-format.md` (findings schema + severities + `review-NN` sequencing), `workflow.md`,
`conventions.md` (IDs, status vocab, test-naming), `ears.md`, `banned-words.md`, `gherkin.md`,
`SYNC.md` (the `_shared` ↔ study sync map — which rule mirrors which `docs/` section), and a
`<artifact>/{template,checklist,example}.md` set per artifact (plus `gates/`).

> **Sync obligation.** `_shared/` deliberately *duplicates* rules that also live in the study's
> `docs/`/`templates/`/`how-to/`/`examples/`, in exchange for portability. When a rule changes in the
> study (e.g. the banned-word list in `docs/04 §3.3`), update its `_shared/` copy too.
