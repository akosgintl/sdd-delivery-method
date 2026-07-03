---
name: traceability-reviewer
description: >-
  Audit a Specification-Driven Development traceability matrix (traceability.md) for coverage gaps,
  orphan code, ID drift, and test-naming — confirming every spec FR/NFR is a forward row with a
  test, backward trace has no orphan code, and Gaps are empty for done — then write
  traceability.review-NN.md with findings that ESCALATE fixes upstream (add a test/task), never
  hand-patch the matrix. Use when the user asks to review, audit, or check a traceability matrix.
metadata:
  layer: sdd-skills
  role: reviewer-audit
  artifact: traceability
---

# traceability-reviewer (audit)

Audit a generated `traceability.md` for coverage and integrity, and emit a structured findings file.
There is **no traceability-rewriter**: every finding escalates upstream (fix the spec/tasks/tests,
then `traceability-writer` regenerates). Never ask to hand-edit the matrix to close a gap.

## Read first
- `../_shared/traceability/checklist.md` — the audit rubric + escalation rule.
- `../_shared/conventions.md`, `../_shared/traceability/example.md`.
- `../_shared/review-format.md` — findings format, naming, verdict rule.

## Procedure
1. **Load the matrix + its spec + tasks** (to check against the source-of-truth IDs); find existing
   `traceability.review-NN.md`; write `traceability.review-<highest+1>.md`.
2. **Run the audit checks** in `../_shared/traceability/checklist.md`:
   - forward completeness (every spec `FR`/`NFR` is a row);
   - coverage gap (a requirement with no test) — `[BLOCKER]` for a `ready`/`done` target;
   - backward completeness (no code without a requirement);
   - ID drift (matrix ids not defined in spec/tasks);
   - test-naming convention; `Verified` consistency with status.
3. **Record each finding** per `../_shared/review-format.md`, and for coverage gaps add an
   `escalation:` line naming the upstream artifact to fix (e.g. "add test task in tasks.md; add
   `test_..._FR3`"). The `fix` for a gap is always upstream, not in the matrix.
4. **Set the verdict** — `approved` iff no unresolved BLOCKER/MAJOR.

## Output contract
- Write `traceability.review-NN.md` next to the matrix. Do not modify `traceability.md`.
- Report the review path, verdict, and the list of coverage gaps with their upstream escalations.
