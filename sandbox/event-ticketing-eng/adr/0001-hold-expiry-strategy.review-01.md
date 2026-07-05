---
artifact: adr
target: ./0001-hold-expiry-strategy.md
round: 1
reviewed: 2026-07-03
verdict: approved
---

# Review of ADR-0001 — round 01

## Findings

(none blocking)

## Summary

One decision per file; globally unique number `ADR-0001`; Status `accepted` (legal). Context explains
the forces and why-now (concurrency correctness + timely release, blocks 0004). Decision is active
voice. Four real rejected alternatives each with a why-not (sliding window, reserve-on-select,
external lock as source-of-truth, TTL-only). Consequences list positives **and** negatives (sweep
load, row contention, lazy-check branch). Not a constitution exception, so no expiry needed.
Immutability respected.

- BLOCKER: 0 · MAJOR: 0 · MINOR: 0

**Verdict: approved.**
