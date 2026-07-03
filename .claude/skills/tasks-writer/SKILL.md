---
name: tasks-writer
description: >-
  Draft a Specification-Driven Development task breakdown (tasks.md) from an agreed design — a
  dependency-aware table of independently verifiable T-n tasks, each tracing to the FR/NFR it
  advances, with a critical path, parallelizable markers, and a test/acceptance task. Use when the
  user asks to write, draft, break down, or plan tasks / a task list / work breakdown for a feature.
metadata:
  layer: sdd-skills
  role: writer
  artifact: tasks
---

# tasks-writer

Draft a feature `tasks.md` — the ordered, dependency-aware decomposition of the design into units
small enough to build and verify one at a time.

## Read first
- `../_shared/tasks/template.md`, `../_shared/tasks/checklist.md`, `../_shared/tasks/example.md`.
- `../_shared/conventions.md` (IDs, coverage rules).

## Procedure
1. **Read the spec and the design** (need both; refuse if the design isn't at least `agreed`).
2. **Derive tasks from the design's components and test strategy**, not imagination — each component
   to build, interface to implement, and requirement to verify becomes one or more tasks.
3. **Make each task independently verifiable**; split anything that isn't (an `L` is usually two tasks).
4. **Tag every task with the `FR`/`NFR` it advances**, then check coverage **both ways**: every
   requirement has ≥1 task; every task has ≥1 requirement.
5. **Add a dedicated test/acceptance task** referencing the FR IDs.
6. **Wire dependencies, read off the critical path**, mark independent work `[P]`.
7. **Note sequencing rationale, spikes, external blockers.**
8. **Self-apply `../_shared/tasks/checklist.md`.**

## Output contract
- Write to `specs/NNNN-slug/tasks.md` (default Layout A; honor a user-given path).
- **Never overwrite an existing `tasks.md`** — defer to `tasks-rewriter`.
- Report the path and the both-directions coverage result (any requirement with no task, or task
  with no requirement).
