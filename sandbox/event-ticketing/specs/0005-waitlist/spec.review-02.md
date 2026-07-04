---
artifact: spec
target: ./spec.md
round: 2
reviewed: 2026-07-04
verdict: changes-requested
reviewer: independent (fresh-context sub-agent — no access to review-01 or the writer's rationale)
---

# Review of ./spec.md — round 02 (independent)

> This round was produced by a separate agent given only the spec + the `_shared` rulebook, **not**
> the round-01 review or the author's intent. It found a BLOCKER and 4 MAJORs that the same-context
> round-01 self-review missed (round-01 verdict was *approved* with a single MINOR). This is the
> reviewer-independence demonstration.

## Findings

- [BLOCKER] §7 · unresolved design choice "second join rejected **or** deduplicated (design; flagged)" is an open question outside §10 while `status: ready` · spec.md:86
  rule: spec checklist ("Anything in §10 Open questions while ready/done" + conventions.md "a ready/done spec MUST have an empty Open Questions section and no TBD")
  fix: resolve the duplicate-join behavior now with an EARS FR (reject the second join, keep the existing entry's position), give it a criterion, and delete the hedge — or drop the spec back to `draft`.
  resolved: [x]

- [MAJOR] FR-5 · compound: joins two distinct behaviors with "and" ("expire the offer **and** extend the next offer") · spec.md:41
  rule: ears.md ("'and' — compound; split"); conventions.md (two behaviors → two requirements)
  fix: FR-5 = expire the offer at window end; a new FR = extend an offer to the next buyer. Split the scenario at spec.md:71-74 so each is verified.
  resolved: [x]

- [MAJOR] FR-6 · compound: "convert the held inventory into a Hold ... **and** route them to checkout" · spec.md:43
  rule: ears.md; conventions.md (singular IDs)
  fix: FR-6 = convert to a Hold; a new FR = route the buyer to checkout. Update the FR-6 criterion (spec.md:76) to cite both.
  resolved: [x]

- [MAJOR] FR-7 · compound trigger + ambiguous target ("When a buyer accepts **or** the Waitlist is exhausted ... remove satisfied **or** offered buyers") · spec.md:45
  rule: ears.md ("and/or — compound or open-ended; split or close it")
  fix: split by trigger — FR = on accept, remove that buyer; separate FR = on exhausted list with inventory left, return it to open sale. Replace "satisfied or offered" with the precise entry state(s).
  resolved: [x]

- [MAJOR] FR-5 / NFR-3 · internal contradiction: FR-5 hard-codes a "300 s" window while NFR-3 makes the window configurable per event (60–900 s) · spec.md:41, spec.md:54
  rule: spec checklist (internal consistency / single source of truth)
  fix: FR-5 refers to "its offer window"; keep the 300 s default + 60–900 s range owned solely by the NFR.
  resolved: [x]

- [MINOR] NFR-3 · conflates a fixed default and a configurable range in one line · spec.md:54
  rule: ears.md/singularity
  fix: split into "default offer window shall be 300 s" and "shall be configurable within [60 s, 900 s]".
  resolved: [x]

- [MINOR] §8 · WaitlistEntry.state {waiting, offered, accepted, expired} has no terminal state for buyers removed (FR-7) or for a rejected duplicate join · spec.md:89
  rule: spec checklist (data model must cover the behaviors the FRs assert)
  fix: add `removed`/`rejected` states once those FRs are resolved.
  resolved: [x]

## Summary

Front-matter, status vocab, banned-word scan, and 1:1 acceptance coverage are clean; FR-4's
"shall hold ... and shall not offer" is the *sanctioned* prohibition pairing, not a defect. But the
duplicate-join hedge is a BLOCKER at `ready`, three FRs are compound, and FR-5 contradicts NFR-3.

- BLOCKER: 1 · MAJOR: 4 · MINOR: 2

**Verdict: changes-requested.**
