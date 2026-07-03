---
type: problem-statement
title: Small organizers lose sales to overbooking, checkout drop-off, and manual recovery
status: draft
owner: product
updated: 2026-07-03
---

# Problem statement: Small organizers lose sales to overbooking, checkout drop-off, and manual recovery

> A problem, stated before anyone has picked a solution.

## Who has the problem
Independent event organizers running one-off or small-series ticketed events (music nights, local
theatre, workshops, community sport) with 50–2,000 seats per event, who sell through incumbent
general-purpose ticketing platforms and manage the event themselves rather than through a promoter.

## What the problem is
When demand spikes at on-sale, two attendees can be sold the same seat, and the organizer only finds
out when both arrive at the door. Buyers who start a purchase abandon it partway through
because the flow is slow or asks for details before showing a price. When a ticket type sells out, interested buyers simply
leave with no way to be told if seats free up. When a buyer needs a refund, the organizer processes
it by hand and forgets to release the seat back for resale.

## Why it matters (impact)
- **Overbooking:** in a sample of 40 events run on incumbent tools, organizers reported 1–3 duplicate
  seat sales per sold-out event, each ending in a door refund plus a disgruntled attendee.
- **Checkout drop-off:** analytics from three pilot organizers show 38% of buyers who begin a
  purchase never complete payment.
- **Lost sell-out demand:** organizers estimate 15–20% of would-be buyers arrive after a ticket type
  is sold out and are lost entirely, because there is no waitlist.
- **Manual recovery cost:** refunds and their seat-release average 6 minutes of organizer time each,
  and released seats are re-listed late or not at all.

## Success metric
Reduce duplicate-seat incidents to **0 per event**, and reduce begin-purchase→paid drop-off from 38%
to **≤ 20%** within two on-sale cycles of an organizer adopting the platform.

## Constraints & assumptions
- Assume organizers self-serve; there is no dedicated box-office staff to reconcile errors.
- Assume payment is card-based through a third-party payment provider; the platform stores no PAN.
- Assume events are seated or capacity-limited (both must be supported).
- Assume mobile is the primary buying device for attendees.

## Out of scope
- Physical access-control hardware (turnstiles); we validate tickets, we don't drive gates.
- Dynamic/surge pricing algorithms.
- Marketing, email campaigns, and social promotion of events.
- Reserved-seating chart *design* tools (organizers import a venue map; they don't draw one here).

## User stories
- As an organizer, I want each seat sellable to at most one attendee at a time, so that nobody is
  turned away at the door. → resolves to `0003-seat-selection` (Hold/allocation FRs).
- As a buyer, I want to see the price before entering my details and finish paying quickly, so that
  I don't abandon the purchase. → resolves to `0004-checkout-payment`.
- As a buyer, I want to join a waitlist for a sold-out ticket type and be offered a seat if one frees
  up, so that I'm not lost as a sale. → resolves to `0005-waitlist`.
- As an organizer, I want a refund to automatically release the seat back for resale, so that I don't
  lose the resale and don't do it by hand. → resolves to `0006-refunds-cancellation`.
