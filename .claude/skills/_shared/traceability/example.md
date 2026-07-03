---
id: 0001-cart-persistence
artifact: traceability
updated: 2026-06-18
spec: ./spec.md
---

# Traceability Matrix: Cart persistence

> In practice this view is generated from the IDs referenced in spec/tasks/tests.
> Shown here filled in for illustration.

## Forward trace (coverage)

| Req | Requirement (short) | Source need | Task(s) | Test(s) | Code | Verified |
|-----|---------------------|-------------|---------|---------|------|:--------:|
| FR-1 | persist cart on change ≤500 ms | PRD §2.1 | T-2,T-3 | `test_persist_on_change__FR1` | `CartStore.put` | ☐ |
| FR-2 | restore on reopen | PRD §2.1 | T-4 | `test_restore_after_crash__FR2` | `CartService.restore` | ☐ |
| FR-3 | expire after 24h | PRD §2.2 | T-4,T-6 | `test_expiry_boundary__FR3` | expiry filter + purge job | ☐ |
| FR-4 | retry + non-blocking warning | PRD §2.1 | T-5 | `test_persist_failure_retry__FR4` | `CartStore` retry | ☐ |
| FR-5 | per-user isolation | PRD §4 (privacy) | T-1,T-2 | `test_cross_user_isolation__FR5` | userId-keyed store | ☐ |
| NFR-1 | restore <1s p95 (100 items) | PRD §3 | T-7 | `perf_restore_100_items__NFR1` | — | ☐ |
| NFR-2 | encrypted at rest, no PAN | constitution Q-4 | T-1 | `test_no_pan_stored__NFR2` | storage config | ☐ |
| NFR-3 | persist success ≥99.9% | PRD §3 | T-8 | monitored (dashboard) | metrics | ☐ |

## Backward trace (justification)

| Code / module | Realizes | Originating need |
|---------------|----------|------------------|
| `CartStore.put` | FR-1, FR-5, NFR-2 | "customers lose carts on crash" |
| `CartService.restore` | FR-2, FR-3 | "customers lose carts on crash" |
| retry/backoff in `CartStore` | FR-4 | resilience under storage blips |

## Gaps
- Requirements with no test: **none** (all FR/NFR mapped). ✅
- Code with no requirement: **none**. ✅

*Verified boxes flip to ☑ as tests pass in CI; all must be ☑ for the spec to reach `done`.*
