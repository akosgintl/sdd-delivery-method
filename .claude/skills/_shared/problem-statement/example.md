---
type: problem-statement
title: Customers lose their cart when the browser crashes
status: agreed
owner: p.singh
updated: 2026-05-28
---

# Problem statement: Customers lose their cart when the browser crashes

> Stated as a problem, before choosing a solution. Feeds the Checkout Revamp PRD §1 and the
> cart-persistence spec.

## Who has the problem
Logged-in customers shopping during a sale, who have built up a multi-item cart mid-session.

## What the problem is
When their browser crashes or is closed accidentally before checkout, their cart is lost. Faced with
rebuilding it from memory, a large share abandon the purchase instead.

## Why it matters (impact)
Session analytics show ~8% of sale-day sessions hit a cart-loss event, and the majority of those do
not convert. Support tickets corroborate ("lost everything in my basket when Chrome crashed"). This
is measurable, recurring, lost revenue — not a one-off.

## Success metric
Crash-related cart abandonment, reduced by 50% within one quarter of launch.

## Constraints & assumptions
- Must not store payment-card data as part of any cart-preservation mechanism (privacy floor).
- Assumes customers are logged in (guest carts are a separate problem).
- Assumes durable per-user storage is available to the checkout service.

## Out of scope
- Synchronizing carts across **different** devices.
- Preserving carts for guest (not-logged-in) users.

## User stories
- As a logged-in customer, I want my cart to survive a browser crash so that I don't lose items I
  was about to buy. → resolves to FR-1, FR-2 in specs/0001-cart-persistence/spec.md.
