# How to write a good constitution

The constitution is your project's **standing law**: the short list of non-negotiable rules every
spec and every implementation must honor, so that per-feature specs don't have to restate them. Its
power comes from being *short, stable, and enforceable* — a constitution that drifts into a style
guide gets ignored, and an ignored constitution governs nothing. This is the *doing* companion to
[`docs/01 §2`](../docs/01-principles.md#2-the-constitution).

> The constitution answers one question: **"what is *always* true of how we build here?"** If a
> rule doesn't apply to every feature, or you can't check whether it was followed, it doesn't
> belong.

## When you write it

**Once per project**, early — before or during the first few specs — and amended rarely and
deliberately thereafter. Owned by engineering leadership / architecture. It is the only artifact
that outranks an individual spec, so it changes through a reviewed amendment, never casually.

## Anatomy: what goes in (and what doesn't)

Six buckets, each a short list of checkable clauses with stable IDs:

| Section | Holds | Example clause |
|---------|-------|----------------|
| Engineering principles | how you build, universally | "Every feature ships with automated tests that verify its spec." |
| Architectural constraints | structural non-negotiables | "The presentation layer never accesses the database directly." |
| Quality bars | the numeric floor | "p95 API latency < 300 ms at 1000 RPS." |
| Technology constraints | approved/banned tech, data rules | "PII stays in region X; retention ≤ 90 days." |
| Process rules | how work moves | "Each behavior change updates its `spec.md` in the same PR." |
| Amendments & exceptions | how the rules change | "Exceptions are recorded as an ADR with a stated expiry." |

A good constitution is **stable** (changes rarely), **short** (if it's long, it's a style guide —
split that out), **enforceable** (each clause checkable, ideally in CI), and **universal within
scope** (applies to *every* feature). See
[properties of a good constitution](../docs/01-principles.md#properties-of-a-good-constitution).

## The recipe

1. **Brain-dump candidate rules** across the six buckets — everything the team currently argues
   about or assumes.
2. **Filter hard.** Keep a rule only if it is (a) universal — true for every feature, (b)
   non-negotiable — not a default you'd waive on a whim, and (c) enforceable — you can point at how
   it's checked. Cut everything else; most of it is style-guide or per-feature detail.
3. **Quantify the quality bars.** Replace adjectives with numbers and standards: coverage %, a p95
   latency budget, an accessibility level (e.g. WCAG 2.2 AA), a named security baseline.
4. **Make each clause checkable.** For every clause, note *how* it's enforced — a CI gate, a review
   step, a linter. A clause no one can check is decoration.
5. **Define the amendment and exception process.** State who can amend, and that any exception is
   an [ADR](../docs/03-artifacts.md#adr--architecture-decision-record) recording the clause, the
   reason, the scope, and an expiry/review date.
6. **Cut until it's short.** Aim for something a new engineer reads in one sitting. If it's growing
   past a page or two, you're writing the wrong document.

## Before → After

**Aspirational mush → enforceable quality bars**

> ✗ *Before:* "Code should be high-quality, secure, and performant."
> *("High-quality", "secure", "performant" are unenforceable — no one can fail a review against
> them objectively.)*
>
> ✓ *After:*
> - **Q-1:** Test coverage ≥ 80% on changed lines; every `FR-*`/`NFR-*` has a verifying test.
> - **Q-2:** Security: dependency, secrets, and SAST scans pass with no high/critical findings.
> - **Q-3:** Performance budget: p95 API latency < 300 ms at 1000 RPS.

**Style-guide clutter → out of the constitution**

> ✗ *Before:* "Use 2-space indentation, name React components in PascalCase, and prefer `const`."
> *(True, but not universal law — it's a style guide, and it'll bloat and date the constitution.)*
>
> ✓ *After:* one process rule — "**R-4:** Code conforms to the repo's linter/formatter config,
> enforced in CI." — and the details live in the linter config, not here.

## Smell test — rewrite or remove if you see…

- **Unenforceable adjectives.** "robust", "clean", "scalable" with no number or check behind them.
- **Style-guide material.** Formatting, naming, file layout — push it to a linter/style doc.
- **Per-feature specifics.** Anything that isn't true for *every* feature belongs in a spec.
- **No "how it's checked."** A clause with no enforcement mechanism is a hope.
- **No amendment/exception path.** Without one, people either freeze or quietly ignore it.
- **Length.** More than a page or two usually means rules 3, 4, and 5 weren't applied.

## Checklist

- [ ] Every clause is universal (applies to all features), non-negotiable, and enforceable.
- [ ] Quality bars are quantified with numbers and named standards.
- [ ] Each clause notes how it's enforced (CI / review / tooling).
- [ ] An amendment process and an ADR-based exception process are defined.
- [ ] No style-guide or per-feature detail has crept in.
- [ ] It's short enough to read in one sitting and has stable IDs (`P-`, `A-`, `Q-`, `T-`, `R-`).

## Links

- **Template:** [`templates/constitution.md`](../templates/constitution.md).
- **Concept:** [`docs/01 §2` — The Constitution](../docs/01-principles.md#2-the-constitution) and
  [what goes in one](../docs/01-principles.md#what-goes-in-a-constitution).
- **Related:** record exceptions as ADRs —
  [`docs/03` ADR](../docs/03-artifacts.md#adr--architecture-decision-record); the gates that enforce
  it live in [`docs/06`](../docs/06-definition-of-ready.md) /
  [`docs/07`](../docs/07-definition-of-done.md).
