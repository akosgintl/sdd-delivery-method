---
id: 0003-seat-selection
artifact: traceability
updated: 2026-07-03
spec: ./spec.md
---

# Traceability Matrix: Seat selection with exclusive Holds

> Generated from stable IDs across spec + tasks. No code exists yet (feature not built), so Code and
> Verified are empty and Gaps lists every requirement as not-yet-tested — the expected pre-build state.

## Forward trace (coverage: is every requirement built & tested?)

| Req ID | Requirement (short) | Source need | Task(s) | Test(s) | Code | Verified |
|--------|---------------------|-------------|---------|---------|------|:--------:|
| FR-1 | select creates a Hold | PRD §6 #3 | T-1,T-2 | `test_select_creates_hold__FR1` | — | ☐ |
| FR-2 | reject already-held seat | PRD §6 #3 | T-2,T-9 | `test_race_one_winner__FR2` | — | ☐ |
| FR-3 | held seat hidden from others | PRD §6 #3 | T-3 | `test_held_seat_unavailable__FR3` | — | ☐ |
| FR-4 | hold expiry releases seat | PRD §6 #3 | T-4,T-10 | `test_expiry_release__FR4` | — | ☐ |
| FR-5 | ≤1 claim per seat | PRD §6 #3 | T-1,T-9 | `test_no_double_alloc__FR5` | — | ☐ |
| FR-6 | hold→reservation on checkout | PRD §6 #3 | T-5 | `test_convert__FR6` | — | ☐ |
| FR-7 | capacity-only counter path | PRD §6 #3 | T-7 | `test_capacity_last_unit__FR7` | — | ☐ |
| FR-8 | per-order 10-seat limit | PRD §6 #3 | T-2 | `test_order_limit__FR8` | — | ☐ |
| FR-9 | explicit release | PRD §6 #3 | T-6 | `test_explicit_release__FR9` | — | ☐ |
| NFR-1 | p95 select < 300 ms @500 | PRD §6 #3 | T-11 | `perf_select_p95__NFR1` | — | ☐ |
| NFR-2 | exactly-one under 1,000 race | PRD §6 #3 | T-9 | `test_race_one_winner__NFR2` | — | ☐ |
| NFR-3 | release within 5 s of expiry | PRD §6 #3 | T-10 | `test_expiry_within_5s__NFR3` | — | ☐ |
| NFR-4 | default 600 s, bounds 120–1800 | PRD §6 #3 | T-8 | `test_hold_duration_bounds__NFR4` | — | ☐ |

## Backward trace (justification: why does this exist?)

| Code / module | Realizes | Originating need |
|---------------|----------|------------------|
| _(none yet — feature not built)_ | — | — |

## Gaps
- **Requirements with no implemented test:** FR-1..FR-9, NFR-1..NFR-4 (all) — tests are planned in
  tasks (T-9..T-12) but not yet written. Must be empty before `done`.
- **Code with no requirement:** none (no code yet).
