---
type: prd
title: Tessera — self-serve ticketing for independent organizers
status: draft
owner: product
updated: 2026-07-03
---

# PRD: Tessera — self-serve ticketing for independent organizers

> Product-level "why and what". Behavioral precision lives in the per-feature specs this spawns.

## 1. Problem & opportunity
Independent organizers of 50–2,000-seat events lose sales and goodwill on incumbent platforms to
overbooking, checkout drop-off, unrecovered sold-out demand, and manual refund/seat-release work.
Full evidence and the named segment are in
[`problem-event-ticketing.md`](./problem-event-ticketing.md): duplicate seat sales at every sold-out
event, 38% begin-purchase→paid drop-off, 15–20% of demand lost after sell-out, and ~6 min of manual
work per refund. The opportunity is a self-serve platform that makes overbooking structurally
impossible and recovers the leaked demand.

## 2. Goals & success metrics
- **G1 — No overbooking.** Metric: duplicate-seat incidents per event. Target: **0** on every event
  by **2026-12-31**.
- **G2 — Recover checkout.** Metric: begin-purchase→paid drop-off. Target: **≤ 20%** (from 38%)
  within two on-sale cycles of adoption, measured by **2027-03-31**.
- **G3 — Recover sold-out demand.** Metric: share of waitlisted buyers converted to a paid Order
  when inventory frees up. Target: **≥ 30%** by **2027-03-31**.
- **G4 — Eliminate manual recovery.** Metric: organizer minutes spent per refund. Target: **< 1 min**
  (from ~6) by **2027-03-31**, via automatic seat release.

## 3. Target users & needs
- **Organizer** (primary): sets up an event, prices tickets, and runs the door alone. JTBD: sell out
  without overselling, and recover refunds/waitlist without manual work.
- **Buyer**: purchases one or more tickets, often on mobile. JTBD: see price fast, pay quickly, get
  onto a waitlist if sold out.
- **Attendee**: is admitted at the door (may differ from the Buyer). JTBD: get in quickly, once.

## 4. Scope
- **In scope:** event setup; ticket types & pricing; seat selection with exclusive Holds; checkout &
  payment through a third-party provider; waitlist with offers; refunds with automatic seat release;
  check-in/validation at the door. Both seated and capacity-only (general-admission) events.
- **Out of scope / non-goals:** access-control hardware (turnstiles); dynamic/surge pricing;
  marketing & email campaigns; seating-chart design tools (organizers import a venue map); secondary
  resale/transfer marketplace; multi-organizer promoter hierarchies.

## 5. Constraints & assumptions
- Payment is card-based via a third-party PSP; the platform stores no PAN (constitution Q-4).
- Attendee PII stays in the EU region; retention ≤ 24 months post-event (constitution T-3).
- Mobile is the primary buying device.
- Organizers self-serve — no box-office staff to reconcile errors.

## 6. Feature breakdown → specs
> Each row becomes a spec under `specs/`.

| # | Feature | Spec | Status |
|---|---------|------|--------|
| 1 | Event setup (create event, venue/capacity, on-sale window) | specs/0001-event-setup/spec.md | draft |
| 2 | Ticket types & pricing (named priced categories, per-type inventory, fees) | specs/0002-ticket-types-pricing/spec.md | draft |
| 3 | Seat selection & Holds (exclusive seat claim, hold expiry, no double-book) | specs/0003-seat-selection/spec.md | draft |
| 4 | Checkout & payment (price-first flow, idempotent charge, Order creation) | specs/0004-checkout-payment/spec.md | draft |
| 5 | Waitlist (join sold-out type, ordered offers, offer expiry) | specs/0005-waitlist/spec.md | draft |
| 6 | Refunds & cancellation (full/partial refund, automatic seat release) | specs/0006-refunds-cancellation/spec.md | draft |
| 7 | Check-in & scanning (validate ticket once at the door, offline tolerance) | specs/0007-check-in/spec.md | draft |

## 7. Risks & open questions
- **R1:** PSP webhook latency could delay Order confirmation; specs 0004/0006 must define the
  reconciliation window.
- **R2:** Hold-expiry tuning trades conversion against overbooking risk; spec 0003 owns the decision
  (expect an ADR).
- **R3:** Offline check-in tolerance (spec 0007) risks double-admission if devices desync; bound it
  with a quantified reconciliation rule.
