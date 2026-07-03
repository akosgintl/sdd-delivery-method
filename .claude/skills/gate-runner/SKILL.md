---
name: gate-runner
description: >-
  Evaluate a Specification-Driven Development artifact or feature against a quality gate — the
  Definition of Ready (is this spec ready to build?) or Definition of Done (is this feature complete?)
  — and report pass or a list of blocking criteria with pointers. A checkpoint, not a rewriter. Use
  when the user asks "does this pass DoR/DoD?", "is this ready to build?", "is this done?", or to run
  the ready/done gate on a spec or feature.
metadata:
  layer: sdd-skills
  role: gate-runner
  artifact: gates
---

# gate-runner

Run a gate against a target and report **pass** or the blocking criteria. This is a checkpoint that
governs progression — it does not fix anything (that's the artifact's own writer/rewriter).

## Read first
- `../_shared/gates/gate-criteria.md` — the default criteria + how each is checked.
- `../_shared/conventions.md`, `../_shared/review-format.md` (the gate-report variant).

## Inputs
- **which gate** (DoR | DoD) and **the target** (a `spec.md` for DoR; a feature folder for DoD).

## Procedure
1. **Load the gate definition.** If the project has `definition-of-ready.md` /
   `definition-of-done.md`, evaluate against **those**; otherwise fall back to
   `../_shared/gates/gate-criteria.md`.
2. **Evaluate each criterion** against the target, using the stated check (e.g. DoR: §10 empty, every
   `FR` testable, NFRs quantified; DoD: every `FR` has a test referencing its ID, traceability Gaps
   empty, spec reconciled + `status: done`). For DoD, use the feature's `traceability.md` for the
   coverage/reconciliation checks.
3. **Collect failures** as findings (severity `BLOCKER`), each naming the criterion, the gap, and a
   `file:line` pointer.
4. **Set the outcome:** `verdict: pass` if no criterion fails; else `verdict: blocked`.

## Output contract
- **DoD (audit trail):** write a gate report `<feature>.dod-gate-NN.md` next to the feature per the
  `../_shared/review-format.md` gate-report variant. **DoR:** report inline (persist a
  `<spec>.dor-gate-NN.md` only if the user wants a record).
- Report the verdict and, on `blocked`, the failing criteria and where to fix them (a DoR failure
  bounces to the spec loop; a DoD failure bounces to the offending artifact).
