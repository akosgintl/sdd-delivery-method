---
id: 0007-check-in
title: Check-in — validate each ticket exactly once at the door
status: ready
owner: eng-venue
created: 2026-07-03
updated: 2026-07-03
need: ../../discovery/prd-event-ticketing.md#6-feature-breakdown--specs
supersedes: null
---

# Specification: Check-in — validate each ticket exactly once at the door

> The precise, testable description of intended behavior — the contract.

## 1. Summary & context
At the door, each Attendee must be admitted exactly once against a valid Reservation, even when the
venue's network is unreliable. This feature validates a ticket, marks it checked-in, and prevents the
same ticket from admitting two people. Terms Check-in (the act), Attendee, Reservation are defined in
the glossary.

## 2. Goals
- Admit an Attendee exactly once per valid Reservation.
- Keep the door moving when the network is intermittent, without allowing double-admission beyond a
  bounded, quantified tolerance.

## 3. Non-goals (out of scope)
- Physical turnstile / gate hardware (PRD non-goal — the platform validates, it does not drive gates).
- Selling or refunding tickets at the door (specs `0004`/`0006`).
- Identity verification of the Attendee beyond the ticket credential.

## 4. Functional requirements
- **FR-1:** When a scanner submits a ticket credential for a valid, un-checked-in Reservation, the
  system shall mark it `checked-in` and shall return an admit result.
- **FR-2:** If a scanner submits a credential for a Reservation already `checked-in`, then the system
  shall return a reject result and shall not admit again.
- **FR-3:** If a scanner submits a credential that matches no valid Reservation, then the system shall
  return a reject result.
- **FR-4:** If a scanner submits a credential for a refunded or cancelled Reservation, then the system
  shall return a reject result.
- **FR-5:** Where the scanner device is offline, the system shall permit local admit decisions against
  a downloaded manifest and shall queue each decision for reconciliation.
- **FR-6:** When an offline device reconnects, the system shall reconcile its queued decisions and
  shall flag any credential admitted on more than one device as a double-admission conflict.
- **FR-7:** The system shall record every check-in attempt (admit, reject, offline-admit) with its
  Reservation, device, and timestamp.

## 5. Non-functional requirements
- **NFR-1:** 95% of online validation responses shall return within 500 ms.
- **NFR-2:** In online mode, the system shall admit a given Reservation on at most one device — a
  test scanning the same ticket on 10 devices simultaneously shall record exactly 1 admit.
- **NFR-3:** Offline double-admissions shall be detectable within 60 s of device reconnection and
  reported per NFR-2's conflict rule; the offline tolerance window shall be configurable and default
  to devices reconnecting at least every 15 min.
- **NFR-4:** The manifest downloaded to a device shall contain no payment data (only credential and
  seat/Reservation identifiers).

## 6. Acceptance criteria / scenarios

```gherkin
Scenario: First scan admits                                    # verifies FR-1
  Given a valid un-checked-in Reservation
  When its credential is scanned
  Then the result shall be admit and the Reservation shall be checked-in

Scenario: Second scan is rejected                              # verifies FR-2, NFR-2
  Given a Reservation already checked-in
  When its credential is scanned again
  Then the result shall be reject and no second admission shall occur

Scenario: Offline admit is queued and reconciled               # verifies FR-5, FR-6
  Given a device is offline with a downloaded manifest
  When it admits a credential locally
  And the device later reconnects
  Then the decision shall be reconciled and any cross-device duplicate flagged as a conflict
```
- [ ] Unknown credential is rejected (FR-3).
- [ ] Refunded/cancelled Reservation is rejected (FR-4).
- [ ] Same ticket on 10 online devices yields exactly 1 admit (NFR-2).
- [ ] Offline duplicates detected within 60 s of reconnection (NFR-3).
- [ ] Every attempt is logged with device and timestamp (FR-7).
- [ ] Manifest carries no payment data (NFR-4).
- [ ] 95% of online validations return < 500 ms under door load (NFR-1).

## 7. Edge cases & error behavior
- **Same ticket, two online scanners at once:** exactly one admit (FR-2/NFR-2).
- **Two offline devices admit the same ticket:** both admit locally; conflict flagged on reconnect (FR-6/NFR-3).
- **Refund lands while Attendee is at the door:** credential rejected if reservation released (FR-4).
- **Device never reconnects:** its admits stay unreconciled and are surfaced in an exceptions report.

## 8. Data & interfaces
- CheckInRecord = {`reservationId`, `deviceId`, `result` ∈ {admit, reject, offline-admit}, `at`}.
- Manifest = list of {`credential`, `reservationId`, `seatId`} with no payment fields.

## 9. Dependencies & assumptions
- Dependencies: `0004-checkout-payment` (Reservations), `0006-refunds-cancellation` (cancellation
  state), the inventory/reservation store.
- Assumptions: door devices reconnect periodically; a bounded offline window is acceptable to the organizer.

## 10. Open questions
(none)

## 11. Rationale / decisions
- Allowing bounded offline admission (availability) at the cost of possible, detectable double-admits
  (consistency) is a significant tradeoff → **ADR candidate** if feature 0007 goes to design.
