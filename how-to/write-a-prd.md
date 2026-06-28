# How to write a good PRD (vision)

A PRD (Product Requirements Document) is the **product-level "why and what"** for an initiative too
big for a single feature. It sits *upstream* of specs: one PRD typically spawns several feature
specs, and its job is to keep them pointed at the same outcome. Its power — and its main failure
mode — is **altitude**. A PRD that drifts down into behavioral detail competes with the specs and
goes stale; one that stays at goals, users, scope, and metrics stays useful for the life of the
initiative. This is the *doing* companion to
[`docs/03`](../docs/03-artifacts.md#vision--prd-product-requirements-document).

> A PRD answers **"why are we doing this initiative, for whom, and how will we know it worked?"** The
> moment it starts answering *"and here's exactly how the cart behaves"*, that content belongs in a
> spec — cut it.

## When you write it

**Phase 0**, for initiatives larger than one feature; **optional** for small changes (a one-feature
change goes straight to a [spec](write-a-spec.md)). Owned by product management. Unlike a problem
statement, a PRD is **living** — goals and the feature breakdown evolve as the initiative ships — but
it changes far less often than the specs beneath it.

## Anatomy: what goes in (and what doesn't)

| Section | Holds | Stays out |
|---------|-------|-----------|
| Problem & opportunity | who hurts, what it costs, evidence | the solution design |
| Goals & success metrics | outcomes + numbers + dates | feature-level acceptance criteria |
| Target users & needs | personas, jobs-to-be-done | UI specifics |
| Scope | in-scope capabilities + **non-goals** | EARS requirements |
| Constraints & assumptions | regulatory, technical, timeline, budget | implementation choices (→ design) |
| Feature breakdown → specs | the table of features, each → a spec | the specs themselves |
| Risks & open questions | initiative-level risks | per-feature edge cases |

The single distinguishing section is **feature breakdown → specs**: a table where each row is a
feature that becomes its own `spec.md`. That table is the seam between the PRD and the per-feature
bundles — it's how "one initiative" decomposes into independently shippable, independently testable
units.

## The recipe

1. **Open with the problem, not the product.** Reuse a [problem statement](write-a-problem-statement.md):
   who, what pain, what it costs, with evidence. If you can't state the problem, you're not ready to
   write a PRD.
2. **State goals as measurable outcomes.** Each goal gets a metric and a target-by-date —
   *"cut crash-related cart abandonment 50% by Q4"*, not *"improve the cart experience."*
3. **Name target users and their jobs-to-be-done.** Keep them concrete enough that a spec author can
   tell whether a requirement serves them.
4. **Draw the scope line — including non-goals.** What this initiative deliberately does *not* do is
   as load-bearing as what it does; it stops the spec set from sprawling.
5. **Decompose into features, and list them as a table.** Each row is one feature → one spec. Aim for
   features that are independently valuable and independently shippable; if a row can't be a spec on
   its own, it's not a feature yet.
6. **Record constraints, assumptions, and initiative-level risks** — the things that constrain *all*
   the child specs, so they don't get restated (or forgotten) in each one.
7. **Resist behavioral detail.** Every time you're tempted to write a rule, write the feature row
   instead and let the spec carry the rule. Keep the PRD at the why/what altitude.

## Before → After

**Altitude creep → product-level framing**

> ✗ *Before (a "PRD" that's really a spec):* "When the user adds an item, persist the cart within
> 500 ms; if persistence fails, retry 3 times…"
> *(Behavioral precision — this is `FR`-level content. It will drift out of sync with the real spec
> and create two sources of truth.)*
>
> ✓ *After:* "**Goal:** logged-in customers don't lose carts to crashes (metric: crash-related
> abandonment −50% by Q4). **Feature breakdown:** *Cart persistence* → `specs/0001-cart-persistence/spec.md`;
> *Cross-device sync* → `specs/0002-cart-sync/spec.md` (later)." The 500 ms rule lives in `0001`'s spec.

**Goal as adjective → goal as metric**

> ✗ *Before:* "Goal: make the cart more reliable."
> *(Unmeasurable — there's no point at which you can say it was achieved.)*
>
> ✓ *After:* "Goal: cart survives client crashes. **Metric:** crash-related abandonment. **Target:**
> reduce by 50% within one quarter of launch."

## Smell test — rewrite or remove if you see…

- **Requirements-level detail.** EARS statements, edge cases, schemas — push them down into specs.
- **Goals without metrics.** Any goal you can't measure is a slogan.
- **No non-goals.** Scope with no stated edges grows until the initiative is unbounded.
- **A feature list that isn't a list of specs.** If a row can't become its own `spec.md`, it's not
  decomposed yet.
- **Restating the constitution.** Project-wide rules live in the [constitution](write-a-constitution.md),
  not in every PRD.
- **It never changes / it changes constantly.** A frozen PRD is being ignored; a churning one is
  doing the specs' job.

## Checklist

- [ ] Opens with an evidenced problem, not a solution.
- [ ] Every goal has a metric and a target date.
- [ ] Target users and their jobs-to-be-done are named.
- [ ] Scope states both in-scope capabilities **and** non-goals.
- [ ] The feature-breakdown table maps each feature to a `spec.md` path.
- [ ] Constraints, assumptions, and initiative-level risks are recorded.
- [ ] No behavioral / requirement-level detail has leaked in from the specs.

## Links

- **Template:** [`templates/vision-prd.md`](../templates/vision-prd.md).
- **Concept:** [`docs/03` — Vision / PRD](../docs/03-artifacts.md#vision--prd-product-requirements-document);
  proportion-to-risk in [`docs/03 §7`](../docs/03-artifacts.md#7-the-minimum-viable-artifact-set).
- **Feeds:** each feature row becomes a [spec](write-a-spec.md); the PRD reuses a
  [problem statement](write-a-problem-statement.md) and inherits rules from the
  [constitution](write-a-constitution.md).
