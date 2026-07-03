---
id: 0006-refunds-cancellation
artifact: traceability
updated: 2026-07-03
spec: ./spec.md
---

# Traceability Matrix: Refunds and cancellation with automatic seat release

> Generated from stable IDs across spec + tasks. No code exists yet, so Code/Verified are empty and
> Gaps lists every requirement as not-yet-tested — the expected pre-build state.

## Forward trace (coverage: is every requirement built & tested?)

| Req ID | Requirement (short) | Source need | Task(s) | Test(s) | Code | Verified |
|--------|---------------------|-------------|---------|---------|------|:--------:|
| FR-1 | reverse line payment | PRD §6 #6 | T-1,T-5 | `test_full_refund_reverses__FR1` | — | ☐ |
| FR-2 | release seat in same txn | PRD §6 #6 | T-3 | `test_refund_releases_seat__FR2` | — | ☐ |
| FR-3 | idempotent refund | PRD §6 #6 | T-2,T-9 | `test_duplicate_refund_once__FR3` | — | ☐ |
| FR-4 | partial refund | PRD §6 #6 | T-4 | `test_partial_refund__FR4` | — | ☐ |
| FR-5 | timeout keeps seat, pending | PRD §6 #6 | T-6,T-10 | `test_billing_timeout_holds__FR5` | — | ☐ |
| FR-6 | audit every attempt | PRD §6 #6 | T-7 | `test_refund_audit_log__FR6` | — | ☐ |
| FR-7 | reject double refund | PRD §6 #6 | T-2 | `test_double_refund_rejected__FR7` | — | ☐ |
| FR-8 | emit seat.released | PRD §6 #6 | T-8 | `test_seat_released_event__FR8` | — | ☐ |
| NFR-1 | 95% response < 800 ms | PRD §6 #6 | T-11 | `perf_refund_p95__NFR1` | — | ☐ |
| NFR-2 | no reversed-but-reserved state | PRD §6 #6 | T-10 | `test_atomic_no_halfstate__NFR2` | — | ☐ |
| NFR-3 | no PAN, token-only | PRD §6 #6 | T-5,T-12 | `scan_no_pan__NFR3` | — | ☐ |
| NFR-4 | dup key → 1 reversal | PRD §6 #6 | T-9 | `test_idempotent_100x__NFR4` | — | ☐ |

## Backward trace (justification: why does this exist?)

| Code / module | Realizes | Originating need |
|---------------|----------|------------------|
| _(none yet — feature not built)_ | — | — |

## Gaps
- **Requirements with no implemented test:** FR-1..FR-8, NFR-1..NFR-4 (all) — planned in tasks
  (T-9..T-13), not yet written. Must be empty before `done`.
- **Code with no requirement:** none (no code yet).
