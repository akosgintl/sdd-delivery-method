# Definition of Done (DoD) — Tessera

> Gate a feature must pass to be called complete. The SDD-defining clauses — **verified against the
> spec** and **spec reconciled with reality** — are non-negotiable. [auto]/[human] as in DoR.

Work is **Done** when all hold:

## A. Specification satisfied
- [ ] Every acceptance criterion in the spec is demonstrated. [human]
- [ ] Every `FR-*` is implemented and covered by ≥1 automated test referencing its ID (`__FR<n>`/`@FR-<n>`). [auto]
- [ ] Every `NFR-*` is verified against its stated number — including the p95/p99 latency and idempotency bars. [auto]
- [ ] Specified edge/error cases (double-book, hold expiry, payment timeout, duplicate scan) are implemented and tested. [auto]

## B. Specification reconciled *(SDD — non-negotiable)*
- [ ] The spec reflects what was built; any behavior change is in the spec in the **same PR**. [human]
- [ ] Design doc / ADRs updated for approach decisions made during build. [human]
- [ ] Traceability complete: need → requirement → test → code all linked; the matrix's Gaps section is empty. [auto]
- [ ] Spec `status` advanced to `done`; `updated` date current. [auto]

## C. Engineering quality
- [ ] Code reviewed and merged to the target branch. [human]
- [ ] CI tests pass (unit, integration, acceptance/contract). [auto]
- [ ] Coverage meets the constitution's bar (≥ 80% changed lines). [auto]
- [ ] Lint / type / static analysis pass. [auto]
- [ ] Security checks pass (dependency, secrets, SAST); no PAN stored at rest. [auto]

## D. Operability & documentation
- [ ] Observability (logs/metrics/alerts) in place per constitution — including seat-inventory and payment reconciliation metrics. [human]
- [ ] Feature flag / rollout / migration handled; rollback path known. [human]
- [ ] Attendee-facing docs / changelog updated. [human]

## E. Accepted
- [ ] Product owner accepts against the spec's acceptance criteria. [human]
- [ ] Deployed to the agreed environment (or merged and ready to deploy). [human]

## For AI-implemented work, also:
- [ ] A human reviewed the output **against the spec** (every acceptance criterion), not just "it runs." [human]
- [ ] Agent deviations from the spec are reconciled (code corrected or spec updated with a decision). [human]
- [ ] Generated tests verify the spec — not merely mirror the implementation. [human]

> Litmus test: open the spec and the running system side by side — do they agree on every
> acceptance criterion, with a test proving each one?
