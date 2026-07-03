---
name: spec-rewriter
description: >-
  Revise a Specification-Driven Development feature spec (spec.md) by applying the findings from its
  latest review file (spec.review-NN.md) — fixing EARS/NFR/acceptance/traceability defects, checking
  off each finding, and re-self-checking. Use when the user asks to rewrite, revise, fix, address
  review comments on, or apply feedback to a spec / spec.md.
metadata:
  layer: sdd-skills
  role: rewriter
  artifact: spec
---

# spec-rewriter

Apply a reviewer's findings to a `spec.md`, resolving each one in the spec and checking it off in the
review file.

## Read first
- `../_shared/review-format.md` — how findings are structured and the rewriter contract.
- `../_shared/spec/checklist.md`, `../_shared/ears.md`, `../_shared/banned-words.md`,
  `../_shared/gherkin.md`, `../_shared/conventions.md`.

## Procedure
1. **Find the highest-numbered `spec.review-NN.md`** next to the spec; read its unresolved findings.
2. **Apply each unresolved finding's `fix`** to `spec.md`, preserving stable IDs (never renumber;
   retire, don't reuse). Keep front-matter honest — bump `updated`.
3. **Flip `resolved: [ ]` → `resolved: [x]`** in the review file for each finding you resolved
   (never delete a finding).
4. **If a finding's root cause is upstream** (e.g. the need itself is missing/untestable), do not
   fake a fix — add an `escalation:` line naming the upstream artifact and leave `resolved: [ ]`.
5. **Re-self-check** the changed spec against `../_shared/spec/checklist.md`.
6. Leave the `verdict` for the next `spec-reviewer` round to re-judge.

## Output contract
- Update `spec.md` in place and the review file's checkboxes.
- Report: which findings were resolved, which were escalated (and to where), and the new spec
  version's readiness. Do **not** write a new `review-NN.md` — that is the reviewer's next round.
