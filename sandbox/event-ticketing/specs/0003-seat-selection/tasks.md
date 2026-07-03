---
id: 0003-seat-selection
artifact: tasks
status: ready
updated: 2026-07-03
spec: ./spec.md
design: ./design.md
---

# Task Breakdown: Seat selection with exclusive Holds

> Ordered, dependency-aware decomposition of the design into independently verifiable tasks.

## Legend
- **ID** `T-n` · **Dep** depends-on · **Req** requirement(s) advanced · **Est** S/M/L · `[P]` parallelizable

| ID | Task | Dep | Req | Est | Status |
|----|------|-----|-----|-----|--------|
| T-1 | Define `seat`/`hold`/`capacity`/`outbox` schema + CAS primitive on seat state | — | FR-1, FR-5 | M | todo |
| T-2 | Implement `POST /holds` select endpoint: CAS available→held, idempotency key, reject held/reserved | T-1 | FR-1, FR-2, FR-8 | M | todo |
| T-3 | Implement availability read model with lazy-expiry check [P] | T-1 | FR-3 | S | todo |
| T-4 | Implement hold sweeper (≤2 s) releasing expired holds + emit `seat.released` via outbox | T-1 | FR-4 | M | todo |
| T-5 | Implement `POST /holds/{id}/convert` hold→reservation (called by 0004) | T-2 | FR-6 | S | todo |
| T-6 | Implement `DELETE /holds/{id}` explicit release [P] | T-2 | FR-9 | S | todo |
| T-7 | Implement capacity-only counter path (atomic decrement, floor 0) [P] | T-1 | FR-7 | M | todo |
| T-8 | Hold-duration config: default 600 s, bounds 120–1,800 s [P] | T-1 | NFR-4 | S | todo |
| T-9 | Concurrency test: 1,000 parallel selects on one seat → exactly 1 Hold | T-2 | FR-2, FR-5, NFR-2 | M | todo |
| T-10 | Timing test with injected clock: expiry release within 5 s | T-4 | FR-4, NFR-3 | S | todo |
| T-11 | Load test: 500 concurrent buyers/event, p95 select < 300 ms under sweep | T-2,T-3,T-4 | NFR-1 | M | todo |
| T-12 | Acceptance suite referencing FR IDs (`__FR1`..`__FR9`) | T-2,T-3,T-4,T-5,T-6,T-7 | FR-1..FR-9 | M | todo |

## Critical path
T-1 → T-2 → T-5 → T-12 (schema → select → convert → acceptance). The expiry chain T-1 → T-4 → T-10
runs alongside.

## Parallelizable
T-3, T-6, T-7, T-8 can proceed alongside the critical path once T-1 (T-6 after T-2) is done.

## Notes
- CAS granularity is per-seat (not per-event) per design §8 to limit hot-row contention.
- No design decisions here — hold-expiry strategy is settled in ADR-0001.
