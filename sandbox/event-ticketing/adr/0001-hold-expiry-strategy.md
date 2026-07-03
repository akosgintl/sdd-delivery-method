# ADR-0001: Fixed-timer server-authoritative Holds with a sweep + lazy check

> Architecture Decision Record. One decision per file. Immutable once accepted.

- **Status:** accepted
- **Date:** 2026-07-03
- **Deciders:** eng-inventory, architecture-guild
- **Related:** spec `0003-seat-selection` (FR-1, FR-4, FR-5, NFR-2, NFR-3); constitution A-1, P-2

## Context
Spec 0003 requires that a seat is never allocated to two attendees (FR-5, NFR-2) and that a Hold
auto-releases within 5 s of a 600 s expiry (FR-4, NFR-3), under 500 concurrent buyers per event
(NFR-1). We must choose how a Hold's exclusivity and expiry are enforced. The forces: correctness
under concurrency (no double-book), timely release of abandoned Holds, and bounded latency — decided
now because every other 0003 requirement and spec 0004's conversion depend on the Hold semantics.

## Decision
We will represent a Hold as a row in the single authoritative inventory store (constitution A-1) with
an absolute `expiresAt` set at creation to `now + 600 s`. Exclusivity is enforced by a conditional
write (compare-and-set on seat state `available → held`) so exactly one concurrent selection wins.
Expiry is enforced two ways: (a) a **lazy check** — any read/allocation treats a Hold past `expiresAt`
as released and eligible; and (b) a **background sweep** every ≤ 2 s that flips expired Holds to
`available` and emits `seat.released`. All expiry decisions use the store's authoritative clock, not
app-node clocks.

## Alternatives considered
- **Sliding-window (extend on activity)** — rejected: a buyer idling on the seat map could hold a
  seat indefinitely, defeating timely release and hurting sell-through; also complicates fairness.
- **Reserve-on-select (no separate Hold, immediate Reservation)** — rejected: forces payment before
  the buyer commits and makes abandonment expensive to unwind; contradicts the price-first flow (0004).
- **In-memory locks / distributed lock service (e.g. Redis lease) as source of truth** — rejected:
  introduces a second source of truth for seat state, risking split-brain double-allocation against
  the inventory store (violates A-1's single-writer rule).
- **TTL-only (no sweep), rely on lazy check** — rejected: a seat could appear held long after expiry
  to other buyers' cached views, exceeding the 5 s release bound (NFR-3) and starving the waitlist.

## Consequences
- **Positive:** single source of truth (A-1 honored); conditional write gives provable no-double-book
  (NFR-2); absolute `expiresAt` + ≤2 s sweep meets the 5 s release bound; idempotency key on the
  select satisfies P-2.
- **Negative / trade-offs:** the sweep is continuous background load proportional to open Holds; a
  hot event's inventory row is a contention point (mitigated by per-seat, not per-event, CAS
  granularity); lazy check adds a branch to every read path.
- **Follow-ups:** design must specify the sweep cadence and the CAS retry/backoff; load test must
  prove NFR-1 under sweep load. Not a constitution exception, so no expiry date.
