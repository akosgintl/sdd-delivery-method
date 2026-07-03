---
id: 0003-seat-selection
title: Seat selection with exclusive Holds and no double-booking
status: ready
owner: eng-inventory
created: 2026-07-03
updated: 2026-07-03
need: ../../discovery/prd-event-ticketing.md#6-feature-breakdown--specs
supersedes: null
---

# Specification: Seat selection with exclusive Holds and no double-booking

> The precise, testable description of intended behavior — the contract.

## 1. Summary & context
Buyers must be able to claim specific seats (or general-admission capacity) during a busy on-sale
without any seat ever being sold to two attendees. This feature makes overbooking structurally
impossible by granting each buyer a short-lived exclusive **Hold** that either converts to a
**Reservation** at checkout or auto-releases on expiry. It is the core of PRD goal **G1 (0 duplicate
seats)**. Terms Hold / Reservation / Seat allocation are defined in the glossary.

## 2. Goals
- Guarantee at most one active claim (Hold or Reservation) per seat at any instant.
- Give buyers a bounded, predictable window to complete purchase of the seats they picked.
- Allocate specific seats for seated events and remaining-count units for capacity-only
  (general-admission) events (see FR-7).

## 3. Non-goals (out of scope)
- The checkout/payment flow itself (spec `0004-checkout-payment`).
- Seating-chart design/import (organizer imports a venue map; not drawn here).
- Waitlisting a sold-out ticket type (spec `0005-waitlist`).
- Choosing the hold-expiry duration policy per event — a single platform default with an ADR here.

## 4. Functional requirements
- **FR-1:** When a buyer selects an available seat, the system shall create a Hold on that seat for
  that buyer with an expiry timestamp.
- **FR-2:** If a buyer selects a seat that is already held or reserved, then the system shall reject
  the selection and shall not create a second Hold on that seat.
- **FR-3:** While a Hold on a seat is active, the system shall present that seat as unavailable to
  every other buyer.
- **FR-4:** When a Hold reaches its expiry timestamp without a completed Order, the system shall
  release the seat to `available`.
- **FR-5:** The system shall allocate each seat to at most one active Hold or Reservation at any
  instant.
- **FR-6:** When a buyer completes checkout for a held seat, the system shall convert that Hold into
  a Reservation.
- **FR-7:** Where an event is capacity-only, the system shall hold against the ticket type's
  remaining-capacity count rather than a specific seat, applying FR-1 through FR-6 to that count.
- **FR-8:** If a buyer's active Holds for an event would exceed the per-order limit of 10 seats, then
  the system shall reject the additional selection and shall not create the Hold.
- **FR-9:** When a buyer explicitly releases a held seat, the system shall release it to `available`
  before its expiry.

## 5. Non-functional requirements
- **NFR-1:** 95% of seat-selection requests shall complete within 300 ms at 500 concurrent buyers
  per event.
- **NFR-2:** Under concurrent selection of the same seat, exactly one request shall succeed; a test
  issuing ≥ 1,000 simultaneous selections against one seat shall record exactly 1 Hold created and
  999 rejections.
- **NFR-3:** Hold expiry (FR-4) shall take effect within 5 s of the expiry timestamp.
- **NFR-4:** The default Hold duration shall be 600 s and shall be configurable per event between
  120 s and 1,800 s.

## 6. Acceptance criteria / scenarios

```gherkin
Scenario: Selecting an available seat creates a Hold           # verifies FR-1
  Given seat A12 is available
  When a buyer selects seat A12
  Then a Hold on A12 shall exist for that buyer with an expiry in the future

Scenario: Two buyers race for the same seat                    # verifies FR-2, FR-5, NFR-2
  Given seat A12 is available
  When 1,000 buyers select seat A12 simultaneously
  Then exactly one buyer shall hold A12
  And the other 999 selections shall be rejected

Scenario: A Hold expires and the seat returns                  # verifies FR-4, NFR-3
  Given a buyer holds seat A12 with a 600 s timer
  When 600 s pass with no completed Order
  Then within 5 s seat A12 shall be available again

Scenario: Held seat is hidden from others                      # verifies FR-3
  Given buyer X holds seat A12
  When buyer Y views the seat map
  Then A12 shall be shown as unavailable to buyer Y

Scenario: Hold becomes a Reservation at checkout               # verifies FR-6
  Given buyer X holds seat A12
  When buyer X completes checkout
  Then A12 shall be a Reservation for buyer X and no longer a Hold
```
- [ ] Capacity-only event holds against remaining count, last unit race yields exactly one Hold (FR-7).
- [ ] Selecting an 11th seat in one order is rejected (FR-8).
- [ ] Explicitly releasing a held seat returns it to available before expiry (FR-9).
- [ ] 95% of selections complete < 300 ms at 500 concurrent buyers (NFR-1).
- [ ] Default hold duration is 600 s; configurable 120–1,800 s (NFR-4).

## 7. Edge cases & error behavior
- **Simultaneous last-seat selection (seated):** exactly one Hold created (FR-2/FR-5/NFR-2).
- **Simultaneous last-unit (capacity-only):** exactly one Hold against the count (FR-7).
- **Hold expires while buyer is on the payment screen:** checkout of an expired Hold is rejected;
  buyer is told the seat was released (FR-4 + escalates to 0004 checkout).
- **Buyer abandons without releasing:** seat returns automatically at expiry (FR-4).
- **Clock skew between app nodes:** expiry is evaluated against a single authoritative clock (design).

## 8. Data & interfaces
- Seat state ∈ {`available`, `held`, `reserved`}.
- Hold = {`seatId`, `buyerId`, `eventId`, `expiresAt`, `idempotencyKey`}.
- Select API is idempotent on `idempotencyKey` (constitution P-2).
- Full schema and store choice → `design.md`.

## 9. Dependencies & assumptions
- Dependencies: `0002-ticket-types-pricing` (inventory counts), `0004-checkout-payment` (conversion
  to Reservation), the inventory service (constitution A-1).
- Assumptions: a single authoritative inventory store mediates all seat-state writes.

## 10. Open questions
(none)

## 11. Rationale / decisions
- Hold-expiry strategy (fixed timer vs sliding vs reservation-on-select) is a significant,
  hard-to-reverse decision → **ADR-0001**.
