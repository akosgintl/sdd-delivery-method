---
id: 0006-refunds-cancellation
artifact: design
status: agreed
owner: eng-billing
updated: 2026-07-03
spec: ./spec.md
---

# Technical Design: Refunds and cancellation with automatic seat release

> How we satisfy `spec.md`. Behavior lives in the spec; this is the how.

## 1. Overview
A refund is a state machine `requested → pending → succeeded|failed`. The local status flip and the
seat release (constitution A-1) commit in **one local DB transaction**; the external PSP reversal is
an async, idempotent step reconciled by a background worker. On confirmation the release transaction
runs and emits `seat.released` via a transactional outbox. A PSP timeout leaves the refund `pending`
and the seat reserved — failing safe. Atomicity strategy fixed by
**[ADR-0002](../../adr/0002-refund-release-atomicity.md)**.

## 2. Approach & alternatives considered
| Option | Pros | Cons | Decision |
|--------|------|------|----------|
| Local txn (status+release) + async PSP reconcile (ADR-0002) | atomic local invariant; fails safe | funds-reversed-before-resellable window; needs reconciler | ✅ chosen |
| Release seat immediately on request | simple | timeout ⇒ seat freed but money not returned (unsafe) | rejected (ADR-0002) |
| Distributed 2PC PSP+inventory | strong consistency | PSPs offer no prepare/commit | rejected (ADR-0002) |
| Sync PSP call inside DB txn | fewer moving parts | multi-second lock hold; fails under load (Q-3) | rejected (ADR-0002) |

Significant decision recorded as **ADR-0002** (accepted).

## 3. Components & responsibilities
- **Refund service** — owns the refund state machine; runs the local status+release transaction;
  enforces the idempotency key. Realizes FR-1, FR-2, FR-3, FR-4, FR-7.
- **Billing adapter** (constitution A-3) — the only caller of the PSP; issues idempotent reversals by
  charge token. Realizes FR-1, NFR-3.
- **Reconciler** — background worker retrying `pending` reversals; on confirm, invokes the release
  transaction. Realizes FR-5.
- **Audit log writer** — appends every attempt. Realizes FR-6.
- **Outbox** — reliable `seat.released` emission. Realizes FR-8.

## 4. Interfaces & contracts
- `POST /orders/{id}/refunds {lineIds[], idempotencyKey}` → Refund — realizes **FR-1, FR-3, FR-4**;
  idempotent per P-2; rejects already-refunded line (409) — **FR-7**.
- Billing adapter `reverse(chargeToken, amountMinor, idempotencyKey)` → result — **FR-1, NFR-3**.
- Event `seat.released {seatId, eventId, releasedAt}` (outbox) — **FR-8**, consumed by 0005.
- Refund state: `requested → pending → succeeded | failed`; release runs only on `succeeded` (FR-2).

## 5. Data model
- `refund(refund_id, order_id, idempotency_key UNIQUE, status, psp_charge_token, created_at)`.
- `refund_line(refund_id, order_line_id, amount_minor)` — supports partial (FR-4).
- `reservation(... state)` — released within the same txn as `status=succeeded` (FR-2, NFR-2).
- `audit_log(id, order_id, refund_id, event, idempotency_key, at)` — FR-6.
- `outbox(...)` — FR-8.

## 6. Integration points & dependencies
- 0004 (Order/charge token), billing adapter/PSP (A-3), inventory service (A-1), 0005 (consumer).
- Assumes PSP reversal-by-token + idempotency-key support.

## 7. Test strategy
- **FR-1, FR-2** → integration: full refund → PSP reversed + seat available (single txn).
- **FR-3, NFR-4** → idempotency test: same key ×100 → exactly 1 reversal (`__FR3`, `__NFR4`).
- **FR-4** → integration: partial refund → only named line released, others reserved.
- **FR-5, NFR-2** → fault-injection: PSP no-confirm within 30 s → seat stays reserved, refund
  `pending`; assert **no** reversed-but-reserved state observable (`__NFR2`).
- **FR-6** → integration: every attempt appears in audit log with key.
- **FR-7** → integration: second refund of same line rejected, no re-release.
- **FR-8** → integration: release emits `seat.released` (outbox).
- **NFR-1** → load test: 95% accepted/pending response < 800 ms excluding settlement.
- **NFR-3** → data-catalog scan: no PAN stored; only token referenced.

## 8. Risks & mitigations
| Risk | Likelihood | Impact | Mitigation |
|------|-----------|--------|------------|
| Reconciler stalls, refunds stuck `pending` | Low | High | alert on `pending` age > 5 min; on-call runbook |
| Outbox duplicate `seat.released` | Med | Low | consumers (0005) idempotent on seatId+releasedAt |
| PSP idempotency key reuse across orders | Low | High | key namespaced by order+line |
| Partial-failure of local txn | Low | High | single DB txn; no cross-service write in it |

## 9. Rollout & operability
- Feature-flag `auto_seat_release`; enable reconciler first in shadow (log-only) then enforce.
- Rollback: disable flag → refunds still process, release falls back to a manual ops queue (degraded).
- Observability: metrics `refunds_pending`, `reversal_latency_ms`, `reversed_but_reserved_total`
  (must stay 0 — guards NFR-2), `seat_released_total`; alert if `reversed_but_reserved_total` > 0 or
  `refunds_pending` age p99 > 5 min.
