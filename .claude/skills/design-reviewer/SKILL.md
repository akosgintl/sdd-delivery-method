---
name: design-reviewer
description: >-
  Review a Specification-Driven Development technical design (design.md) against its spec — every
  FR/NFR has a build+verify path, the alternatives table keeps rejected options, components list
  responsibilities, interfaces/data model cite requirement IDs, rollout/rollback/observability are
  planned, no behavior is smuggled in, and decisions are captured as immutable ADRs — then write
  design.review-NN.md. Use when the user asks to review, critique, or check a design / design.md.
metadata:
  layer: sdd-skills
  role: reviewer
  artifact: design
---

# design-reviewer

Audit a `design.md` against its spec and emit a structured, sequence-numbered findings file. You
judge; you do not edit the design.

## Read first
- `../_shared/design/checklist.md` — the rubric (sections, smell test, severities).
- `../_shared/conventions.md`, `../_shared/design/example.md`.
- `../_shared/review-format.md` — findings-file format, naming, verdict rule.

## Procedure
1. **Load the design + its spec** (via the `spec:` front-matter link) and any existing
   `design.review-NN.md`; write `design.review-<highest+1>.md`.
2. **Run every check** in `../_shared/design/checklist.md`, in particular:
   - test strategy maps **every** spec `FR`/`NFR` to a verification (a missing one is BLOCKER);
   - alternatives table keeps rejected options + reasons;
   - components list responsibilities; interfaces/data model cite requirement IDs;
   - no observable behavior smuggled in (belongs in the spec);
   - rollout/rollback/observability present;
   - significant decisions are ADRs; ADRs are immutable (flag any edited beyond its status line);
   - front-matter valid; `spec:` resolves.
3. **Record each violation as a finding** per `../_shared/review-format.md` (severity, id/section,
   `file:line`, `rule:` + source, concrete `fix:`, `resolved: [ ]`).
4. **Set the verdict** — `approved` iff no unresolved BLOCKER/MAJOR.

## Output contract
- Write `design.review-NN.md` next to the design. Do not modify `design.md`.
- Report the review path, verdict, and finding counts by severity.
