# Critical review of the SDD skills + the sandbox run

> A skeptical second pass over both the `.claude/skills/` layer and the artifacts this sandbox
> produced. Written to correct the earlier `TEST-REPORT.md`, which graded everything green and was
> **too generous**. That report measured *format conformance* of artifacts I wrote and then had me
> review my own writing — it validated the scaffolding, not independent judgment.

## The headline

The skills' **structure is genuinely good**; the **run's methodology was weak**. One context played
writer → reviewer → rewriter, so the reviewer never knew anything the writer didn't. It rationalized
instead of judging, and the "MAJORs" it raised were chosen in advance to exercise the loop rather than
discovered. The proof is a set of real defects the self-review sailed past (below), several of which a
fresh-context reviewer should catch — that re-test is Part C.

## What went well (keep)

1. **The whole pipeline runs.** All 26 skills fired P0→P5 and produced internally consistent,
   cross-linked artifacts. Nothing in the scaffolding fell over.
2. **The domain earned its keep.** Concurrency (no-double-book), money precision, and contested
   vocabulary (Hold/Reservation/Order, Waitlist/Queue) exercised EARS If/Then, quantified NFRs, and
   the glossary the way a toy domain would not.
3. **ADRs are the strongest artifacts.** `adr/0002-refund-release-atomicity.md` rejects "release the
   seat immediately" as the *unsafe direction* and keeps an honest negative consequence (a
   funds-reversed-before-resellable window). That is real architectural reasoning, not a template
   fill.
4. **Designs tie every FR/NFR to a verify path** (`design.md §7` in both deep features), and the
   traceability pre-build semantics were handled correctly — which surfaced a **genuine study
   inconsistency** (`ready` vs `done` coverage) that we then fixed in `docs/09`/`docs/07`. Best
   outcome of the whole exercise, and it came from the traceability skill behaving well.
5. **Loop file mechanics are sound.** Sequence-numbered `review-NN.md`, findings checked off in place
   (never deleted), verdicts gated on unresolved BLOCKER/MAJOR, and the non-trio shapes
   (generator+audit, append-only ADR/constitution, writer+maintainer glossary, writer+runner gates)
   all behaved as designed.

## What went badly (the findings the TEST-REPORT missed)

1. **Self-review has no independence — the root cause.** Writer and reviewer shared one context and
   one intent. Every "finding" was something I already knew; nothing was *discovered*. This is the
   weakness that produced all the misses below, and the reason the green TEST-REPORT is not
   trustworthy as a *judgment* check.

2. **Latent open questions hiding in `ready` specs.** Per `conventions.md`, a `ready` spec must have an
   empty Open Questions section and no TBD. Yet unresolved choices sit in other sections while §10 says
   "(none)":
   - `0005-waitlist/spec.md:86` — "Buyer joins twice: second join rejected **or** deduplicated
     (design; flagged)".
   - `0004-checkout-payment/spec.md` §7 — "partial success … reconciled to one Order **or** a full
     reversal (design/ADR territory; flagged)".
   - `0001-event-setup/spec.md` §7 — "Editing a published event's start: allowed only while no
     Reservation exists (design detail; flagged here)".
   Each is a real undecided question wearing an edge-case's clothes. The spec-reviewer passed them and
   the **DoR gate passed them** — a gate whose whole job is to catch exactly this.

3. **Compound and soft FRs graded as discretionary MINORs.** EARS bans "and"-joined behaviors and
   unobservable responses, but reviews waved several through:
   - `0005/spec.md:41` FR-5 — "expire the offer **and** shall extend the next offer" = two distinct
     behaviors (expiry; re-offer), each needing its own test.
   - `0005/spec.md:45` FR-7 — "remove satisfied **or** offered buyers" — an "or" that hides two rules.
   - `0005/spec.md:33` FR-1 — "shall **offer to add** the buyer" — soft/unobservable verb.
   The reviews called these "author's discretion"; a stricter reviewer splits them.

4. **A dependency cycle no gate noticed.** `0003-seat-selection` depends on `0004-checkout-payment`
   (convert Hold→Reservation at checkout) while `0004` depends on `0003` (the Holds it converts); and
   `0005` depends on four siblings. The coupling is mediated by events so it is *workable*, but no
   spec, PRD, or gate acknowledges the cycle or names a build order — an integration risk left
   implicit.

5. **`updated` never advanced on rewrite.** The four rewritten specs (0001/0003/0004/0007) still show
   `updated == created`. Cosmetic because it was the same day, but it shows the rewriter had no
   `updated`-bump step at the time (since fixed as T-3).

