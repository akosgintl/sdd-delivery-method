---
name: constitution-reviewer
description: >-
  Review a Specification-Driven Development project constitution — every clause universal,
  non-negotiable, and enforceable; quality bars quantified; each clause notes how it's checked; an
  amendment + ADR-based exception process defined; no style-guide or per-feature detail; short — then
  write constitution.review-NN.md. Use when the user asks to review, critique, or check a
  constitution / project principles.
metadata:
  layer: sdd-skills
  role: reviewer
  artifact: constitution
---

# constitution-reviewer

Audit the constitution and emit a structured, sequence-numbered findings file. You judge; you do not
edit it (amendments are `constitution-writer`'s job — there is no rewriter, since the constitution is
append/amend-only via a reviewed process).

## Read first
- `../_shared/constitution/checklist.md` — the rubric (buckets, smell test, severities).
- `../_shared/banned-words.md`, `../_shared/constitution/example.md`.
- `../_shared/review-format.md` — findings format, naming, verdict rule.

## Procedure
1. **Locate the constitution** and any existing `constitution.review-NN.md`; write
   `constitution.review-<highest+1>.md`.
2. **Run every check** in `../_shared/constitution/checklist.md`, in particular:
   - each clause is universal, non-negotiable, **and enforceable** (names how it's checked);
   - quality bars are quantified (no banned vague words);
   - an amendment process **and** an ADR-based exception process exist (missing = BLOCKER);
   - no style-guide or per-feature detail; the document is short.
3. **Record each violation as a finding** per `../_shared/review-format.md`.
4. **Set the verdict** — `approved` iff no unresolved BLOCKER/MAJOR.

## Output contract
- Write `constitution.review-NN.md` next to the constitution. Do not modify it.
- Report the review path, verdict, and finding counts by severity. To act on findings, the user runs
  `constitution-writer` to amend.
