---
name: tasks-rewriter
description: >-
  Revise a Specification-Driven Development task breakdown (tasks.md) by applying the findings from
  its latest review file (tasks.review-NN.md) — adding missing tasks for uncovered requirements,
  fixing traces, dependencies, critical path, and splitting oversized tasks, checking off each
  finding. Use when the user asks to rewrite, revise, fix, or apply review feedback to a task list /
  tasks.md.
metadata:
  layer: sdd-skills
  role: rewriter
  artifact: tasks
---

# tasks-rewriter

Apply a reviewer's findings to a `tasks.md`, resolving each and checking it off in the review file.

## Read first
- `../_shared/review-format.md` — findings structure + rewriter contract.
- `../_shared/tasks/checklist.md`, `../_shared/conventions.md`.

## Procedure
1. **Find the highest-numbered `tasks.review-NN.md`**; read unresolved findings.
2. **Apply each finding's `fix`** — add tasks for uncovered requirements (new `T-n`, never reuse a
   retired ID), fix `Req`/`Dep` columns, re-derive the critical path, split `L` tasks. Keep
   `updated` current.
3. **Flip `resolved: [ ]` → `resolved: [x]`** per resolved finding (never delete).
4. **If a coverage gap's root cause is upstream** (e.g. the requirement itself is untestable so no
   task can verify it), add an `escalation:` line naming the spec and leave `resolved: [ ]`.
5. **Re-self-check** against `../_shared/tasks/checklist.md`, re-running both-directions coverage.
6. Leave the verdict to the next `tasks-reviewer` round.

## Output contract
- Update `tasks.md` and the review file's checkboxes. Report resolved vs escalated findings.
- Do not write a new `review-NN.md`.
