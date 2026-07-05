---
artifact: adr
target: ./0002-refund-release-atomicity.md
round: 1
reviewed: 2026-07-03
verdict: approved
---

# Review of ADR-0002 — round 01

## Findings

(none blocking)

## Summary

One decision, unique number `ADR-0002`, Status `accepted`. Context names the hard constraint (no PSP
2PC) and why now. Decision is active voice and concrete (local txn for status+release; async PSP
reversal reconciled; transactional outbox). Four real rejected alternatives with why-not, including
the tempting-but-unsafe "release immediately". Consequences give positives **and** the honest
negative (a funds-reversed-before-resellable window, in the safe direction) plus follow-ups.
Immutability respected.

- BLOCKER: 0 · MAJOR: 0 · MINOR: 0

**Verdict: approved.**