6. **Minor "how" leaking into "what".** Specs name concrete mechanisms — event topics
   (`seat.released`) and flow ("route them to checkout", `0005` FR-6) — that belong in the design.
   Defensible under §8 "Data & interfaces", but the line was crossed a few times.

7. **The TEST-REPORT over-claimed.** "All green / No ❌ defects" is not faithful given items 2–6. The
   report is fine as a *format-conformance* check; it should never have been read as a quality verdict.

## Critique of the skills themselves (independent of this run)

- **No independence mechanism.** `sdd-loop` is prose the driving model executes in one context;
  nothing pushes the reviewer to fresh eyes. The design invites self-justification. → fix: independence
  guidance (Part B-3).
- **Reviewer severity is under-specified.** "acceptable as-is / author's discretion" is available for
  compounds and soft verbs, so a lenient reviewer never has to split them. → fix: sharpen EARS +
  spec-checklist calibration (B-1).
- **No spec self-consistency check.** Nothing tells a reviewer to reconcile §7/§8/§11 hedges against
  §10 and `status`. → fix: add that check (B-2); it is the single most valuable one.
- **Manual `_shared` ↔ study sync is fragile.** We literally hit it this session (had to patch
  `docs/09` after changing a skill). The obligation exists in prose but has no checklist. → fix:
  `SYNC.md` map (B-4).
- **`sdd-loop` round-counting/cap/"never fabricate approval" are unenforced.** Acceptable for a
  prose-driven tool, but worth stating the loop should log each round so the audit trail is explicit.

## Independent re-test (Part C) — reviewer independence, demonstrated

A fresh **general-purpose sub-agent** was given only `0005-waitlist/spec.md` + the `_shared` rulebook
+ the glossary — **not** my `spec.review-01.md` and none of my reasoning — and asked to review the
spec. The contrast is the whole point:

| | My self-review (`spec.review-01.md`, same context) | Independent re-review (`spec.review-02.md`, cold start) |
|---|---|---|
| Verdict | **approved** | **changes-requested** |
| BLOCKER | 0 | **1** |
| MAJOR | 0 | **4** |
| MINOR | 1 | 2 |

What the independent reviewer caught that my self-review **missed entirely**:
- **BLOCKER** — the "second join rejected **or** deduplicated (design; flagged)" latent open question
  in §7 while `status: ready`. My self-review approved the spec with this sitting in plain sight.
- **MAJOR** — FR-5, FR-6, FR-7 each compound (two behaviors under one ID).
- **MAJOR** — a real **internal contradiction**: FR-5 hard-coded a 300 s window while NFR-3 declared
  it configurable (60–900 s). I wrote both lines and never noticed they disagreed — the exact failure
  mode of self-review.

This is direct evidence that the run's green TEST-REPORT reflected format conformance, not judgment.
A separate reviewer, judging cold, produced a materially different — and correct — verdict.

**Loop closed with independence.** The independent findings drove a genuine rewrite: FR-5/FR-9,
FR-6/FR-11, and FR-7/FR-12 split the compounds; FR-10 resolves the duplicate-join question; NFR-3/NFR-4
separate the default from the range; the data model gains `removed`/`rejected` states. A round-03
re-review confirms `approved`, and the spec returns to `ready`. See `0005-waitlist/spec.review-02.md`
(independent), the rewritten `spec.md`, and `spec.review-03.md`.

**Takeaway (now baked into the skills):** `sdd-loop` and `workflow.md` were updated to require the
reviewer step to run with fresh eyes — ideally a separate sub-agent given only the artifact + rules.
It is the single highest-leverage improvement this exercise produced.

## Prioritized suggestions

| # | Suggestion | Why it matters | Effort |
|---|------------|----------------|:------:|
| 1 | **Spec self-consistency check** (§7/§8/§11 hedges vs §10 + status → BLOCKER at `ready`/`done`) | Catches the exact class of defect that slipped the DoR gate | S |
| 2 | **Run the reviewer as a separate agent** with only the artifact + rules | Removes the self-justification that made this run's reviews theatrical | S (guidance) |
| 3 | **Sharpen EARS/spec calibration** for compound + soft FRs (MAJOR, not discretionary) | Stops real testability defects being waved through | S |
| 4 | **`_shared` ↔ study `SYNC.md` map** | Makes the sync obligation checkable instead of memory | S |
| 5 | **Flag cross-feature cycles / build order** in PRD or design review | An integration risk currently left implicit | S |

Items 1–4 are implemented in this pass (see the skill diffs); item 5 is a one-line checklist addition.
