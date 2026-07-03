---
name: design-rewriter
description: >-
  Revise a Specification-Driven Development technical design (design.md) by applying the findings
  from its latest review file (design.review-NN.md) — adding missing FR/NFR verification paths,
  rejected alternatives, responsibilities, requirement links, and rollout/rollback plans, checking
  off each finding. Use when the user asks to rewrite, revise, fix, or apply review feedback to a
  design / design.md.
metadata:
  layer: sdd-skills
  role: rewriter
  artifact: design
---

# design-rewriter

Apply a reviewer's findings to a `design.md`, resolving each in the design and checking it off in
the review file.

## Read first
- `../_shared/review-format.md` — findings structure + rewriter contract.
- `../_shared/design/checklist.md`, `../_shared/conventions.md`.

## Procedure
1. **Find the highest-numbered `design.review-NN.md`**; read unresolved findings.
2. **Apply each finding's `fix`** to `design.md`. Keep `updated` current.
3. **For a decision that should be an ADR**, do not inline it permanently — create it via
   `adr-writer` and link from §2. **Never edit an existing accepted ADR** — supersede it.
4. **Flip `resolved: [ ]` → `resolved: [x]`** for each resolved finding (never delete).
5. **If a finding's root cause is in the spec** (e.g. an unbuildable/contradictory requirement),
   add an `escalation:` line naming the spec and leave `resolved: [ ]`.
6. **Re-self-check** against `../_shared/design/checklist.md`. Leave the verdict to the next review.

## Output contract
- Update `design.md` and the review file's checkboxes. Report resolved vs escalated findings.
- Do not write a new `review-NN.md`.
