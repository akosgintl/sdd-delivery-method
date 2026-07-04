---
artifact: spec
target: ./spec.md
round: 3
reviewed: 2026-07-04
verdict: changes-requested
reviewer: independent (fresh-context sub-agent — no access to prior reviews or the writer's rationale)
---

# Review of ./spec.md — round 03 (independent)

> Produced by a separate agent given only the spec + the `_shared` rulebook, not the round-01/02
> reviews. Round-02 (same-context) had *approved* this spec; the independent pass returned
> changes-requested with 2 BLOCKERs — another demonstration of the independence gap.

## Findings

- [BLOCKER] §7 · latent open question — partial-success handling left as an unresolved "A or B" hedge ("reconciled to one Order or a full reversal (design/ADR territory; flagged)") while `status: ready` and §10 says none · spec.md:90
  rule: spec checklist (hedge outside §10 while ready) + conventions (ready ⇒ empty Open Questions)
  fix: decide it now as a normative FR — "If the charge succeeds but the Order write fails, reverse the charge and create no Order."
  resolved: [x]

- [BLOCKER] §11 · latent open question — charge/reserve ordering and charge/Order-write atomicity left undecided ("vs", "an ADR is raised if the design surfaces a hard tradeoff") while `status: ready` · spec.md:108
  rule: spec checklist (hedge in §11 while ready) + conventions
  fix: resolve the *what*-level ordering (charge first; convert on success; reverse on Order-write failure) and record it concretely.
  resolved: [x]

- [MAJOR] FR-3 · compound: "create one Order **and** convert each Hold to a Reservation" · spec.md:37
  rule: EARS (two distinct behaviors → split)
  fix: FR-3 = create exactly one Order; new FR = convert each Hold to a Reservation.
  resolved: [x]

- [MAJOR] FR-5 · compound: a distinct reporting behavior ("shall report which seats were lost") bolted onto the reject/prohibition pair · spec.md:41
  rule: EARS (only "shall X and shall not Y" about the same behavior is sanctioned)
  fix: keep reject + not-charge as one FR; split the notification into its own FR.
  resolved: [x]

- [MAJOR] FR-6 · compound: "keep the Holds until their normal expiry" joined with the cancel/prohibition pair · spec.md:43
  rule: EARS (distinct behaviors under one ID)
  fix: split the Hold-retention behavior into its own FR.
  resolved: [x]

- [MAJOR] NFR-1 · internal contradiction — two latency bounds on the same measured quantity ("95% within 3 s, excluding PSP" and "p99 < 800 ms own-processing") · spec.md:50
  rule: spec checklist (two normative lines fixing the same value differently)
  fix: state one coherent budget for own-processing time and mirror it in the acceptance line.
  resolved: [x]

- [MINOR] FR-2 · the "Successful payment" scenario never observes "charged exactly the displayed total" · spec.md:65
  rule: gherkin (Then must observe the requirement)
  fix: add a Then asserting the buyer was charged exactly the displayed itemized total.
  resolved: [x]

## Summary

- BLOCKER: 2 · MAJOR: 4 · MINOR: 1 — all resolved by the rewrite (see `spec.review-04.md`).

**Verdict: changes-requested.**
