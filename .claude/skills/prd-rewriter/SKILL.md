---
name: prd-rewriter
description: >-
  Revise a Specification-Driven Development PRD (vision) by applying the findings from its latest
  review file (prd-<slug>.review-NN.md) — quantifying goals, adding non-goals, mapping features to
  specs, and pulling behavioral detail back down into specs, checking off each finding. Use when the
  user asks to rewrite, revise, fix, or apply review feedback to a PRD / vision document.
metadata:
  layer: sdd-skills
  role: rewriter
  artifact: prd
---

# prd-rewriter

Apply a reviewer's findings to a PRD, resolving each and checking it off in the review file.

## Read first
- `../_shared/review-format.md` — findings structure + rewriter contract.
- `../_shared/prd/checklist.md`, `../_shared/banned-words.md`.

## Procedure
1. **Find the highest-numbered `<basename>.review-NN.md`**; read unresolved findings.
2. **Apply each finding's `fix`** to the PRD — quantify goals, add non-goals, fix the feature→spec
   table. **Behavioral detail flagged as too low-altitude** is not deleted blindly: move it into the
   relevant spec (or note it belongs there) rather than dropping the intent. Keep `updated` current.
3. **Flip `resolved: [ ]` → `resolved: [x]`** per resolved finding (never delete).
4. **If a finding's root cause is a missing problem statement**, add an `escalation:` line and leave
   `resolved: [ ]`.
5. **Re-self-check** against `../_shared/prd/checklist.md`. Leave the verdict to the next review.

## Output contract
- Update the PRD and the review file's checkboxes. Report resolved vs escalated findings.
- Do not write a new `review-NN.md`.
