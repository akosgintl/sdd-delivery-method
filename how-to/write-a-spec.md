# How to write a good spec.md

The `spec.md` is the **central artifact** of SDD: the precise, testable description of intended
behavior that everything downstream is built and checked against — the contract. The other how-to
guides teach you to write its *parts* ([EARS requirements](write-ears-requirements.md),
[NFRs](write-nfrs.md), [acceptance criteria](write-acceptance-criteria.md)); this one is about
**assembly** — turning those parts into a whole that is complete, bounded, and free of open
questions. A spec is more than a pile of requirements: its value is in the structure that makes the
pile *reviewable* and *verifiable*. This is the *doing* companion to
[`docs/04 §6`](../docs/04-from-needs-to-spec.md#6-assembling-the-specification).

> A spec describes **what** and **why**, never **how**. The instant a requirement names a class, a
> table, or an algorithm, you've started writing the [design](write-a-technical-design.md) — move it
> there and restate the behavior as an observable *what*.

## When you write it

**Phase 1**, after the need is understood and before construction. One spec per
[feature](../docs/12-glossary.md#feature) (`NNNN-kebab-case-slug`), owned by the spec author
(PM/BA/architect/senior engineer), reviewed by engineering and stakeholders. It's a **living**
artifact: it carries YAML front-matter with a status that advances `draft → in-review → ready →
in-progress → done`, and it gets updated whenever behavior changes — for the life of the feature.

## Anatomy: the eleven sections

The [template](../templates/specification.md) mirrors [`docs/04 §6`](../docs/04-from-needs-to-spec.md#6-assembling-the-specification):

| # | Section | Carries |
|---|---------|---------|
| — | Front-matter | `id, title, status, owner, created, updated, need, supersedes` |
| 1 | Summary & context | the problem in a paragraph; why now; link to the need |
| 2–3 | Goals & **non-goals** | what it does, and what it explicitly does *not* |
| 4 | Functional requirements | [EARS](write-ears-requirements.md) statements, `FR-n` IDs |
| 5 | Non-functional requirements | quantified [NFRs](write-nfrs.md), `NFR-n` IDs |
| 6 | Acceptance criteria / scenarios | [Gherkin / checklist](write-acceptance-criteria.md), 1:1 to requirements |
| 7 | Edge cases & error behavior | the boundary and unwanted-behavior cases |
| 8 | Data & interfaces | schemas/contracts touched (or links) |
| 9 | Dependencies & assumptions | what it relies on; stated guesses |
| 10 | Open questions | **must be empty before `ready`** |
| 11 | Rationale / decisions | the *why* (or links to ADRs) |

The two sections people skip are the two that carry the most weight: **non-goals** (scope creep
enters through omission) and **edge cases & error behavior** (the highest-value content, and the
first thing a happy-path-only spec gets wrong).

## The recipe

1. **Fill the front-matter and link the need.** A spec with no traceable need optimizes the wrong
   thing precisely. Set `status: draft` and the `need` link first.
2. **Write goals *and* non-goals together.** For every goal, ask "what's the tempting adjacent thing
   we are *not* doing?" and write it as a non-goal. Bounding scope is part of the contract.
3. **Write the functional requirements in EARS**, each singular, each with a stable `FR-n` ID. Cover
   the trio for every behavior: happy path, boundary, and unwanted condition — see
   [how to write EARS](write-ears-requirements.md).
4. **Quantify the NFRs** ([guide](write-nfrs.md)); reference, don't restate, the
   [constitution](write-a-constitution.md)'s project-wide floor.
5. **Write acceptance criteria that map 1:1 to requirements** ([guide](write-acceptance-criteria.md)).
   Every `FR`/`NFR` has at least one criterion; every criterion traces back to an ID.
6. **Pull the edge cases out of the happy path** into §7, each tagged with the requirement it
   refines. This is where you find the requirements you forgot.
7. **Capture data/interfaces, dependencies, and assumptions** — enough for a reader with no tribal
   knowledge to build from it.
8. **Drive open questions to zero.** §10 must be empty before the spec can be `ready`; each resolved
   question becomes rationale (§11) or an [ADR](write-a-technical-design.md).
9. **Run the quality checklist, then change status to `in-review`.** Hand it to the
   [DoR gate](write-dor-dod-gates.md).

## Before → After

**Solution-shaped, happy-path-only → behavioral, bounded, complete**

> ✗ *Before:* "Add a `carts` table and a cron job to save carts. The cart should reload when the user
> comes back."
> *(Names the implementation — table, cron — and specifies only the happy path. No IDs, no scope, no
> failure behavior, untestable.)*
>
> ✓ *After (excerpt):*
> - **Goal:** logged-in customers don't lose carts to browser crashes. **Non-goal:** syncing carts
>   across *different* devices.
> - **FR-1 (event-driven):** When an item is added to or removed from the cart, the system shall
>   persist the updated cart to durable per-user storage within 500 ms.
> - **FR-4 (unwanted):** If persisting the cart fails, then the system shall retry up to 3 times and
>   surface a non-blocking warning if all retries fail.
> - **NFR-1:** Restoration shall complete within 1 s at p95 for carts up to 100 items.
> - **Acceptance:** Gherkin scenarios verifying FR-2 and FR-3; §10 Open questions: *(empty)*.
>
> See the fully worked version in [`examples/specs/0001-cart-persistence/spec.md`](../examples/specs/0001-cart-persistence/spec.md).

## Smell test — rewrite or remove if you see…

- **"How" in the spec.** Class names, tables, frameworks, algorithms → move to the design.
- **No non-goals.** Unbounded scope; the spec can't say what's *not* in it.
- **Only the happy path.** No boundary or error behavior → §7 is empty or thin.
- **Acceptance criteria that don't map to IDs**, or requirements with no verifying criterion.
- **Anything in §10.** A non-empty Open Questions section means it isn't `ready` — by definition.
- **Stale front-matter.** `status`/`updated` not reflecting reality is the first sign of spec rot.
- **Vague words** ([banned list](../docs/04-from-needs-to-spec.md#33-words-to-ban-or-define)) left
  un-quantified.

## Checklist

(The spec-level checklist from [`docs/04 §6`](../docs/04-from-needs-to-spec.md#specification-quality-checklist-apply-before-review).)

- [ ] Every requirement has a stable ID and is singular, unambiguous, and testable.
- [ ] Goals **and** non-goals are both stated.
- [ ] Happy path, boundaries, and error behavior are all covered.
- [ ] NFRs are quantified; banned vague words are gone or defined.
- [ ] Acceptance criteria map 1:1 to requirements.
- [ ] Open questions are resolved (or the spec is explicitly `draft`).
- [ ] It traces up to a need and down to tests.
- [ ] Front-matter is complete and the status/updated date is current.

## Links

- **Template:** [`templates/specification.md`](../templates/specification.md).
- **Worked example:** [`examples/specs/0001-cart-persistence/spec.md`](../examples/specs/0001-cart-persistence/spec.md).
- **Concept:** [`docs/04 §6` — Assembling the specification](../docs/04-from-needs-to-spec.md#6-assembling-the-specification);
  full chapter [`docs/04`](../docs/04-from-needs-to-spec.md).
- **Its parts:** [EARS requirements](write-ears-requirements.md) ·
  [NFRs](write-nfrs.md) · [acceptance criteria](write-acceptance-criteria.md). **Its gate:**
  [DoR / DoD](write-dor-dod-gates.md). **Its downstream:**
  [technical design](write-a-technical-design.md), [tasks](write-tasks.md).
