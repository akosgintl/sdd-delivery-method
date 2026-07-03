---
name: prd-writer
description: >-
  Draft a Specification-Driven Development product requirements document (PRD / vision) for an
  initiative — evidenced problem, measurable goals with target dates, target users, bounded scope
  with non-goals, and a feature-breakdown table where each row becomes a spec. Kept at the why/what
  altitude, not behavioral detail. Use when the user asks to write, draft, or create a PRD / vision
  doc / product requirements for an initiative.
metadata:
  layer: sdd-skills
  role: writer
  artifact: prd
---

# prd-writer

Draft a PRD — the product-level **why and what** for an initiative too big for one feature. Keep it
at the goals/users/scope/metrics altitude; behavioral precision belongs in the specs it spawns.

## Read first
- `../_shared/prd/template.md`, `../_shared/prd/checklist.md`, `../_shared/prd/example.md`.
- `../_shared/banned-words.md` (metrics must be quantified).

## Procedure
1. **Open with the problem, not the product** — reuse a problem statement (who, what pain, what it
   costs, with evidence). If there's no evidenced problem, get one first.
2. **State goals as measurable outcomes** — each goal gets a metric and a target-by-date.
3. **Name target users and their jobs-to-be-done.**
4. **Draw the scope line including non-goals.**
5. **Decompose into a feature-breakdown table** — each row is one feature → one `spec.md` path.
6. **Record constraints, assumptions, and initiative-level risks.**
7. **Resist behavioral detail** — write the feature row, not the rule; the rule lives in the spec.
8. **Self-apply `../_shared/prd/checklist.md`.**

## Output contract
- Write to `docs/product/prd-<slug>.md` (default; honor a user-given path).
- **Never overwrite an existing PRD** — defer to `prd-rewriter`.
- Report the path and the feature→spec table (the seam to the per-feature bundles).
