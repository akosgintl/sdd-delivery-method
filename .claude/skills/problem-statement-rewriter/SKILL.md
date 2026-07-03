---
name: problem-statement-rewriter
description: >-
  Revise a Specification-Driven Development problem statement by applying the findings from its latest
  review file (problem-<slug>.review-NN.md) — stripping out solutions, naming the who, quantifying
  impact and the success metric, and fixing stories, checking off each finding. Use when the user asks
  to rewrite, revise, fix, or apply review feedback to a problem statement.
metadata:
  layer: sdd-skills
  role: rewriter
  artifact: problem-statement
---

# problem-statement-rewriter

Apply a reviewer's findings to a problem statement, resolving each and checking it off in the review file.

## Read first
- `../_shared/review-format.md` — findings structure + rewriter contract.
- `../_shared/problem-statement/checklist.md`, `../_shared/banned-words.md`.

## Procedure
1. **Find the highest-numbered `<basename>.review-NN.md`**; read unresolved findings.
2. **Apply each finding's `fix`** — strip out any solution language, name the specific who, add
   evidence, quantify the success metric, tighten scope, fix stories. Keep `updated` current.
3. **Flip `resolved: [ ]` → `resolved: [x]`** per resolved finding (never delete).
4. **If a finding needs evidence you don't have** (e.g. the impact number), do not invent it — add an
   `escalation:` line noting the missing evidence and leave `resolved: [ ]`.
5. **Re-self-check** against `../_shared/problem-statement/checklist.md`. Leave the verdict to the
   next review.

## Output contract
- Update the problem statement and the review file's checkboxes. Report resolved vs escalated findings.
- Do not write a new `review-NN.md`.
