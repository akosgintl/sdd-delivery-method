---
id: NNNN-feature-slug
artifact: traceability
updated: YYYY-MM-DD
spec: ./spec.md
---

# Traceability Matrix: <title>

> Links need → requirement → acceptance → test → code. For most teams this is **generated** from
> stable IDs referenced across artifacts; maintain it by hand only where audit/compliance requires
> explicit evidence.

## Forward trace (coverage: is every requirement built & tested?)

| Req ID | Requirement (short) | Source need | Task(s) | Test(s) | Code | Verified |
|--------|---------------------|-------------|---------|---------|------|:--------:|
| FR-1 | <persist cart on change> | PRD §2.1 | T-1,T-2 | `test_..._FR1` | `cart.persist()` | ☐ |
| FR-2 | <restore on reopen> | PRD §2.1 | T-3 | `test_..._FR2` | `cart.restore()` | ☐ |
| NFR-1 | <restore < 1s p95> | PRD §3 | T-6 | `perf_..._NFR1` | — | ☐ |

## Backward trace (justification: why does this exist?)

| Code / module | Realizes | Originating need |
|---------------|----------|------------------|
| `cart.persist()` | FR-1 | "customers lose carts on crash" |

## Gaps
- Requirements with no test: <list — must be empty for `done`>.
- Code with no requirement: <list — candidates for removal or a missing requirement>.
