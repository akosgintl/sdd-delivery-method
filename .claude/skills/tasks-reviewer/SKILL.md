---
name: tasks-reviewer
description: >-
  Review a Specification-Driven Development task breakdown (tasks.md) — stable T-n IDs,
  independently verifiable tasks, every task tracing to a requirement and every requirement covered
  (both directions), explicit dependencies + critical path, [P] markers, a test task referencing FR
  IDs, and no design/behavior leakage — then write tasks.review-NN.md. Use when the user asks to
  review, critique, or check a task list / tasks.md / work breakdown.
metadata:
  layer: sdd-skills
  role: reviewer
  artifact: tasks
---

# tasks-reviewer

Audit a `tasks.md` against its spec + design and emit a structured, sequence-numbered findings file.

## Read first
- `../_shared/tasks/checklist.md` — the rubric (columns, smell test, severities).
- `../_shared/conventions.md`, `../_shared/tasks/example.md`.
- `../_shared/review-format.md` — findings format, naming, verdict rule.

## Procedure
1. **Load tasks + its spec + design** (front-matter links); find existing `tasks.review-NN.md`; write
   `tasks.review-<highest+1>.md`.
2. **Run every check** in `../_shared/tasks/checklist.md`, in particular the **both-directions
   coverage** check: list every spec `FR`/`NFR` and confirm ≥1 task cites it (a requirement with no
   task is BLOCKER); list every task and confirm it cites ≥1 requirement (a task with no `Req` is
   MAJOR). Also: stable `T-n` IDs, no ID drift, explicit `Dep` + a critical path, `[P]` markers, a
   test/acceptance task referencing FR IDs, no design/behavior leakage, front-matter links spec+design.
3. **Record each violation as a finding** per `../_shared/review-format.md`.
4. **Set the verdict** — `approved` iff no unresolved BLOCKER/MAJOR.

## Output contract
- Write `tasks.review-NN.md` next to the tasks file. Do not modify `tasks.md`.
- Report the review path, verdict, and the coverage gaps found in each direction.
