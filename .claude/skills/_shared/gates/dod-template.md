# Definition of Done (DoD)

> Gate work must pass to be called complete. The SDD-defining clauses: **verified against the
> spec** and **spec reconciled with reality**. Calibrate depth to risk; the items don't change.

Work is **Done** when all hold:

## A. Specification satisfied
- [ ] Every acceptance criterion in the spec is met and demonstrated.
- [ ] Every `FR-*` is implemented and covered by ≥1 automated test referencing its ID.
- [ ] Every `NFR-*` is verified against its stated number (not assumed).
- [ ] Specified edge cases and error behavior are implemented and tested.

## B. Specification reconciled  *(SDD-specific)*
- [ ] The spec reflects what was actually built — any behavior change is in the spec, **same PR**.
- [ ] Design doc / ADRs updated for approach decisions made during build.
- [ ] Traceability complete: need → requirement → test all linked; no orphan or untested reqs.
- [ ] Spec status advanced to `done`; `updated` date current.

## C. Engineering quality
- [ ] Code reviewed and merged to the target branch.
- [ ] Automated tests pass in CI (unit, integration, acceptance/contract).
- [ ] Coverage meets the constitution's bar.
- [ ] Static analysis / lint / type checks pass.
- [ ] Security checks pass (dependency, secrets, SAST as applicable).
- [ ] No TODO/FIXME contradicting the spec; no known defects above agreed severity.

## D. Operability & documentation
- [ ] User-facing docs / changelog / quickstart updated.
- [ ] Observability (logs/metrics/alerts) in place per constitution.
- [ ] Feature flag / rollout / migration handled; rollback path known.
- [ ] Runbook / on-call notes updated if operations are affected.

## E. Accepted
- [ ] Product owner / stakeholder accepts against the spec's acceptance criteria.
- [ ] Deployed to the agreed environment (or merged and ready to deploy, per your flow).

## For AI-implemented work, also:
- [ ] A human reviewed the output **against the spec** (every acceptance criterion), not just "it runs."
- [ ] Agent deviations from the spec are reconciled (code corrected or spec updated with a decision).
- [ ] Generated tests verify the spec — not merely mirror the implementation.

> Litmus test: open the spec and the running system side by side — do they agree on every
> acceptance criterion, with a test proving each one?
