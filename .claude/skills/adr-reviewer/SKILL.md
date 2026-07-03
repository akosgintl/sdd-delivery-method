---
name: adr-reviewer
description: >-
  Review a Specification-Driven Development Architecture Decision Record (ADR) — one decision per
  file, globally unique number, legal status, context that explains why now, real rejected
  alternatives, positive+negative consequences, and immutability respected (no edits to an accepted
  ADR beyond its status line) — then write ADR-NNNN.review-NN.md. Use when the user asks to review,
  critique, or check an ADR / architecture decision record.
metadata:
  layer: sdd-skills
  role: reviewer
  artifact: adr
---

# adr-reviewer

Audit an ADR and emit a structured, sequence-numbered findings file. You judge; you do not edit it —
and there is **no adr-rewriter**: a flawed *accepted* decision is changed by writing a superseding
ADR (`adr-writer`), never by editing in place.

## Read first
- `../_shared/adr/checklist.md` — the rubric (anatomy, smell test, severities).
- `../_shared/conventions.md`, `../_shared/adr/example.md`.
- `../_shared/review-format.md` — findings format, naming, verdict rule.

## Procedure
1. **Locate the ADR** and any existing `ADR-NNNN.review-NN.md`; write `ADR-NNNN.review-<highest+1>.md`.
2. **Run every check** in `../_shared/adr/checklist.md`, in particular:
   - one decision per file; a **globally unique** number (flag any collision with an existing ADR);
   - status legal; a superseded ADR links to its successor and is otherwise unedited;
   - context explains the forces + why now; decision in active voice;
   - alternatives include real rejected options with reasons;
   - consequences list positives **and** negatives; a constitution exception has an expiry;
   - **immutability respected** — flag any edit to an accepted ADR beyond its Status line (BLOCKER).
3. **Record each violation as a finding** per `../_shared/review-format.md`.
4. **Set the verdict** — `approved` iff no unresolved BLOCKER/MAJOR.

## Output contract
- Write `ADR-NNNN.review-NN.md` next to the ADR. Do not modify the ADR.
- Report the review path, verdict, and finding counts. Fixes to a *proposed* ADR are made by editing
  it directly; fixes to an *accepted* one require a superseding ADR via `adr-writer`.
