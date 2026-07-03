---
id: 0004-checkout-payment
title: Checkout and payment — price-first flow with idempotent charge
status: ready
owner: eng-billing
created: 2026-07-03
updated: 2026-07-03
need: ../../discovery/prd-event-ticketing.md#6-feature-breakdown--specs
supersedes: null
---

# Specification: Checkout and payment — price-first flow with idempotent charge

> The precise, testable description of intended behavior — the contract.

## 1. Summary & context
Buyers abandon purchases when the flow is slow or hides the price. This feature turns a set of active
Holds into a paid Order: it shows the full price before asking for details, charges the buyer once
through the billing adapter, and converts the Holds to Reservations. It is the core of PRD goal
**G2 (drop-off ≤ 20%)**. Terms Hold, Reservation, Order, Buyer are defined in the glossary.

## 2. Goals
- Show the total price (tickets + fees) before collecting payment details.
- Charge the buyer exactly once and create one Order per successful checkout.
- Convert the buyer's Holds to Reservations only when payment succeeds.

## 3. Non-goals (out of scope)
- Creating or expiring Holds (spec `0003-seat-selection`).
- Refunds (spec `0006-refunds-cancellation`).
- Storing card data — all card handling is delegated to the PSP.

## 4. Functional requirements
- **FR-1:** When a buyer opens checkout for their active Holds, the system shall display the itemized
  total (ticket prices plus fees) before requesting any payment detail.
- **FR-2:** When a buyer submits payment for their Holds, the system shall charge the buyer through
  the billing adapter for exactly the displayed total.
- **FR-3:** When the charge succeeds, the system shall create one Order and convert each of the
  buyer's Holds to a Reservation.
- **FR-4:** If the buyer submits a checkout whose idempotency key was already processed, then the
  system shall return the existing Order and shall not charge again.
- **FR-5:** If any of the buyer's Holds has expired before payment succeeds, then the system shall
  reject the checkout, shall not charge the buyer, and shall report which seats were lost.
- **FR-6:** If the billing adapter declines or does not respond within 5 s, then the system shall
  cancel the attempt, shall not create an Order, and shall keep the Holds until their normal expiry.
- **FR-7:** The system shall record every charge attempt in an audit log with its Order (if any) and
  idempotency key.
- **FR-8:** The system shall store no payment-card PAN; it shall persist only the PSP charge token.

## 5. Non-functional requirements
- **NFR-1:** 95% of checkout submissions shall receive a terminal response within 3 s, excluding
  third-party PSP time; p99 < 800 ms for the platform's own processing.
- **NFR-2:** Duplicate submission of the same checkout (same idempotency key) shall result in exactly
  one charge, verified by a test submitting the same request 100 times.
- **NFR-3:** The system shall store no PAN at rest (constitution Q-4), verified by a data-catalog scan.
- **NFR-4:** All checkout screens shall meet WCAG 2.2 AA.

## 6. Acceptance criteria / scenarios

```gherkin
Scenario: Price shown before payment details                   # verifies FR-1
  Given a buyer holds two seats
  When they open checkout
  Then the itemized total including fees shall be shown before any card field

Scenario: Successful payment creates one Order                 # verifies FR-2, FR-3
  Given a buyer holds two seats and the charge will succeed
  When they submit payment
  Then exactly one Order shall be created and both Holds shall become Reservations

Scenario: Duplicate submit charges once                        # verifies FR-4, NFR-2
  Given a checkout with idempotency key K has succeeded
  When the same checkout with key K is submitted 100 times
  Then exactly one charge shall have occurred and each response returns the same Order

Scenario: Expired hold blocks checkout                         # verifies FR-5
  Given a buyer whose Hold on seat A12 has expired
  When they submit payment
  Then no charge shall occur and the buyer shall be told A12 was lost
```
- [ ] PSP decline/timeout cancels the attempt, creates no Order, keeps Holds (FR-6).
- [ ] Every charge attempt is audit-logged with key (FR-7).
- [ ] No PAN persisted; only PSP token stored (FR-8, NFR-3).
- [ ] 95% of submissions terminal < 3 s excluding PSP; p99 own-processing < 800 ms (NFR-1).
- [ ] Checkout screens pass an axe WCAG 2.2 AA scan (NFR-4).

## 7. Edge cases & error behavior
- **Hold expires mid-payment:** checkout rejected, no charge (FR-5).
- **PSP timeout:** attempt cancelled, Holds retained (FR-6).
- **Network retry / double-click:** idempotency key collapses to one charge (FR-4/NFR-2).
- **Partial success (charge ok, Order write fails):** reconciled to one Order or a full reversal
  (design/ADR territory; flagged).

## 8. Data & interfaces
- Order = {`orderId`, `buyerId`, `eventId`, `lines[]`, `feeMinorUnits`, `totalMinorUnits`,
  `pspChargeToken`, `idempotencyKey`, `createdAt`}.
- Checkout API is idempotent on `idempotencyKey` (constitution P-2); PSP calls go through the billing
  adapter (constitution A-3).

## 9. Dependencies & assumptions
- Dependencies: `0003-seat-selection` (Holds to convert), `0002-ticket-types-pricing` (prices), the
  billing adapter (A-3).
- Assumptions: the PSP supports idempotency keys and returns a reusable charge token.

## 10. Open questions
(none)

## 11. Rationale / decisions
- Charge-then-reserve vs reserve-then-charge ordering and the charge/Order-write atomicity are
  significant → captured in design; a dedicated ADR is raised if the design surfaces a hard tradeoff.
