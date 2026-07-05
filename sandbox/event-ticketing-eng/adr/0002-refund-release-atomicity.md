# ADR-0002: Refund and seat-release committed in one local transaction; PSP reversal reconciled

> Architecture Decision Record. One decision per file. Immutable once accepted.

- **Status:** accepted
- **Date:** 2026-07-03
- **Deciders:** eng-billing, architecture-guild
- **Related:** spec `0006-refunds-cancellation` (FR-2, FR-5, NFR-2); constitution A-1, A-3, P-2

## Context
Spec 0006 requires that no observable state exists where a payment is reversed but the seat stays
reserved, or vice versa (NFR-2), and that a billing timeout leaves the seat reserved with the refund
`pending` (FR-5). The PSP reversal is an external call that can time out or partially fail, while the
seat release is a local write to the inventory store. We cannot make an external PSP call and a local
DB write atomic in a single distributed transaction without a two-phase commit the PSP does not
support. Decided now because it governs the correctness of every refund path.

## Decision
We will treat the **local** refund-status change and the seat release (constitution A-1) as one local
DB transaction, and treat the PSP reversal as an asynchronous step guarded by an idempotency key
(P-2), reconciled by state machine: `requested → (PSP reversal confirmed) → succeeded` commits the
release in the same local transaction as the status flip; if the PSP does not confirm within 30 s the
refund stays `pending` and the seat stays reserved (FR-5). A background reconciler retries `pending`
reversals idempotently and only on confirmation runs the release transaction. `seat.released` is
emitted transactionally with the release (transactional outbox).

## Alternatives considered
- **Release the seat immediately on refund request, before PSP confirms** — rejected: a failed/timed-
  out reversal would leave a released (resellable) seat with the buyer's money not returned — the
  worst inconsistency; directly violates FR-5.
- **Distributed 2PC across PSP and inventory** — rejected: PSPs do not offer a prepare/commit
  protocol; not implementable.
- **Emit release event first, release eventually via consumer** — rejected: makes NFR-2's "no
  observable half-state" untestable and couples correctness to consumer liveness.
- **Synchronous PSP call inside the DB transaction** — rejected: holds a DB transaction open across a
  multi-second network call, causing lock contention and failing under load (Q-3).

## Consequences
- **Positive:** the money-reversed-and-seat-released invariant is atomic locally (NFR-2 testable);
  timeouts fail safe (seat stays reserved, FR-5); idempotency key makes retries exactly-once (NFR-4);
  transactional outbox makes `seat.released` reliable for the waitlist (0005).
- **Negative / trade-offs:** a window exists where the PSP has reversed but local state is still
  `pending` until the reconciler runs — funds are refunded slightly before the seat is resellable
  (acceptable: never the unsafe direction); requires a reconciler process and outbox infrastructure.
- **Follow-ups:** design specifies the reconciler cadence and outbox; test must assert no
  reversed-but-reserved state under injected PSP timeouts. Not a constitution exception.
