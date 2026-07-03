---
artifact: gate
gate: DoR
target: ./ (specs 0001–0007)
round: 1
reviewed: 2026-07-03
verdict: pass
---

# DoR gate — round 01 (all 7 specs)

Evaluated against `../memory/definition-of-ready.md`.

| Spec | Front-matter | FRs EARS/testable | NFRs quantified | Acceptance 1:1 | Non-goals | §10 empty | Glossary-aligned | Verdict |
|------|:---:|:---:|:---:|:---:|:---:|:---:|:---:|:---:|
| 0001-event-setup | ✅ | ✅ | ✅ | ✅ (NFR-3 fixed r2) | ✅ | ✅ | ✅ | **pass** |
| 0002-ticket-types-pricing | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ | **pass** |
| 0003-seat-selection | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ (r2 fix) | **pass** |
| 0004-checkout-payment | ✅ | ✅ | ✅ | ✅ (NFR-4 fixed r2) | ✅ | ✅ | ✅ | **pass** |
| 0005-waitlist | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ | **pass** |
| 0006-refunds-cancellation | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ | **pass** |
| 0007-check-in | ✅ | ✅ | ✅ | ✅ (NFR-1 fixed r2) | ✅ | ✅ | ✅ | **pass** |

## Notes
- All specs are `status: ready` with empty Open Questions (DoR-8 satisfied; no `TBD`).
- Each `need:` traces to the approved PRD feature table (DoR-1).
- Contested terms (Hold/Reservation/Order, Waitlist/Queue, Offer, Manifest, Capacity-only event)
  resolve against the glossary (DoR-9) — Offer/Manifest/Capacity-only/On-sale window were added by the
  glossary-maintainer scan and promoted before this gate.
- Money/inventory failure paths (double-book, hold expiry, payment timeout, duplicate scan) appear in
  each spec's §7 (DoR-6).

**Verdict: pass** — all 7 specs are Ready to enter design/build. No blocking criteria.
