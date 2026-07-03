# Gate criteria (bundled reference for gate-runner)

The default, machine-checkable criteria the `gate-runner` evaluates when a project has no customized
`definition-of-ready.md` / `definition-of-done.md`. When those files **do** exist in the project, the
runner evaluates against **them** and treats this file only as a fallback. Each criterion names *how*
it is checked so the result is objective, not a vibe. Uses `../_shared/conventions.md`.

> A gate item must answer "how, concretely, do we check this?" — "the spec is clear" is not a gate
> item; "the Open Questions section is empty and every FR has a test referencing its ID" is.

## DoR — evaluate a spec/work-item before construction

Each is pass/fail; any fail ⇒ the item is **blocked** (not Ready).

- **DoR-1 intent:** the need/story is stated (who/what/why) and traces to a PRD or is standalone.
- **DoR-2 spec exists:** a `spec.md` is present with valid front-matter.
- **DoR-3 requirements:** every `FR` has a stable ID and is singular, unambiguous, testable (EARS).
- **DoR-4 NFRs quantified:** every `NFR` carries a number/condition (no banned vague words).
- **DoR-5 acceptance:** acceptance criteria exist and map 1:1 to requirement IDs.
- **DoR-6 edges:** edge/error behavior covered, not just the happy path.
- **DoR-7 scope:** goals **and** non-goals both stated.
- **DoR-8 open questions:** §10 is **empty**; no `TBD` (BLOCKER if `status` is `ready`/beyond).
- **DoR-9 glossary:** terms align with the glossary; contested terms defined.
- **DoR-10 feasible & agreed:** dependencies identified; conforms to the constitution (or a recorded
  ADR exception exists); reviewed by engineering + product owner.

## DoD — evaluate a completed feature

- **DoD-1 acceptance met:** every acceptance criterion demonstrated.
- **DoD-2 FR coverage:** every `FR` covered by ≥1 test referencing its ID (`__FR<n>`/tag/annotation).
- **DoD-3 NFR verified:** every `NFR` measured against its number (not assumed).
- **DoD-4 edges tested:** specified edge/error cases implemented and tested.
- **DoD-5 spec reconciled (SDD):** the spec reflects what was built; any behavior change is in the
  spec in the **same PR**; `status: done`; `updated` current.
- **DoD-6 design/ADRs updated** for decisions made during build.
- **DoD-7 traceability complete:** need → requirement → test linked; **no orphan or untested reqs**
  (the traceability matrix's Gaps are empty).
- **DoD-8 engineering quality:** review merged; CI tests/lint/type/security pass; coverage meets the
  constitution's bar.
- **DoD-9 operability:** observability, rollout/rollback, docs/runbook updated as applicable.
- **DoD-10 accepted:** stakeholder accepts against the spec's acceptance criteria.

> Calibrate depth to risk, but never by deleting a criterion — a one-line fix still has both gates,
> the clauses just shrink. The two SDD-defining DoD clauses (DoD-2/DoD-3 verification and DoD-5/DoD-7
> reconciliation) are non-negotiable — dropping them is exactly how spec rot sets in.
