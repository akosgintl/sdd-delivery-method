---
id: 0001-cart-persistence
title: Shopping cart survives a browser crash
status: ready
owner: a.gintl
created: 2026-06-01
updated: 2026-06-18
need: docs/product/prd-checkout-revamp.md#cart-loss
supersedes: null
---

# Specification: Shopping cart survives a browser crash

## 1. Summary & context
Customers frequently lose their shopping cart when their browser crashes or is closed
accidentally during a session, and a measurable share abandon the purchase rather than rebuild
it. This feature persists a logged-in customer's cart durably so it can be restored when they
return, reducing crash-related abandonment.

Originating need: [PRD — Checkout Revamp §2.1 "Cart loss"](../../../docs/product/prd-checkout-revamp.md#cart-loss).

## 2. Goals
- A logged-in customer's cart is preserved across browser crashes, accidental closes, and device
  restarts within the same session window.
- Reduce crash-related cart abandonment (PRD success metric: −50%).

## 3. Non-goals (out of scope)
- Synchronizing carts across **different** devices (a separate feature).
- Persisting carts for **guest** (not-logged-in) users.
- Merging a restored cart with items added on another session (last-write-wins is acceptable here).

## 4. Functional requirements
- **FR-1** *(event-driven):* When an item is added to or removed from the cart, the system shall
  persist the updated cart to durable per-user storage within 500 ms.
- **FR-2** *(event-driven):* When a logged-in customer with a persisted, non-expired cart opens
  the site, the system shall restore the most recently persisted cart.
- **FR-3** *(state-driven):* While a persisted cart has had no activity for more than 24 hours,
  the system shall treat it as expired and shall not restore it.
- **FR-4** *(unwanted behavior):* If persisting the cart fails, then the system shall retry up to
  3 times with backoff and, if all retries fail, shall surface a non-blocking warning to the
  customer without losing the in-memory cart.
- **FR-5** *(ubiquitous):* The system shall scope every persisted cart to the authenticated user
  and shall never expose one user's cart to another.

## 5. Non-functional requirements
- **NFR-1** *(performance):* Cart restoration shall complete within 1 s at p95 for carts of up
  to 100 line items.
- **NFR-2** *(security/privacy):* Persisted cart data shall be stored encrypted at rest and shall
  contain no payment-card data.
- **NFR-3** *(reliability):* Persistence shall succeed for ≥ 99.9% of cart-change events
  (measured monthly).

## 6. Acceptance criteria / scenarios

```gherkin
Scenario: Unsaved cart is restored after a crash        # verifies FR-2
  Given a logged-in customer with 3 items in their cart
  And the cart has not been checked out
  When the browser crashes and the customer reopens the site within 24 hours
  Then the cart shall contain the same 3 items

Scenario: Cart is not restored after expiry             # verifies FR-3
  Given a logged-in customer with items in their cart
  When 24 hours pass without any cart activity
  Then the cart shall be treated as expired and shall not be restored
```
- [ ] Changing the cart persists within 500 ms (FR-1).
- [ ] A persist failure retries 3× and shows a non-blocking warning without dropping items (FR-4).
- [ ] User A can never see User B's persisted cart (FR-5).
- [ ] Restoration of a 100-item cart completes < 1 s p95 (NFR-1).

## 7. Edge cases & error behavior
- **Empty cart:** persisting an empty cart is valid and clears any prior persisted cart (FR-1).
- **Concurrent sessions:** last write wins; no merge (per non-goals).
- **Storage unavailable:** retry then non-blocking warning; in-memory cart preserved (FR-4).
- **Cart exactly at 24h boundary:** treated as expired at *strictly greater than* 24h (FR-3).

## 8. Data & interfaces
- Persisted entity: `Cart { userId, items[], updatedAt }`. See
  [`design.md` §5](design.md#5-data-model). No new public API; uses existing cart service.

## 9. Dependencies & assumptions
- Depends on the existing authentication service (to scope carts to a user).
- Assumes durable per-user storage is available (see design for the choice).

## 10. Open questions
*(empty — resolved during `/clarify`; this spec is `ready`)*

## 11. Rationale / decisions
- 24-hour expiry chosen to balance convenience against stale-price/stock risk; see
  [ADR-0007](../../../docs/adr/0007-cart-expiry-window.md).
- Last-write-wins (no cross-session merge) chosen to keep v1 simple; revisit if data shows pain.
