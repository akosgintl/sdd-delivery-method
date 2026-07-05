---
id: 0005-waitlist
title: Waitlist — join a sold-out ticket type and receive ordered offers
status: ready
owner: eng-inventory
created: 2026-07-03
updated: 2026-07-04
need: ../../discovery/prd-event-ticketing.md#6-feature-breakdown--specs
supersedes: null
---

# Specification: Waitlist — join a sold-out ticket type and receive ordered offers

> The precise, testable description of intended behavior — the contract.

## 1. Summary & context
When a ticket type sells out, interested buyers leave with no way to be told if a seat frees up —
15–20% of demand is lost (PRD goal **G3**). This feature lets a buyer join an ordered Waitlist for a
sold-out ticket type; when inventory is released, the platform offers it to the front of the list for
a bounded window before moving on. Terms Waitlist (not Queue), Reservation are defined in the
glossary.

## 2. Goals
- Capture demand for a sold-out ticket type as an ordered, first-come-first-served Waitlist.
- Convert released inventory into offers to waitlisted buyers, fairly and in order.

## 3. Non-goals (out of scope)
- The pre-sale traffic Queue / virtual waiting room (a different concept, out of scope entirely).
- Charging for the offered seat (accepting an offer routes into `0004-checkout-payment`).
- Releasing inventory (that happens in `0003`/`0006`; this feature consumes `seat.released`).

## 4. Functional requirements
- **FR-1:** If a buyer requests a ticket type whose remaining-inventory is zero, then the system shall
  offer to add the buyer to that type's Waitlist.
- **FR-2:** When a buyer joins a Waitlist, the system shall append them to the end of that type's
  Waitlist with a recorded position and timestamp.
- **FR-3:** When inventory for a ticket type is released while its Waitlist is non-empty, the system
  shall create a time-limited offer to the buyer at the front of the Waitlist.
- **FR-4:** While an offer is outstanding, the system shall hold the released inventory for that buyer
  and shall not offer it to anyone else.
- **FR-5:** If an offer is not accepted within its offer window, then the system shall expire the
  offer.
- **FR-6:** When a buyer accepts an outstanding offer, the system shall convert the held inventory
  into a Hold for that buyer.
- **FR-7:** When a buyer accepts an offer, the system shall remove that buyer's entry from the
  Waitlist.
- **FR-8:** The system shall preserve Waitlist order: an earlier-joined buyer shall be offered
  released inventory before a later-joined buyer.
- **FR-9:** When an offer expires while the Waitlist is still non-empty, the system shall extend a new
  offer to the next buyer on the Waitlist.
- **FR-10:** If a buyer who already holds a `waiting` or `offered` entry for a ticket type requests to
  join that Waitlist again, then the system shall reject the second request and shall retain the
  existing entry's position.
- **FR-11:** When a buyer's held inventory is converted into a Hold (FR-6), the system shall route
  that buyer to checkout.
- **FR-12:** When the Waitlist is exhausted while released inventory remains unclaimed, the system
  shall return the remaining inventory to open sale.

## 5. Non-functional requirements
- **NFR-1:** When inventory is released, the first offer shall be created within 10 s.
- **NFR-2:** Waitlist ordering shall be strict FCFS: for any two buyers, the earlier `joinedAt` is
  offered first, verified by an ordering test over ≥ 1,000 entries.
- **NFR-3:** The default offer window shall be 300 s.
- **NFR-4:** The offer window shall be configurable per event within the range 60 s to 900 s.

## 6. Acceptance criteria / scenarios

```gherkin
Scenario: Join a sold-out ticket type's waitlist              # verifies FR-1, FR-2
  Given a ticket type with remaining-inventory 0
  When a buyer requests it and opts to join the waitlist
  Then the buyer shall be appended to the waitlist with a position and timestamp

Scenario: Released seat is offered to the front               # verifies FR-3, FR-4, FR-8
  Given a non-empty waitlist and one released seat
  When the release occurs
  Then a time-limited offer shall be created for the front buyer
  And that seat shall not be offered to anyone else while the offer stands

Scenario: Unaccepted offer expires and rolls to the next buyer   # verifies FR-5, FR-9
  Given an outstanding offer to buyer X with a 300 s window
  When 300 s pass without acceptance
  Then the offer to buyer X shall expire
  And an offer shall be extended to the next buyer on the Waitlist
```
- [ ] Accepting an offer converts held inventory into a Hold (FR-6).
- [ ] After converting, the buyer is routed to checkout (FR-11).
- [ ] On accept, the buyer's Waitlist entry is removed (FR-7).
- [ ] A repeat join while already `waiting`/`offered` is rejected and keeps the original position (FR-10).
- [ ] When the Waitlist is exhausted with inventory left, the remainder returns to open sale (FR-12).
- [ ] First offer created within 10 s of release (NFR-1).
- [ ] Strict FCFS ordering across ≥1,000 entries (NFR-2).
- [ ] Default offer window is 300 s (NFR-3); configurable within 60–900 s (NFR-4).

## 7. Edge cases & error behavior
- **Multiple seats released at once:** offers created to the front N buyers in order (FR-3/FR-8).
- **Front buyer already bought elsewhere:** offer still made; expiry rolls to next (FR-5).
- **Waitlist exhausted before inventory consumed:** remaining released inventory returns to open sale (FR-12).
- **Buyer joins twice while already `waiting`/`offered`:** the second request is rejected and the original position is kept (FR-10).

## 8. Data & interfaces
- WaitlistEntry = {`waitlistId`, `ticketTypeId`, `buyerId`, `joinedAt`, `position`, `state` ∈
  {waiting, offered, accepted, expired, removed, rejected}}.
- Consumes `seat.released` (from `0003`/`0006`); produces a Hold (into `0003`) on acceptance.

## 9. Dependencies & assumptions
- Dependencies: `0002-ticket-types-pricing` (inventory), `0003-seat-selection` (Hold on accept),
  `0006-refunds-cancellation` (release source), `0004-checkout-payment` (accept → checkout).
- Assumptions: buyers are reachable to receive an offer notification within the offer window.

## 10. Open questions
(none)

## 11. Rationale / decisions
- Single-offer-at-a-time (vs offering to N buyers and first-come-wins) is chosen for fairness and
  simplicity; if contested it becomes an ADR.
