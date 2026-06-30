# 11 — Adoption & Anti-Patterns

A method is only as good as its adoption. This chapter covers how to roll SDD out without
drowning the team in ceremony, the failure modes that kill it, and how to tell whether it's
working.

## 1. How to adopt SDD incrementally

Do **not** start by writing a constitution and twelve templates and mandating them. Start where
the pain is, prove value, expand.

**Stage 1 — One spec, one feature.** Pick a single upcoming feature with real ambiguity. Write a
`spec.md` with testable requirements and acceptance criteria. Review it before building. Ship it.
Notice how many "we'll figure it out later" moments disappeared.

**Stage 2 — The two gates.** Adopt a lightweight DoR and DoD (start from
[`templates/`](../templates/)). Apply them to new work. This is where most of the value lives —
the gates, not the documents.

**Stage 3 — Storage conventions.** Standardize where specs live (numbered `specs/` folders),
front-matter, and stable requirement IDs. Add a `specs/README.md` index.

**Stage 4 — Traceability & CI.** Add the requirement-coverage check (every `FR-*` has a test).
Add spec lint. This makes quality self-enforcing.

**Stage 5 — Constitution.** Once you've seen which rules you keep repeating in reviews, codify
them as the constitution. (It's better discovered than decreed.)

**Stage 6 — AI agents (if applicable).** With specs and gates in place, you have exactly what
agents need. Add tooling ([10](10-ai-assisted-sdd.md)) on top of the now-solid foundation.

> Sequencing principle: **value before ceremony.** Each stage should pay for itself before the
> next is added. If a stage isn't earning its keep, stop and fix that before expanding.

> Rolling out across **many squads with AI agents central**? The stages above still apply, but you
> pull the tooling stage forward and expand squad-by-squad behind a shared paved road — see
> [C — Scaling to the Enterprise](appendices/C-scaling-to-the-enterprise.md).

## 2. Anti-patterns (the failure modes)

### A. Big-spec-up-front (waterfall in disguise)
Writing one enormous specification for the whole system before any construction. Symptoms:
months of specifying, a doc no one reads, reality diverging immediately.
**Fix:** spec per feature ([P6](01-principles.md#p6--the-smallest-useful-unit-is-the-feature-not-the-project));
specify just-ahead-of-build, not all-up-front.

### B. Spec rot (the silent killer)
Specs that drift out of date until they actively mislead. A stale spec is worse than none — it
lies with authority.
**Fix:** the DoD reconcile clause ([07 §2.B](07-definition-of-done.md)); CI spec-touch check;
specs in-repo so they change *with* the code.

### C. Specifying *how* instead of *what*
The spec prescribes implementation, ossifying design and bloating the document.
**Fix:** the altitude table ([00 §7](00-overview.md#7-calibrating-specification-depth-the-right-altitude));
push *how* into `design.md`/code; spec review challenges any implementation detail.

### D. Untestable requirements
"The system should be fast/intuitive/robust." Nothing can be verified; DoD becomes a debate.
**Fix:** [P3](01-principles.md#p3--every-requirement-must-be-testable); the testability test;
EARS + quantified NFRs ([04](04-from-needs-to-spec.md)).

### E. Artifacts without gates (theater)
The team produces specs but builds before they're sound and ships without verifying against them.
The documents become decoration.
**Fix:** enforce DoR and DoD ([P7](01-principles.md#p7--two-gates-make-the-method-real-ready-and-done)).
The gates *are* the method; the documents are just their inputs.

### F. Uniform ceremony for all work
Forcing a full bundle on a one-line copy fix. The team revolts, reasonably.
**Fix:** [P8](01-principles.md#p8--specify-in-proportion-to-risk) — calibrate to risk; the
minimum viable artifact set ([03 §7](03-artifacts.md#7-the-minimum-viable-artifact-set)).

### G. Specs in a separate tool (drift by design)
Keeping the engineering spec in a wiki/Confluence detached from the code.
**Fix:** specs in the repo ([05 §1](05-storage-and-organization.md#1-first-principle-specs-live-with-the-code)).
Product artifacts may live upstream; the *contract* lives with the code.

### H. The spec as the story (or vice versa)
Treating a user story as if it were a specification, or burying a spec where stakeholders can't
see the value.
**Fix:** stories express value and point to specs; specs express precise behavior
([04 §2](04-from-needs-to-spec.md#2-capturing-intent-user-stories-and-acceptance-criteria)).

### I. Trusting fluent AI output (in agent contexts)
Accepting plausible-looking generated code/tests without verifying against the spec.
**Fix:** mandatory human verification against acceptance criteria; spec→test-first
([10 §4](10-ai-assisted-sdd.md#4-practices-that-keep-agent-built-systems-faithful)).

## 3. Cultural prerequisites

SDD asks for a few habits that may be new:
- **Writing before building** feels slow to people rewarded for visible code. Leadership must
  protect upstream time and celebrate prevented rework, which is invisible by nature.
- **Reviewing prose** as rigorously as code. Spec review is a learned skill.
- **Updating the spec is part of the work**, not paperwork after it.
- **Comfort with non-Ready outcomes.** Sending work back at the DoR gate is success, not failure.

## 4. Is it working? Signals

**Healthy SDD:**
- Fewer "what did we actually mean here?" moments during build.
- Falling clarification/rework rates ([09 §6](09-quality-and-traceability.md#6-metrics-for-the-method-itself)).
- New joiners (and agents) can build from the spec without a guided tour.
- Incident post-mortems can cite the spec that was (or wasn't) right.

**Unhealthy SDD:**
- Specs written then ignored (gate theater — anti-pattern E).
- Specs consistently stale (rot — B).
- Ballooning specifying time with no fall in rework (over-ceremony — F, or how-not-what — C).
- The team experiences SDD as paperwork rather than as fewer surprises.

If you see the unhealthy signals, the fix is almost always *fewer, better-enforced essentials* —
not more documents. The irreducible core remains: **a testable spec per feature, gated by Ready
and Done, living in the repo.**

## 5. A 90-day adoption sketch

| Weeks | Focus | Outcome |
|-------|-------|---------|
| 1–2 | Stage 1: spec one real feature; run a spec review | Proof the upstream catches ambiguity |
| 3–4 | Stage 2: adopt lean DoR/DoD on new work | Gates in place |
| 5–6 | Stage 3: storage conventions, IDs, index | Specs discoverable & consistent |
| 7–9 | Stage 4: requirement-coverage CI check + spec lint | Quality self-enforcing |
| 10–11 | Stage 5: distill the constitution from review patterns | Standing rules codified |
| 12–13 | Retro + Stage 6 if relevant: introduce agent tooling | Method stable; ready to scale |

Adjust to context, but keep the ordering: **value, then gates, then conventions, then automation,
then standing law, then tooling.**

---

This concludes the study chapters. See [`templates/`](../templates/) for copy-ready artifacts and
[`examples/`](../examples/) for a fully worked feature. Return to the [README](../README.md) for
the table of contents.

> Reference appendices: [`A — Glossary`](appendices/A-glossary.md) defines the method's core vocabulary,
> [`B — Property Graph`](appendices/B-property-graph.md) re-expresses it as a graph model, and
> [`C`–`G`](../README.md#how-to-read-this-repository) cover scaling, EARS, tooling, maturity, and agent prompts.
