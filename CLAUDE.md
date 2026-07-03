# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## What this repository is

This is a **prose-and-templates repository**, not a software project. It is a study of
**Specification-Driven Development (SDD)** as a delivery method, plus a copy-ready toolkit. There
is no build, lint, or test step — the "source" is Markdown. Changes are validated by reading,
not by running anything.

## Structure and how the parts relate

The four top-level content directories form a study → toolkit → craft → demonstration chain, and
they cross-reference each other heavily:

- `docs/00..11` — the numbered study, meant to be read in order. Each chapter ends with a
  `> Continue to [next]` link and links *down* to the templates/examples it describes.
- `docs/appendices/` — appendices to the study, **letter-numbered** (`A`, `B`, … not chapter numbers)
  so the folder reads as a self-contained set rather than orphaned chapters 12+. `A-glossary.md` (the
  20 most-frequent entities, defined and cross-linked) and `B-property-graph.md` (those 20 plus `Task`,
  re-expressed as a Neo4j-style property graph with node properties, relationships, Cypher, and a
  Mermaid diagram) are grounded strictly in the study text; if you change a definition in `A`, keep
  `B`'s nodes and edges in sync. `C-scaling-to-the-enterprise.md` is an applied annex (team topology,
  personas, governance for multi-squad, AI-central orgs); `D-ears-cheat-sheet.md`,
  `E-tooling-comparison.md`, `F-maturity-model.md`, and `G-agent-prompt-library.md` are reference
  cards. Appendix files use `../` to reach study chapters and `../../` to reach `README.md` /
  `templates/`; the `README.md` chapter table indexes them as rows `A`–`G`.
- `templates/` — copy-ready artifact templates (spec, design, tasks, constitution, ADR, DoR/DoD,
  glossary, traceability, vision/PRD). Each template's README row links *up* to the doc chapter
  that explains it.
- `how-to/` — the **craft layer**: recipe-style "how to write a good X" guides for every
  human-authored artifact. Filenames are verb-first and unnumbered (`write-ears-requirements.md`).
  Each guide follows a fixed 8-section recipe (why it matters, when you write it, anatomy, the
  recipe, a before→after rewrite, smell test, checklist, links) and **cross-links to** its concept
  chapter and template rather than restating them — do not duplicate `docs/04` etc. into a guide.
  `how-to/README.md` is the lifecycle-ordered index. All twelve guides ship: problem-statement, PRD,
  constitution, glossary, EARS, NFRs, acceptance-criteria, spec, technical-design, tasks,
  traceability-matrix, DoR/DoD.
- `examples/specs/0001-cart-persistence/` — one fully worked feature bundle (`spec.md`,
  `design.md`, `tasks.md`, `traceability.md`) demonstrating every artifact populated. The
  cart-persistence example is the canonical worked example referenced throughout `docs/04`.
- `.claude/skills/` — the **executable layer**: 26 [Agent Skills](https://agentskills.io/specification)
  (`<artifact>-writer/reviewer/rewriter` for spec/design/tasks/prd/problem-statement; writer+reviewer
  for constitution/adr; writer+generator-audit for traceability; writer+maintainer for glossary;
  gates-writer+gate-runner; and an `sdd-loop` driver). Skills are **self-contained** — they never
  reference `docs/`/`templates/`/`how-to/`/`examples/`; all bundled knowledge lives in
  `.claude/skills/_shared/` (`review-format.md`, `workflow.md`, `conventions.md`, `ears.md`,
  `banned-words.md`, `gherkin.md`, and a `<artifact>/{template,checklist,example}.md` set), reached
  with `../_shared/…`. Reviewers emit sequence-numbered `<artifact>.review-NN.md` findings; rewriters
  apply and check them off. `_shared/` **deliberately duplicates** rules that also live in the study
  (the one place this repo breaks "cross-links over duplication", for portability) — **sync
  obligation:** when a rule changes in `docs/` (e.g. the banned-word list, EARS patterns, a status
  vocabulary), update its `_shared/` copy too. `.claude/skills/README.md` is the runbook + index.

`README.md` is the front door: its table maps each chapter number to the question it answers. Keep
that table and the per-chapter "Continue to" links consistent when adding or reordering docs.

## Conventions to preserve when editing

These are the rules the content itself teaches and must follow:

- **EARS requirement syntax** (`docs/04` §4): functional requirements use one of five patterns —
  ubiquitous, event-driven (`When`), state-driven (`While`), optional (`Where`),
  unwanted-behavior (`If/Then`) — and always use **shall** / **shall not**. When writing or
  editing example requirements, keep them in EARS form and keep them testable (a pass/fail check
  must be writable).
- **Banned vague words** (`docs/04` §3.3): avoid "fast, easy, robust, secure, scalable, support,
  handle, etc., and/or, user-friendly" in any normative statement; quantify NFRs with numbers.
- **Stable IDs, never renumbered**: requirements are `FR-<n>`/`NFR-<n>` scoped to a feature;
  globally referenced as `0001/FR-3`. Feature folders are `NNNN-kebab-case-slug` (zero-padded).
  ADRs are `NNNN-short-title.md`, globally numbered, append-only (superseded, never edited/deleted).
- **Spec front-matter**: specs carry YAML front-matter (`id, title, status, owner, created,
  updated, need, supersedes`). Status vocabulary is fixed: `draft → in-review → ready →
  in-progress → done → superseded`.
- **Cross-links over duplication**: a concept is explained in one chapter and linked from others;
  do not restate it. Templates link to their explaining doc; docs link to their template and to
  the worked example. Preserve these links when moving content.
- **Layout A is the recommended default** repo layout (centralized `specs/` tree); the docs are
  deliberately compatible with GitHub Spec Kit (`.specify/`, `specs/NNN-feature/`) and Amazon
  Kiro (`.kiro/`). Keep that compatibility claim accurate if you change layout guidance.

## Style

Match the existing voice: opinionated, second person, dense ASCII diagrams for flows, Markdown
tables for comparisons, and frequent `> blockquote` rules of thumb. Lines are wrapped at ~100
columns. New chapters follow the `# NN — Title` heading pattern and end with a "Continue to" link.
