---
artifact: glossary
target: ./glossary.md
round: 1
reviewed: 2026-07-03
verdict: changes-requested
---

# Glossary lint — round 01 (proposals)

Scanned: specs 0001–0007. The 12 seeded terms (Hold, Reservation, Order, Waitlist, Queue, Seat
allocation, Idempotent, Ticket type, Attendee, Buyer, Check-in, Refund) are all used consistently —
the Hold/Reservation/Order and Waitlist/Queue distinctions hold across every spec. Below are
contested terms used **normatively** but not yet defined. Each is appended to the glossary as
`status: proposed` for human confirmation (promote via `glossary-writer`).

## Findings (proposed new entries)

- [MAJOR] term "Offer" · used normatively across 0005 waitlist (FR-3..FR-7, "offer window") but undefined; could be confused with a price offer/discount · specs/0005-waitlist/spec.md:34
  rule: glossary checklist — undefined contested term used normatively
  fix (proposed def): "Offer — a time-limited, exclusive right extended to the front buyer of a Waitlist to claim released inventory before it passes to the next buyer. Not a price discount." synonyms to avoid: not "invite", not "deal".
  resolved: [ ]

- [MAJOR] term "Manifest" · used normatively in 0007 check-in (FR-5, NFR-4, §8) but undefined · specs/0007-check-in/spec.md:38
  rule: glossary checklist — undefined contested term
  fix (proposed def): "Manifest — the list of valid credentials and their Reservation/seat identifiers downloaded to a door device for offline Check-in; contains no payment data." synonyms to avoid: not "guest list", not "roster".
  resolved: [ ]

- [MAJOR] term "Capacity-only event (general admission)" · used across 0001/0002/0003 as the counterpart to seated, but undefined · specs/0003-seat-selection/spec.md:23
  rule: glossary checklist — contested term, drift risk (seated vs GA vs capacity)
  fix (proposed def): "Capacity-only event — an event whose inventory is a remaining-count of unreserved admissions rather than specific seats; a.k.a. general admission (GA)." synonyms to avoid: not "unreserved seating", not "festival mode".
  resolved: [ ]

- [MINOR] term "On-sale window" · used normatively in 0001 (FR-6) and referenced by others · specs/0001-event-setup/spec.md:37
  rule: glossary checklist — borderline; define if reused
  fix (proposed def): "On-sale window — the `[onSaleStart, onSaleEnd)` interval during which a published event accepts seat selections." synonyms to avoid: not "sale period", not "booking window".
  resolved: [ ]

## Summary

- Undefined contested terms proposed: 3 MAJOR (Offer, Manifest, Capacity-only event), 1 MINOR (On-sale window).
- No hedged/circular definitions found. No synonym drift against the 12 seeded terms.
- No stale entries (all 12 are referenced by ≥1 spec).

**Verdict: changes-requested** — proposals appended as `status: proposed`; a human promotes accepted
ones via `glossary-writer`.
