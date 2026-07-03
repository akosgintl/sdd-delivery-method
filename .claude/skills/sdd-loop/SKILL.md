---
name: sdd-loop
description: >-
  Drive the Specification-Driven Development write→review→rewrite loop for any SDD artifact (spec,
  design, tasks, traceability, prd, problem-statement) until the review verdict is approved or the
  iteration cap is hit. Use when the user asks to "loop", "iterate to approved", "run the review
  loop", "drive a spec/design/tasks to done", or otherwise wants the writer/reviewer/rewriter skills
  orchestrated automatically instead of invoked one at a time.
metadata:
  layer: sdd-skills
  role: orchestrator
  artifact: any
---

# sdd-loop

Orchestrate the inner loop for one artifact by **following the sibling SDD skills' procedures in
sequence** — skills are instruction sets you apply, not callable functions. This skill contributes
the control flow, the cap, and the escalation rules; the sibling skills contribute the work.

## Read first
- `../_shared/workflow.md` — the canonical loop rules (approval rule, CAP, escalation, ripple).
- `../_shared/review-format.md` — the verdict rule you branch on.

## Inputs
- **artifact type** (spec | design | tasks | traceability | prd | problem-statement) and
- **path** to the artifact (or its target feature folder).

## Procedure
1. **Resolve the sibling skills** for the artifact type: `<type>-writer`, `<type>-reviewer`, and
   (if it exists) `<type>-rewriter`. Read their `SKILL.md` bodies and apply them.
2. **Ensure a draft exists** — if the artifact file is missing, apply `<type>-writer`.
3. **Loop (cap = 3 rounds by default):**
   a. Apply `<type>-reviewer` → it writes the next `<artifact>.review-NN.md` and a verdict.
   b. If `verdict: approved` → **stop, report success** (path + rounds used).
   c. Else if a `<type>-rewriter` exists → apply it (fixes + checks off findings), then loop.
   d. Else (**no rewriter**: traceability, adr, constitution) → **escalate** per `../_shared/workflow.md`:
      regenerate (traceability), or surface the upstream/append fix — do not invent an approval.
   e. If the round counter reaches the cap → **stop without forcing approval**; surface the open
      `<artifact>.review-NN.md` and its unresolved BLOCKER/MAJOR findings to the user.
4. **Never fabricate approval.** Approval comes only from a reviewer verdict with no unresolved
   BLOCKER/MAJOR.

## Output contract
- Report the outcome: `approved` (with round count) or `capped`/`escalated` (with the open review
  file and remaining findings). Leave every `review-NN.md` in place as the audit trail.
