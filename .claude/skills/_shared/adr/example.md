# ADR-0008: Server-side cart store keyed by userId

> Architecture Decision Record. One decision per file. Immutable once accepted.

- **Status:** accepted
- **Date:** 2026-06-16
- **Deciders:** t.lead, architecture-guild
- **Related:** feature 0001-cart-persistence; constitution Q-4 (no PAN at rest)

## Context
Cart persistence (spec 0001) requires a cart to survive browser crashes and device restarts, be
restorable within 1 s p95 for 100-item carts (NFR-1), and be encrypted at rest with no payment-card
data (NFR-2). We must choose where the persisted cart lives. Client-only storage cannot guarantee
durability across device loss or encryption at rest.

## Decision
We will persist the cart **server-side in a durable per-user store, keyed by `userId`**, written on
every cart mutation and read on session start, with expiry computed at read time.

## Alternatives considered
- **Browser `localStorage`** — rejected: not encrypted at rest, lost on cache clear, not durable
  across device restart; fails NFR-2 and the need.
- **Cookie-based storage** — rejected: size limits and sent on every request (perf cost).

## Consequences
- **Positive:** durable, encryptable, satisfies NFR-1/NFR-2; enables later cross-device sync.
- **Negative / trade-offs:** a network dependency on the cart-write path; adds write latency that
  the 500 ms budget (FR-1) must accommodate (mitigated by async write + debounce).
- **Follow-ups:** capacity plan the store for peak sale traffic; revisit if write latency threatens
  FR-1.
