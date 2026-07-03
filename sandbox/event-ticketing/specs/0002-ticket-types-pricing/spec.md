---
id: 0002-ticket-types-pricing
title: Ticket types and pricing with per-type inventory
status: ready
owner: eng-catalog
created: 2026-07-03
updated: 2026-07-03
need: ../../discovery/prd-event-ticketing.md#6-feature-breakdown--specs
supersedes: null
---

# Specification: Ticket types and pricing with per-type inventory

> The precise, testable description of intended behavior — the contract.

## 1. Summary & context
An organizer sells admission in named, priced categories — a Ticket type (e.g. "General Admission",
"VIP") — each with its own inventory count and price. This spec defines how types are created,
priced, and how their inventory bounds what seat-selection may hold. Terms Ticket type, Order are
defined in the glossary.

## 2. Goals
- Let an organizer define priced ticket types, each with a bounded inventory, under an event.
- Provide the per-type remaining-inventory count that seat selection (0003) decrements.

## 3. Non-goals (out of scope)
- Holding/allocating specific seats (spec `0003-seat-selection`).
- Discount codes, dynamic pricing, or bundles (non-goals per PRD).
- Taxes and payout accounting (billing concern, spec `0004`).

## 4. Functional requirements
- **FR-1:** When an organizer adds a ticket type with a name, price, and inventory count to an event,
  the system shall create the ticket type with remaining-inventory equal to that count.
- **FR-2:** If an organizer submits a ticket type with a negative price or a non-positive inventory
  count, then the system shall reject it and shall not create the ticket type.
- **FR-3:** The system shall express every ticket-type price as an integer number of minor currency
  units (e.g. cents) in a single stated currency per event.
- **FR-4:** When a Reservation is confirmed for a ticket type, the system shall decrement that type's
  remaining-inventory by the number of seats reserved.
- **FR-5:** If a hold or reservation would reduce a ticket type's remaining-inventory below zero,
  then the system shall reject it and shall not decrement below zero.
- **FR-6:** When a Reservation for a ticket type is released (e.g. by refund or hold expiry), the
  system shall increment that type's remaining-inventory by the released count.
- **FR-7:** While an event is unpublished, the system shall allow editing a ticket type's price and
  inventory; while published, the system shall allow increasing inventory but shall not allow
  reducing inventory below the already-reserved count.

## 5. Non-functional requirements
- **NFR-1:** Remaining-inventory reads shall reflect a confirmed Reservation within 1 s.
- **NFR-2:** Inventory decrement/increment shall be exact under concurrency: the sum of reserved +
  remaining shall always equal the original count (verified by a concurrent reserve/release test).
- **NFR-3:** All monetary amounts shall be computed in integer minor units; no floating-point money.

## 6. Acceptance criteria / scenarios

```gherkin
Scenario: Add a priced ticket type                             # verifies FR-1, FR-3
  Given an event exists
  When the organizer adds "VIP" priced at 7500 minor units with inventory 100
  Then a VIP ticket type shall exist with remaining-inventory 100

Scenario: Inventory never goes negative                        # verifies FR-5, NFR-2
  Given a ticket type with remaining-inventory 1
  When two Reservations for it are confirmed concurrently
  Then exactly one shall succeed and remaining-inventory shall be 0
```
- [ ] Negative price or non-positive inventory is rejected (FR-2).
- [ ] Confirming a Reservation decrements remaining-inventory (FR-4).
- [ ] Releasing a Reservation increments remaining-inventory (FR-6).
- [ ] Published inventory can increase but not drop below reserved count (FR-7).
- [ ] Remaining-inventory reflects a Reservation within 1 s (NFR-1).
- [ ] Prices are integer minor units, single currency per event (FR-3, NFR-3).

## 7. Edge cases & error behavior
- **Last unit, concurrent reserve:** exactly one succeeds, floor at zero (FR-5/NFR-2).
- **Reduce published inventory below reserved:** rejected (FR-7).
- **Release after sell-out:** remaining-inventory rises above zero, reopening sales (FR-6).
- **Price of 0:** allowed (free ticket); negative rejected (FR-2 boundary).

## 8. Data & interfaces
- TicketType = {`ticketTypeId`, `eventId`, `name`, `priceMinorUnits`, `currency`, `inventoryTotal`,
  `remainingInventory`}.

## 9. Dependencies & assumptions
- Dependencies: `0001-event-setup` (parent event); the inventory service (constitution A-1);
  consumed by `0003-seat-selection` and `0004-checkout-payment`.
- Assumptions: one currency per event.

## 10. Open questions
(none)

## 11. Rationale / decisions
- Integer-minor-unit money is mandated to avoid rounding error; aligns with constitution and is not
  significant enough for an ADR.
