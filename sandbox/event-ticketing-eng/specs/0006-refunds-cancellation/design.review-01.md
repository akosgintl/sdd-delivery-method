---
artifact: design
target: ./design.md
round: 1
reviewed: 2026-07-03
verdict: approved
---

# Review of ./design.md — round 01

## Findings

(none blocking)

## Summary

§7 maps FR-1..FR-8 and NFR-1..NFR-4 to tests naming their IDs; the atomicity invariant (NFR-2) is
verified by fault injection asserting no reversed-but-reserved state, and idempotency (FR-3/NFR-4) by
a 100× key test. Alternatives table keeps three rejected options and links **ADR-0002** — including
the tempting-but-unsafe "release immediately". Components own clear responsibilities (refund service,
billing adapter, reconciler, outbox). Interfaces/data model cite the FRs they realize. Rollout,
rollback (manual ops queue), and observability (`reversed_but_reserved_total` must stay 0) are
planned. Front-matter `spec:` resolves. ADR immutable and linked.

- BLOCKER: 0 · MAJOR: 0 · MINOR: 0

**Verdict: approved.**
