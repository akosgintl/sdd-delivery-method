# Glossary — Ubiquitous Language (Tessera event-ticketing)

> Agreed definitions of domain terms, shared by specs, code, tests, and AI agents. Ambiguity in
> terms is ambiguity in the spec. Lives at a fixed, well-known path.

| Term | Definition | Notes / synonyms to avoid |
|------|------------|---------------------------|
| Hold | A short-lived, exclusive claim on one or more specific seats while an attendee completes checkout; auto-released when its timer expires. | not "reservation", not "lock", not "booking" |
| Reservation | A confirmed, paid allocation of specific seats to an attendee that persists until the event or a refund. | not "hold", not "booking" |
| Order | The commercial record of a purchase: the line items, prices, fees, and payment for one checkout. Distinct from the seats it grants. | not "booking", not "cart", not "reservation" |
| Waitlist | An ordered, first-come-first-served list of attendees requesting a sold-out ticket type, from which offers are made when inventory frees up. | not "queue" (which we reserve for the traffic/virtual waiting room) |
| Queue | The pre-sale virtual waiting room that throttles concurrent traffic into the buying flow. Not the same as a Waitlist. | not "waitlist", not "line" |
| Seat allocation | The act of moving a specific seat from `available` to `held` or `reserved` for one attendee. | not "assignment", not "booking" |
| Idempotent | An operation that, when retried with the same idempotency key, produces at most one effect (one charge, one allocation) regardless of how many times it is received. | not "repeatable", not "safe" |
| Ticket type | A named, priced category of admission for an event (e.g. "General Admission", "VIP"), with its own inventory count. | not "ticket class", not "tier" (alone) |
| Attendee | The person who will be admitted with a ticket; may differ from the buyer who paid. | not "user", not "customer", not "guest" |
| Buyer | The account that placed and paid for an Order; may purchase for other Attendees. | not "user", not "customer" |
| Check-in | The act of admitting an Attendee at the venue by validating their ticket exactly once. | not "scan" (the scan is the mechanism), not "admission" |
| Refund | The reversal of all or part of an Order's payment, which releases the associated Reservation back to inventory. | not "cancellation" (cancellation is the buyer's request; the refund is the money movement) |
| Offer | A time-limited, exclusive right extended to the front Buyer of a Waitlist to claim released inventory before it passes to the next Buyer. Not a price discount. | not "invite", not "deal" |
| Manifest | The list of valid credentials and their Reservation/seat identifiers downloaded to a door device for offline Check-in; contains no payment data. | not "guest list", not "roster" |
| Capacity-only event | An event whose inventory is a remaining-count of unreserved admissions rather than specific seats; a.k.a. general admission (GA). | not "unreserved seating", not "festival mode" |
| On-sale window | The `[onSaleStart, onSaleEnd)` interval during which a published event accepts seat selections. | not "sale period", not "booking window" |

*Rule: if a word in a spec could mean two things to two readers, define it here or replace it.*

<!-- Proposed entries (appended by glossary-maintainer, awaiting confirmation) carry `status: proposed`. -->

<!-- glossary.review-01 proposals (Offer, Manifest, Capacity-only event, On-sale window) accepted and promoted into the table above on 2026-07-03. -->

