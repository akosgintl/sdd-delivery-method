---
name: problem-statement-reviewer
description: >-
  Review a Specification-Driven Development problem statement — states a problem not a solution, names
  a specific who, gives evidenced impact, a quantified success metric, explicit scope, and stories
  with a benefit clause pointing to specs — then write problem-<slug>.review-NN.md. Use when the user
  asks to review, critique, or check a problem statement / the "why" / user stories.
metadata:
  layer: sdd-skills
  role: reviewer
  artifact: problem-statement
---

# problem-statement-reviewer

Audit a problem statement and emit a structured, sequence-numbered findings file. You judge; you do
not edit it.

## Read first
- `../_shared/problem-statement/checklist.md` — the rubric (anatomy, smell test, severities).
- `../_shared/banned-words.md`, `../_shared/problem-statement/example.md`.
- `../_shared/review-format.md` — findings format, naming, verdict rule.

## Procedure
1. **Locate the problem statement** and any existing `<basename>.review-NN.md`; write
   `<basename>.review-<highest+1>.md`.
2. **Run every check** in `../_shared/problem-statement/checklist.md`, in particular:
   - it states a **problem, not a solution** (no screen/table/tech named) — a solution-in-disguise
     is `[BLOCKER]`;
   - a specific **who** is named;
   - **impact is evidenced** and the **success metric is quantified**;
   - scope/out-of-scope explicit;
   - each user story has a benefit clause and points to a spec.
3. **Record each violation as a finding** per `../_shared/review-format.md`.
4. **Set the verdict** — `approved` iff no unresolved BLOCKER/MAJOR.

## Output contract
- Write `<basename>.review-NN.md` next to the problem statement. Do not modify it.
- Report the review path, verdict, and finding counts by severity.
