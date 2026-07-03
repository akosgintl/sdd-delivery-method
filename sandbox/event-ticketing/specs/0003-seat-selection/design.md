---
id: 0003-seat-selection
artifact: design
status: agreed
owner: eng-inventory
updated: 2026-07-03
spec: ./spec.md
---

# Technical Design: Seat selection with exclusive Holds

> How we satisfy `spec.md`. Behavior lives in the spec; this is the how.

## 1. Overview
Seat state lives in one authoritative inventory store (constitution A-1). Selection is a
compare-and-set (CAS) that flips a seat `available → held` and writes a Hold row with an absolute
`expiresAt`; exactly one concurrent CAS wins, giving provable no-double-book. Expiry is enforced by a
lazy check on every read plus a ≤2 s background sweep that releases expired Holds and emits
`seat.released`. Capacity-only events apply the same CAS to a decremented counter instead of a seat
row. The strategy is fixed by **[ADR-0001](../../adr/0001-hold-expiry-strategy.md)**.

## 2. Approach & alternatives considered
| Option | Pros | Cons | Decision |
|--------|------|------|----------|
| CAS on authoritative store + sweep (ADR-0001) | single source of truth; provable exclusivity; bounded release | sweep load; hot-row contention | ✅ chosen |
| Sliding-window Holds | friendlier to slow buyers | seats held indefinitely; hurts sell-through | rejected (ADR-0001) |
| External distributed lock as source of truth | fast locks | second source of truth → split-brain double-book (A-1) | rejected (ADR-0001) |
| Reserve-on-select (no Hold) | simplest state | forces pay-before-commit; contradicts 0004 | rejected (ADR-0001) |

Significant decision recorded as **ADR-0001** (accepted).

## 3. Components & responsibilities
- **Inventory service** — sole writer of seat state (A-1); exposes `select`, `release`, `convert`;
  owns the CAS and the Hold table.
- **Hold sweeper** — background worker; every ≤2 s flips `expiresAt < now` Holds to `available`,
  emits `seat.released` via the outbox. Realizes FR-4, NFR-3.
- **Availability read model** — serves seat maps applying the lazy-expiry check so other buyers never
  see an expired Hold as taken. Realizes FR-3.
- **Capacity counter** — for capacity-only events, an atomic decrement/increment realizing FR-7.

## 4. Interfaces & contracts
- `POST /events/{id}/holds {seatIds[], idempotencyKey}` → Hold — realizes **FR-1, FR-8**; idempotent
  per P-2. Rejects held/reserved seats (409) — **FR-2, FR-5**.
- `DELETE /holds/{holdId}` → 204 — realizes **FR-9**.
- `POST /holds/{holdId}/convert` (called by 0004 checkout) → Reservation — realizes **FR-6**.
- Event `seat.released {seatId, eventId, releasedAt}` (outbox) — realizes **FR-4**, consumed by 0005.
- Seat state machine: `available ⇄ held → reserved`; `held → available` on expiry/release.

## 5. Data model
- `seat(event_id, seat_id, state ∈ {available,held,reserved}, version)` — CAS on `(state, version)`.
- `hold(hold_id, event_id, seat_id, buyer_id, expires_at, idempotency_key UNIQUE)`.
- `ticket_type_capacity(ticket_type_id, remaining)` — atomic decrement floored at 0 (FR-7, ties 0002).
- `outbox(id, topic, payload, published_at)` — reliable `seat.released`.

## 6. Integration points & dependencies
- 0002 (inventory counts), 0004 (convert on checkout), 0005 (consumes `seat.released`).
- Single authoritative clock = store's `now()`; app nodes never judge expiry (mitigates skew).

## 7. Test strategy
- **FR-1** → integration: select available seat → Hold exists with future `expiresAt`.
- **FR-2, FR-5, NFR-2** → concurrency test: 1,000 parallel selects on one seat → exactly 1 Hold, 999
  rejects (`__FR2`, `__NFR2`).
- **FR-3** → integration: buyer B's seat-map read excludes A's held seat.
- **FR-4, NFR-3** → time test with injected clock: Hold past `expiresAt` released within 5 s; sweep
  emits `seat.released`.
- **FR-6** → integration: convert flips `held → reserved`.
- **FR-7** → concurrency test on capacity counter: last unit → exactly one Hold; floor at 0.
- **FR-8** → integration: 11th seat in one order rejected.
- **FR-9** → integration: explicit release returns seat before expiry.
- **NFR-1** → load test: 500 concurrent buyers/event, p95 select < 300 ms under sweep load.
- **NFR-4** → config test: default 600 s; accepts 120–1,800 s, rejects outside.

## 8. Risks & mitigations
| Risk | Likelihood | Impact | Mitigation |
|------|-----------|--------|------------|
| Hot-event inventory row contention | Med | High | per-seat CAS (not per-event); shard capacity counter |
| Sweep load at scale | Med | Med | index on `expires_at`; batch releases |
| Clock skew between nodes | Low | High | single authoritative store clock only |
| Outbox lag delays waitlist offers | Low | Med | monitor outbox age; alert > 10 s |

## 9. Rollout & operability
- Feature-flag `seat_holds_v2`; dark-launch sweep in shadow before enforcing.
- Rollback: disable flag → fall back to reserve-on-select path (degraded, but no double-book).
- Observability: metrics `holds_active`, `hold_cas_conflicts`, `sweep_lag_ms`, `seat_released_total`;
  alert on `sweep_lag_ms` p99 > 5 s (guards NFR-3) and CAS conflict-rate spikes.
