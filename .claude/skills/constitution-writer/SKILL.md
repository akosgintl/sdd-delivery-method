---
name: constitution-writer
description: >-
  Bootstrap or amend a Specification-Driven Development project constitution — the short, stable list
  of non-negotiable, enforceable rules (engineering principles, architectural constraints, quantified
  quality bars, technology constraints, process rules, amendment/exception process) every spec
  inherits. Use when the user asks to write, draft, bootstrap, or amend a constitution / project
  principles / engineering standards.
metadata:
  layer: sdd-skills
  role: writer
  artifact: constitution
---

# constitution-writer

Bootstrap (or **amend**) the project constitution — the standing law every spec and implementation
honors. Short, stable, enforceable.

## Read first
- `../_shared/constitution/template.md`, `../_shared/constitution/checklist.md`,
  `../_shared/constitution/example.md`.
- `../_shared/banned-words.md` (quality bars must be quantified).

## Procedure
1. **Bootstrap vs amend:** if a constitution already exists, do **not** overwrite it — **amend**:
   add/adjust clauses, bump `version`, set `last_amended`, and (for a removed/changed clause) note it.
2. **Brain-dump candidate rules** across the six buckets, then **filter hard** — keep a rule only if
   it is universal, non-negotiable, *and* enforceable. Cut style-guide and per-feature detail.
3. **Quantify the quality bars** — coverage %, latency budget, accessibility level, security baseline.
4. **Make each clause checkable** — note how it's enforced (CI gate, review step, linter).
5. **Define the amendment and exception process** — exceptions are ADRs with an expiry/review date.
6. **Cut until short** — a new engineer should read it in one sitting. Use stable IDs
   (`P-`, `A-`, `Q-`, `T-`, `R-`).
7. **Self-apply `../_shared/constitution/checklist.md`.**

## Output contract
- Write/append to the constitution at the project's well-known path (default `constitution.md` at
  repo root or `docs/`; honor a user-given path). Bootstrapping a fresh file is fine; **amending**
  edits in place with a version bump.
- Report the path, the version, and any clauses that need an enforcement mechanism named.
