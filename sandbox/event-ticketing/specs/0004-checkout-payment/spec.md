---
id: 0004-checkout-payment
title: Checkout and payment — price-first flow with idempotent charge
status: ready
owner: eng-billing
created: 2026-07-03
updated: 2026-07-04
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
- **FR-3:** When the charge succeeds, the system shall create exactly one Order.
- **FR-4:** If the buyer submits a checkout whose idempotency key was already processed, then the
  system shall return the existing Order and shall not charge again.
- **FR-5:** If any of the buyer's Holds has expired before payment succeeds, then the system shall
  reject the checkout and shall not charge the buyer.
- **FR-6:** If the billing adapter declines or does not respond within 5 s, then the system shall
  cancel the attempt and shall not create an Order.
- **FR-7:** The system shall record every charge attempt in an audit log with its Order (if any) and
  idempotency key.
- **FR-8:** The system shall store no payment-card PAN; it shall persist only the PSP charge token.
- **FR-9:** When the charge succeeds, the system shall convert each of the buyer's Holds to a
  Reservation.
- **FR-10:** When a checkout is rejected because a Hold has expired (FR-5), the system shall report to
  the buyer which seats were lost.
- **FR-11:** When the billing adapter declines or times out (FR-6), the system shall keep the buyer's
  Holds until their normal expiry.
- **FR-12:** If the charge succeeds but the Order is not persisted, then the system shall reverse the
  charge and shall not create an Order.

## 5. Non-functional requirements
- **NFR-1:** Excluding third-party PSP time, 95% of checkout submissions shall complete within 800 ms
  and 99% within 1.5 s.
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

Scenario: Successful payment creates one Order                 # verifies FR-2, FR-3, FR-9
  Given a buyer holds two seats and the charge will succeed
  When they submit payment
  Then the buyer shall be charged exactly the displayed itemized total
  And exactly one Order shall be created
  And both Holds shall become Reservations

Scenario: Duplicate submit charges once                        # verifies FR-4, NFR-2
  Given a checkout with idempotency key K has succeeded
  When the same checkout with key K is submitted 100 times
  Then exactly one charge shall have occurred and each response returns the same Order

Scenario: Expired hold blocks checkout                         # verifies FR-5, FR-10
  Given a buyer whose Hold on seat A12 has expired
  When they submit payment
  Then no charge shall occur and the buyer shall be told A12 was lost
```
- [ ] PSP decline/timeout cancels the attempt and creates no Order (FR-6); the buyer's Holds are kept until normal expiry (FR-11).
- [ ] If the Order write fails after a successful charge, the charge is reversed and no Order is created (FR-12).
- [ ] Every charge attempt is audit-logged with key (FR-7).
- [ ] No PAN persisted; only PSP token stored (FR-8, NFR-3).
- [ ] Excluding PSP time, 95% of submissions complete < 800 ms and 99% < 1.5 s (NFR-1).
- [ ] Checkout screens pass an axe WCAG 2.2 AA scan (NFR-4).

## 7. Edge cases & error behavior
- **Hold expires mid-payment:** checkout rejected, no charge (FR-5).
- **PSP timeout:** attempt cancelled, Holds retained (FR-6).
- **Network retry / double-click:** idempotency key collapses to one charge (FR-4/NFR-2).
- **Partial success (charge ok, Order write fails):** the charge is reversed and no Order is created (FR-12).

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
- **Ordering & atomicity (decided):** the buyer is charged first; Order creation and Hold→Reservation
  conversion happen only on charge success (FR-2 → FR-3/FR-9). If the Order write fails after a
  successful charge, the charge is reversed and no Order is created (FR-12). This is the observable
  contract; no separate ADR is required.
