---
id: 0001-cart-persistence
artifact: design
status: agreed
owner: t.lead
updated: 2026-06-18
spec: ./spec.md
---

# Technical Design: Cart persistence

> How we satisfy `spec.md`.

## 1. Overview
Persist the cart server-side, keyed by `userId`, written on every cart mutation and read on
session start. Server-side (not `localStorage`) because the spec requires durability across
device restarts and encryption at rest (NFR-2), which client storage can't guarantee.

## 2. Approach & alternatives considered
| Option | Pros | Cons | Decision |
|--------|------|------|----------|
| Server-side store keyed by userId | Durable, encryptable, cross-restart | Network dependency on write | ✅ chosen |
| Browser `localStorage` | No backend, instant | Not encrypted at rest, lost on cache clear, not cross-device-restart-safe | rejected — fails NFR-2 |
| Cookie-based | Simple | Size limits, sent on every request | rejected — size/perf |

Decision recorded in ADR-0008.

## 3. Components & responsibilities
- **CartService** (existing) — gains `persist(cart)` and `restore(userId)`.
- **CartStore** (new) — encapsulates durable storage; retry/backoff lives here (FR-4).
- **Session bootstrap** — calls `restore(userId)` on login/site-open (FR-2), applying expiry (FR-3).

## 4. Interfaces & contracts
- Internal: `CartStore.put(userId, cart): Result`, `CartStore.get(userId): Cart | null`.
- No new public/external API. Realizes FR-1 (put), FR-2/FR-3 (get + expiry filter), FR-5 (key by userId).

## 5. Data model
`Cart { userId: string (PK), items: LineItem[], updatedAt: timestamp }`, encrypted at rest
(NFR-2). Expiry (FR-3) computed as `now - updatedAt > 24h` at read time; a daily job purges
expired rows.

## 6. Integration points & dependencies
- Auth service supplies `userId` (dependency from spec §9).
- Storage: existing primary datastore with encryption-at-rest enabled.

## 7. Test strategy
- FR-1 → integration test asserting persist within 500 ms after a mutation.
- FR-2 → acceptance test (Gherkin scenario 1) restoring a cart after simulated crash.
- FR-3 → unit + acceptance test at the 24h boundary (Gherkin scenario 2).
- FR-4 → fault-injection test forcing 3 store failures; assert retry + warning + items retained.
- FR-5 → security test: User A cannot read User B's cart.
- NFR-1 → load test: 100-item cart restore p95 < 1 s.

## 8. Risks & mitigations
| Risk | Likelihood | Impact | Mitigation |
|------|-----------|--------|------------|
| Write latency on every mutation | Med | Med | Async write with 500 ms budget; debounce rapid changes |
| Store outage drops carts | Low | High | Retry+backoff; non-blocking warning; in-memory retained (FR-4) |

## 9. Rollout & operability
- Behind feature flag `cart_persistence`; ramp 5% → 50% → 100%.
- Metrics: persist success rate (NFR-3), restore latency (NFR-1), retry counts. Alert if persist
  success < 99.9% over 1h. Rollback = disable flag (in-memory cart behavior unchanged).
