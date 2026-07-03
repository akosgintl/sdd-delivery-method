---
id: 0001-cart-persistence
artifact: tasks
status: ready
updated: 2026-06-18
spec: ./spec.md
design: ./design.md
---

# Task Breakdown: Cart persistence

| ID | Task | Dep | Req | Est | Status |
|----|------|-----|-----|-----|--------|
| T-1 | Define `Cart` schema + encrypted-at-rest storage table | — | FR-5, NFR-2 | S | todo |
| T-2 | Implement `CartStore.put/get` with userId scoping | T-1 | FR-1, FR-5 | M | todo |
| T-3 | Persist-on-mutation hook (500 ms budget, debounce) | T-2 | FR-1 | M | todo |
| T-4 | Restore-on-session-start with expiry filter | T-2 | FR-2, FR-3 | M | todo |
| T-5 | Retry+backoff and non-blocking warning on failure | T-2 | FR-4 | S | todo |
| T-6 | Daily purge job for expired carts | T-1 | FR-3 | S | todo |
| T-7 | Acceptance + security + perf tests referencing FR/NFR IDs | T-3,T-4,T-5 | FR-1..5, NFR-1 | M | todo |
| T-8 | Feature flag, metrics, alerts | T-3,T-4 | NFR-3 | S | todo |

## Critical path
T-1 → T-2 → T-3 → T-7 (persist path drives the longest chain).

## Parallelizable
- T-4 (restore) and T-5 (retry) proceed alongside T-3 once T-2 lands.
- T-6 (purge) parallel after T-1.

## Notes
Spike already confirmed encryption-at-rest is enabled on the primary datastore (no separate
spike task needed).
