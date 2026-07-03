---
id: 0001-event-setup
title: Event setup — create an event with venue, capacity, and on-sale window
status: ready
owner: eng-catalog
created: 2026-07-03
updated: 2026-07-03
need: ../../discovery/prd-event-ticketing.md#6-feature-breakdown--specs
supersedes: null
---

# Specification: Event setup — create an event with venue, capacity, and on-sale window

> The precise, testable description of intended behavior — the contract.

## 1. Summary & context
Before any ticket can be sold, an organizer must define the event: its identity, date/time, venue or
capacity model, and when tickets go on sale. This spec is the entry point of the platform and the
parent record every other feature references. It carries no money or inventory logic itself.

## 2. Goals
- Let an organizer create and publish an event that downstream features (ticket types, seating) hang from.
- Enforce a valid, unambiguous on-sale window so sales cannot open before intended.

## 3. Non-goals (out of scope)
- Ticket types, prices, and inventory (spec `0002-ticket-types-pricing`).
- Seat maps / seat selection (spec `0003-seat-selection`).
- Event promotion, marketing, or discovery/search.

## 4. Functional requirements
- **FR-1:** When an organizer submits an event with a title, start datetime, and capacity model, the
  system shall create the event in `draft` state.
- **FR-2:** If an organizer submits an event whose on-sale start is not before its on-sale end, then
  the system shall reject the submission and shall not create the event.
- **FR-3:** If an organizer submits an event whose start datetime is in the past, then the system
  shall reject the submission.
- **FR-4:** When an organizer publishes a `draft` event that has at least one ticket type, the system
  shall move the event to `published`.
- **FR-5:** If an organizer attempts to publish an event with no ticket type, then the system shall
  reject the publish and shall not change the event state.
- **FR-6:** While an event is `published` and the current time is outside its on-sale window, the
  system shall present the event as not-yet-on-sale or closed and shall not accept seat selections.
- **FR-7:** The system shall assign each event a globally unique, immutable event identifier on creation.

## 5. Non-functional requirements
- **NFR-1:** 95% of event-create and publish operations shall complete within 500 ms.
- **NFR-2:** Event identifiers shall be unique across the platform with collision probability
  < 1e-12 over 10^7 events.
- **NFR-3:** All organizer-facing setup screens shall meet WCAG 2.2 AA (constitution Q-2).

## 6. Acceptance criteria / scenarios

```gherkin
Scenario: Create a draft event                                 # verifies FR-1
  Given an organizer provides a title, a future start datetime, and a capacity model
  When they submit the event
  Then the event shall exist in draft state with a unique id

Scenario: Reject an inverted on-sale window                    # verifies FR-2
  Given an on-sale start that is not before the on-sale end
  When the organizer submits the event
  Then the submission shall be rejected and no event shall be created
```
- [ ] An event with a past start datetime is rejected (FR-3).
- [ ] Publishing a draft event that has ≥1 ticket type moves it to published (FR-4).
- [ ] Publishing an event with no ticket type is rejected (FR-5).
- [ ] Outside the on-sale window a published event refuses seat selection (FR-6).
- [ ] Each created event has a unique immutable id (FR-7, NFR-2).
- [ ] Create/publish complete < 500 ms for 95% of requests (NFR-1).
- [ ] Organizer setup screens pass an axe WCAG 2.2 AA scan (NFR-3).

## 7. Edge cases & error behavior
- **On-sale start == end:** rejected (FR-2, boundary).
- **Publish exactly at on-sale start:** sales open (FR-6 boundary).
- **Timezone of start datetime:** stored and evaluated in UTC; organizer's local zone recorded for display.
- **Editing a published event's start:** allowed only while no Reservation exists (design detail; flagged here).

## 8. Data & interfaces
- Event = {`eventId`, `title`, `startAt`, `capacityModel` ∈ {seated, capacity-only}, `onSaleStart`,
  `onSaleEnd`, `state` ∈ {draft, published, closed}, `timezone`}.

## 9. Dependencies & assumptions
- Dependencies: none upstream; `0002` and `0003` depend on this.
- Assumptions: an organizer account already exists and is authenticated.

## 10. Open questions
(none)

## 11. Rationale / decisions
- Capacity model is fixed at creation (seated vs capacity-only) to avoid mid-sale inventory reshaping;
  minor decision, noted here rather than an ADR.
