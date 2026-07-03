---
id: 0006-refunds-cancellation
artifact: tasks
status: ready
updated: 2026-07-03
spec: ./spec.md
design: ./design.md
---

# Task Breakdown: Refunds and cancellation with automatic seat release

> Ordered, dependency-aware decomposition of the design into independently verifiable tasks.

## Legend
- **ID** `T-n` · **Dep** depends-on · **Req** requirement(s) advanced · **Est** S/M/L · `[P]` parallelizable

| ID | Task | Dep | Req | Est | Status |
|----|------|-----|-----|-----|--------|
| T-1 | Define `refund`/`refund_line`/`audit_log`/`outbox` schema + refund state machine | — | FR-1 | M | todo |
| T-2 | Implement `POST /orders/{id}/refunds`: idempotency key, reject already-refunded line | T-1 | FR-3, FR-7 | M | todo |
| T-3 | Implement local transaction: status→succeeded + seat release, atomic | T-1 | FR-2 | M | todo |
| T-4 | Implement partial (line-scoped) refund | T-2,T-3 | FR-4 | S | todo |
| T-5 | Billing-adapter `reverse(token, amount, key)` — idempotent, no PAN | T-1 | FR-1, NFR-3 | M | todo |
| T-6 | Reconciler: retry pending reversals; 30 s no-confirm keeps seat reserved, refund pending | T-3,T-5 | FR-5 | M | todo |
| T-7 | Audit-log every attempt (requested/pending/succeeded/failed) [P] | T-1 | FR-6 | S | todo |
| T-8 | Emit `seat.released` via transactional outbox [P] | T-3 | FR-8 | S | todo |
| T-9 | Idempotency test: same key ×100 → exactly 1 reversal | T-2,T-5 | FR-3, NFR-4 | M | todo |
| T-10 | Fault-injection test: PSP timeout → no reversed-but-reserved state observable | T-6 | FR-5, NFR-2 | M | todo |
| T-11 | Load test: 95% accepted/pending response < 800 ms excl. settlement | T-2,T-3,T-5 | NFR-1 | M | todo |
| T-12 | Data-catalog scan: no PAN at rest, token-only [P] | T-5 | NFR-3 | S | todo |
| T-13 | Acceptance suite referencing FR IDs (`__FR1`..`__FR8`) | T-3,T-4,T-6,T-7,T-8 | FR-1..FR-8 | M | todo |

## Critical path
T-1 → T-5 → T-6 → T-10 (schema → billing adapter → reconciler → atomicity fault test). The local
release chain T-1 → T-3 → T-13 runs alongside.

## Parallelizable
T-7, T-8, T-12 can proceed alongside the critical path once their deps are met.

## Notes
- Atomicity strategy (local txn + async reconcile) is settled in ADR-0002 — not re-decided here.
- `reversed_but_reserved_total` metric (design §9) is the runtime guard for NFR-2; T-10 is the test.
