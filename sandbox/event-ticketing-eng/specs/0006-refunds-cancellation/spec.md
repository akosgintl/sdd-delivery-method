---
id: 0006-refunds-cancellation
title: Refunds and cancellation with automatic seat release
status: ready
owner: eng-billing
created: 2026-07-03
updated: 2026-07-03
need: ../../discovery/prd-event-ticketing.md#6-feature-breakdown--specs
supersedes: null
---

# Specification: Refunds and cancellation with automatic seat release

> The precise, testable description of intended behavior — the contract.

## 1. Summary & context
Today organizers process refunds by hand and forget to release the seat, losing the resale (PRD goal
**G4**). This feature lets a buyer or organizer cancel all or part of an Order, reverses the payment
through the billing adapter, and automatically releases the freed Reservation back to inventory so it
can be resold or offered to the waitlist. Terms Refund, Reservation, Order are defined in the
glossary.

## 2. Goals
- Reverse payment for a cancelled Order (fully or per-line) without manual accounting.
- Release the freed seats back to `available` automatically, in the same transaction as the refund.
- Make refunds idempotent so a retried or duplicated request never double-refunds.

## 3. Non-goals (out of scope)
- The original charge/checkout (spec `0004-checkout-payment`).
- Offering the freed seat to the waitlist (spec `0005-waitlist` consumes the release event).
- Organizer-initiated event cancellation en masse (future initiative; single-Order scope here).
- Chargeback/dispute handling initiated by the card network.

## 4. Functional requirements
- **FR-1:** When a buyer or organizer requests a refund for an Order line, the system shall reverse
  that line's payment through the billing adapter.
- **FR-2:** When a line's payment is reversed, the system shall release that line's Reservation to
  `available` in the same transaction as the refund.
- **FR-3:** If a refund request carries an idempotency key that has already been processed, then the
  system shall return the original result and shall not issue a second reversal.
- **FR-4:** Where a refund is partial, the system shall reverse only the specified lines and shall
  leave the remaining Reservations intact.
- **FR-5:** If the billing adapter fails to confirm the reversal within 30 s, then the system shall
  leave the Reservation intact and shall mark the refund `pending` for retry, without releasing the
  seat.
- **FR-6:** The system shall record every refund attempt (requested, succeeded, pending, failed) in
  an audit log with its Order and idempotency key.
- **FR-7:** If a refund is requested for an Order line that is already refunded, then the system
  shall reject the request and shall not release the seat again.
- **FR-8:** When a Reservation is released by a refund, the system shall emit a `seat.released` event
  for downstream consumers (waitlist, resale).

## 5. Non-functional requirements
- **NFR-1:** 95% of refund requests shall receive an accepted/pending response within 800 ms,
  excluding the third-party settlement time.
- **NFR-2:** A refund reversal and its seat release shall be atomic: no observable state shall exist
  in which the payment is reversed but the seat remains reserved, or vice versa.
- **NFR-3:** The system shall store no payment-card PAN; refunds reference the PSP charge token only
  (constitution Q-4).
- **NFR-4:** Duplicate submission of the same refund (same idempotency key) shall result in exactly
  one reversal, verified by a test submitting the same request 100 times.

## 6. Acceptance criteria / scenarios

```gherkin
Scenario: Full refund releases the seat                        # verifies FR-1, FR-2
  Given a paid Order with one Reservation on seat A12
  When the organizer refunds the Order
  Then the payment shall be reversed
  And seat A12 shall be available again

Scenario: Duplicate refund is idempotent                       # verifies FR-3, NFR-4
  Given a refund with idempotency key K has succeeded for an Order
  When the same refund with key K is submitted 100 times
  Then exactly one reversal shall have occurred
  And every response shall report the original result

Scenario: Partial refund leaves other seats reserved           # verifies FR-4
  Given a paid Order with Reservations on seats A12 and A13
  When only the A12 line is refunded
  Then A12 shall be available and A13 shall remain reserved

Scenario: Billing timeout keeps the seat held                  # verifies FR-5, NFR-2
  Given a refund request for seat A12
  When the billing adapter does not confirm within 30 s
  Then A12 shall remain reserved
  And the refund shall be marked pending for retry
```
- [ ] Refunding an already-refunded line is rejected and does not re-release the seat (FR-7).
- [ ] Every refund attempt is written to the audit log with Order id and key (FR-6).
- [ ] A release emits a `seat.released` event (FR-8).
- [ ] 95% of refund responses return < 800 ms excluding settlement (NFR-1).
- [ ] No stored PAN; refund uses the PSP charge token (NFR-3).

## 7. Edge cases & error behavior
- **Retry after network drop:** idempotency key collapses to one reversal (FR-3/NFR-4).
- **Billing adapter timeout:** seat stays reserved, refund `pending` (FR-5).
- **Double refund of same line:** second request rejected (FR-7).
- **Partial refund of a multi-seat Order:** only named lines reversed/released (FR-4).
- **Refund of a seat already offered to waitlist:** release event is the trigger; ordering handled in
  `0005-waitlist`.

## 8. Data & interfaces
- Refund = {`orderId`, `lineIds[]`, `idempotencyKey`, `status` ∈ {requested, pending, succeeded,
  failed}, `pspChargeToken`}.
- Emits `seat.released` {`seatId`, `eventId`, `releasedAt`}.
- Refund API is idempotent on `idempotencyKey` (constitution P-2).

## 9. Dependencies & assumptions
- Dependencies: `0004-checkout-payment` (the original Order/charge), the billing adapter
  (constitution A-3), the inventory service (A-1) for release, `0005-waitlist` (consumer).
- Assumptions: the PSP supports referencing a prior charge by token for reversal.

## 10. Open questions
(none)

## 11. Rationale / decisions
- Whether release is synchronous-in-the-refund-transaction vs event-driven-eventual is a significant
  correctness decision (atomicity vs coupling) → **ADR-0002**.
