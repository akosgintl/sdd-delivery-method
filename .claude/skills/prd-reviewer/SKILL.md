---
name: prd-reviewer
description: >-
  Review a Specification-Driven Development PRD (vision) — evidenced problem not solution, measurable
  goals with target dates, named users, bounded scope with non-goals, a feature-breakdown table that
  maps each feature to a spec, and no requirement-level detail leaking down from the specs — then
  write prd-<slug>.review-NN.md. Use when the user asks to review, critique, or check a PRD / vision
  document.
metadata:
  layer: sdd-skills
  role: reviewer
  artifact: prd
---

# prd-reviewer

Audit a PRD and emit a structured, sequence-numbered findings file. You judge; you do not edit the PRD.

## Read first
- `../_shared/prd/checklist.md` — the rubric (sections, smell test, severities).
- `../_shared/banned-words.md`, `../_shared/prd/example.md`.
- `../_shared/review-format.md` — findings format, naming, verdict rule.

## Procedure
1. **Locate the PRD** and any existing `<basename>.review-NN.md`; write `<basename>.review-<highest+1>.md`.
2. **Run every check** in `../_shared/prd/checklist.md`, in particular:
   - §1 states an evidenced **problem**, not a solution;
   - every goal has a **quantified metric + target date** (no banned words);
   - scope states in-scope **and** non-goals;
   - the feature-breakdown table maps each feature to a `spec.md` path;
   - **no requirement-level detail** (EARS/edge cases/schemas) has leaked down from the specs;
   - front-matter valid.
3. **Record each violation as a finding** per `../_shared/review-format.md`.
4. **Set the verdict** — `approved` iff no unresolved BLOCKER/MAJOR.

## Output contract
- Write `<basename>.review-NN.md` next to the PRD. Do not modify the PRD.
- Report the review path, verdict, and finding counts by severity.
