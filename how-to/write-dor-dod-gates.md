# How to write good DoR & DoD gates

The Definition of Ready (DoR) and Definition of Done (DoD) are the two **gates** that make SDD real:
DoR is the bar work must clear *before* construction, DoD is the bar it must clear to be called
complete. They are mirror images across the build phase — what DoR demands be *specified*, DoD demands
be *verified* — and the spec is the constant on both sides. Their power is also their fragility: a gate
only works if every item is **checkable** and the team actually applies it. A DoR full of aspirations
nobody verifies is the single most common reason SDD adoptions fail (teams write specs, build before
they're sound, and conclude "the docs didn't help"). This is the *doing* companion to
[`docs/06`](../docs/06-definition-of-ready.md) and [`docs/07`](../docs/07-definition-of-done.md).

> A gate item must answer **"how, concretely, do we check this?"** "The spec is clear" isn't a gate
> item; "the Open Questions section is empty and every FR has a test referencing its ID" is.

## When you write them

**Once per project**, owned collectively by the *whole team* — not handed down by a manager. They're
stable artifacts, revised in retrospectives (especially when reconciliation keeps finding spec-vs-build
divergence — that's a signal your DoR was too weak). You write the gates once; you *apply* them every
item: DoR at the Plan→Build transition, DoD at the Build→Done transition.

## Anatomy: the two gates, mirrored

| Definition of Ready (entry) | Definition of Done (exit) |
|-----------------------------|---------------------------|
| Spec exists, testable, agreed | Spec satisfied **and reconciled** |
| Acceptance criteria defined | Acceptance criteria demonstrated |
| NFRs specified (quantified) | NFRs verified against the numbers |
| Edge cases specified | Edge cases implemented & tested |
| Feasibility checked | Quality gates passed (tests, lint, security) |
| Stakeholders agree to build | Stakeholders accept the result |

**DoR** groups into: *intent clear · spec sound · feasible & bounded · agreed*
([template](../templates/definition-of-ready.md)). **DoD** adds the two clauses that define SDD and
that no ordinary agile DoD has ([template](../templates/definition-of-done.md), [`docs/07 §1`](../docs/07-definition-of-done.md#1-why-sdds-dod-is-special)):

1. **Verified against the spec** — every acceptance criterion demonstrably met, every `FR` covered by
   a test that references its ID, every `NFR` measured against its number. Done is defined by the
   *contract*, not the author's sense of completion.
2. **Spec reconciled** — any behavior decided or changed during construction is back in the spec, *in
   the same PR*. A merged behavior change with a stale spec is **not done**; it's "a lie with
   authority." This is the clause that prevents **spec rot**.

## The recipe

1. **Start from the templates, then cut to your team.** Keep DoR lean enough that meeting it is normal,
   not heroic — a gate that's too heavy gets skipped, and a skipped gate governs nothing.
2. **Make every item checkable.** For each, name *how* it's verified. Rewrite "the spec is clear" into
   mechanical checks: front-matter present, no open questions, every `FR` has an ID and a test.
3. **Put the SDD-specific DoD clauses in writing.** "Verified against the spec" and "spec reconciled in
   the same PR" are the two clauses teams drop — and dropping them is exactly how spec rot sets in.
   Make them non-negotiable.
4. **Automate the mechanical items; reserve humans for judgment.** CI can enforce spec-lint (no `TBD`
   in a `done` spec), requirement-coverage (every `FR` referenced by a test), a spec-touch check
   (behavior-bearing code changed but no `spec.md`?), and the usual test/lint/security gates. Humans
   keep *acceptance* and *"does the spec genuinely describe reality?"*.
5. **Calibrate to risk, not by deleting items.** Per [P8](../docs/01-principles.md#p8--specify-in-proportion-to-risk),
   the items stay the same; their *depth* scales. A one-line bug fix still has a Ready bar (is the
   intended behavior testable and agreed?) and a Done bar (verified, and the doc describing the old
   behavior updated) — the clauses shrink, they never vanish.
6. **Add the AI-assisted items if agents implement work.** DoR: the spec is self-contained, interfaces
   explicit, acceptance criteria mechanically checkable. DoD: a human reviewed the output *against the
   spec*, deviations are reconciled, and generated tests verify the spec rather than mirror the code.
7. **Define what "not Ready" does.** Not-Ready is healthy — it means the gate works. Write the
   responses: *refine, spike, split, or defer* — never "start anyway and clarify in flight."

## Before → After

**Unverifiable gate item → checkable gate item**

> ✗ *Before (DoR):* "The story is well understood and the spec is clear."
> *(Nobody can fail this objectively — "well understood" and "clear" have no check. It will be waved
> through every time.)*
>
> ✓ *After (DoR):*
> - [ ] A `spec.md` exists; every `FR` has a stable ID and is singular, unambiguous, testable.
> - [ ] NFRs quantified; acceptance criteria map 1:1 to requirements.
> - [ ] Open Questions section is **empty**; terms align with the glossary.
> - [ ] Reviewed by engineering **and** the product owner.

**Ordinary DoD → SDD DoD**

> ✗ *Before (DoD):* "Code reviewed, tests pass, merged." *(True of any agile team — and it lets a
> behavior change merge with a spec that now lies about the system.)*
>
> ✓ *After (DoD), adding the two SDD clauses:*
> - [ ] Every acceptance criterion demonstrated; every `FR` covered by a test referencing its ID;
>   every `NFR` measured against its number.
> - [ ] The spec reflects what was actually built — any changed behavior updated **in the same PR**;
>   status → `done`, `updated` date current; traceability complete (no orphan/untested requirements).

## Smell test — rewrite or remove if you see…

- **Unverifiable items.** "clear", "well understood", "high quality" with no concrete check.
- **A DoD missing the two SDD clauses.** No "verified against spec" / "spec reconciled" = spec rot
  incoming.
- **A gate so long it's theatre.** If meeting it is heroic, it'll be skipped; trim to what carries
  weight.
- **Risk-calibration by deletion.** Dropping items for "small" work instead of shrinking their depth —
  even a one-line fix has both gates.
- **A manager's checklist.** If the team didn't agree it and doesn't own it, it won't be applied
  honestly.
- **No "not Ready" path.** Without one, people freeze or quietly start anyway.
- **All-manual gates.** Mechanical items (open-questions, FR-coverage, spec-touch) should be CI, not
  memory.

## Checklist

- [ ] Both gates exist, are team-owned, and were agreed (not imposed).
- [ ] Every item names how it's checked; unverifiable adjectives are gone.
- [ ] DoD includes both SDD clauses: **verified against the spec** and **spec reconciled in the same
      PR**.
- [ ] Mechanical items are automated in CI; judgment items left to humans.
- [ ] Depth scales with risk, but no item is deleted for small work.
- [ ] AI-assisted items are present if agents implement work.
- [ ] The "not Ready" response (refine / spike / split / defer) is written down.

## Links

- **Templates:** [`templates/definition-of-ready.md`](../templates/definition-of-ready.md) ·
  [`templates/definition-of-done.md`](../templates/definition-of-done.md).
- **Concept:** [`docs/06` — Definition of Ready](../docs/06-definition-of-ready.md) ·
  [`docs/07` — Definition of Done](../docs/07-definition-of-done.md); the two-gates principle
  [P7](../docs/01-principles.md#p7--two-gates-make-the-method-real-ready-and-done).
- **What they gate:** the [spec](write-a-spec.md) and its parts —
  [EARS requirements](write-ears-requirements.md), [NFRs](write-nfrs.md),
  [acceptance criteria](write-acceptance-criteria.md); conformance to the
  [constitution](write-a-constitution.md); completeness of the [tasks](write-tasks.md).
