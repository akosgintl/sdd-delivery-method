# How to write a good technical design (and ADRs)

The `design.md` is where you decide **how** to satisfy the spec — structure, interfaces, data model,
and the trade-offs behind them. It is the counterpart to the spec's *what*: if a sentence describes
observable behavior it belongs in the [spec](write-a-spec.md); if it describes the machinery that
produces that behavior it belongs here. A good design is judged not by how clever the chosen approach
is but by whether a reader can see **why this approach and not the alternatives**, and whether every
spec requirement has a stated path to being built and verified. This is the *doing* companion to
[`docs/03 — design.md`](../docs/03-artifacts.md#designmd--the-technical-design).

> The design answers **"how will we build this, and why this way?"** Two failure modes bracket it: a
> design that secretly re-specifies behavior (it belongs in the spec) and a design that's just a wiring
> diagram with no rejected alternatives (no *why* — so nobody can challenge it).

## When you write it

**Phase 2**, after the spec is `ready` and before tasks are broken out. Owned by the architect / lead
engineer. It's **living** through construction (status `draft → in-review → agreed → done`) and points
back at its spec via front-matter. Significant or hard-to-reverse decisions are split out as **ADRs** —
short, immutable records written *whenever* such a decision is made.

## Anatomy: design vs ADR

The [design template](../templates/technical-design.md) has nine sections; the load-bearing ones:

| Section | Carries | The trap |
|---------|---------|----------|
| Overview | the chosen approach in a few sentences | starting with detail, not the shape |
| Approach & alternatives | options table with the **rejected** ones and why | listing only the winner |
| Components & responsibilities | modules/services and what each owns | a diagram with no responsibilities |
| Interfaces & contracts | APIs, schemas, events; which spec req each realizes | contracts with no link to requirements |
| Data model | entities, relationships, constraints, migrations | re-deriving behavior the spec already fixed |
| Integration points & dependencies | external systems, flags, sequencing | hidden coupling discovered at build time |
| Test strategy | how each `FR`/`NFR` will be verified, by ID | "we'll write tests" with no mapping |
| Risks & mitigations | what could go wrong, how likely, the plan | optimism |
| Rollout & operability | deploy, migration, rollback, observability | shipping with no way to operate it |

An **[ADR](../templates/adr.md)** is a different shape: one decision per file — *context, decision,
alternatives, consequences, status* — and **immutable once accepted**. The design is the *current*
picture; ADRs are the *append-only history* of why it became that. To change a decision you write a
new ADR that supersedes the old one; you never edit or delete the old.

## The recipe

1. **Restate the spec's requirements as your acceptance target**, by ID. The design exists to satisfy
   `FR-1…n` and `NFR-1…n` — keep them visible so nothing is left unbuilt.
2. **Sketch the approach in a paragraph first.** If you can't state the shape in a few sentences, you
   don't understand it yet — and neither will a reviewer.
3. **Write the alternatives table with real rejected options.** A design with no rejected alternatives
   hides its reasoning. For each: pros, cons, and *why not*. The point is to make the decision
   challengeable.
4. **Assign responsibilities, not just boxes.** Each component says what it owns; a diagram without
   responsibilities is decoration.
5. **Tie interfaces and the data model back to spec IDs.** Every contract should name the requirement
   it realizes; if a contract serves no requirement, question it.
6. **Write the test strategy as a map: each `FR`/`NFR` → how it's verified.** This is the bridge to
   [DoD](write-dor-dod-gates.md) (every FR covered by a test referencing its ID) and to the
   [tasks](write-tasks.md).
7. **Plan rollout and operability** — migration, feature flag, rollback path, and the
   logs/metrics/alerts the new behavior needs. A change you can't operate or roll back isn't designed.
8. **Promote the significant decisions to ADRs.** Anything hard to reverse, contentious, or an
   *exception to the [constitution](write-a-constitution.md)* gets its own ADR (constitution exceptions
   **must** be recorded as an ADR with a stated expiry/review date). Link them from §2.

## Before → After

**Wiring diagram → reasoned design**

> ✗ *Before:* "We'll add a `CartRepository` backed by Redis and a `CartService` that calls it on every
> change."
> *(States a choice with no alternatives, no *why Redis*, no mapping to requirements, no failure or
> rollback story. Unreviewable — you can only nod or object on vibes.)*
>
> ✓ *After (excerpt):*
> - **Approach:** persist on every cart mutation to a durable per-user store; restore on session open.
> - **Alternatives:** *Redis with AOF* ✅ chosen — meets NFR-1 (p95 < 1 s) with TTL-based expiry for
>   FR-3. *Relational table* — rejected: extra write latency risks FR-1's 500 ms budget. *Client-only
>   `localStorage`* — rejected: doesn't survive device loss, fails the need.
> - **Test strategy:** FR-1 → integration test on persist latency; FR-4 → fault-injection on the store;
>   NFR-1 → load test at 100-item carts. **Rollback:** feature flag `cart_persist`; data is additive.
> - Significant choice (TTL semantics for expiry) → recorded as `ADR-0007` (see the
>   [ADR template](../templates/adr.md)).

**Editing a decision in place → superseding ADR**

> ✗ *Before:* opening `ADR-0007` and rewriting the decision when the store changes. *(ADRs are
> immutable — this erases the history of why the original choice was made.)*
>
> ✓ *After:* leave `ADR-0007` as-is, mark its status `superseded by ADR-0012`, and write `ADR-0012`
> with the new context and decision.

## Smell test — rewrite or remove if you see…

- **Behavior in the design.** If it's observable from outside, it's spec content — move it.
- **Only the chosen option.** No rejected alternatives = no visible reasoning.
- **Boxes without responsibilities**, or contracts that map to no requirement.
- **A test strategy that doesn't reference spec IDs.** It can't anchor the DoD.
- **No rollback / observability plan.** Undeployable-safely is undesigned.
- **An edited or deleted ADR.** History rewritten; write a superseding ADR instead.
- **A constitution exception with no ADR** (and no expiry).

## Checklist

- [ ] Every spec `FR`/`NFR` has a stated path to being built and verified.
- [ ] The alternatives table includes rejected options with reasons.
- [ ] Components list responsibilities, not just names.
- [ ] Interfaces and data model link back to the requirements they realize.
- [ ] Test strategy maps each requirement ID → its verification.
- [ ] Rollout, rollback, and observability are planned.
- [ ] Significant / hard-to-reverse decisions (and any constitution exceptions) are ADRs.
- [ ] ADRs are one-per-file, immutable, and superseded (never edited) when they change.

## Links

- **Templates:** [`templates/technical-design.md`](../templates/technical-design.md) ·
  [`templates/adr.md`](../templates/adr.md).
- **Worked example:** [`examples/specs/0001-cart-persistence/design.md`](../examples/specs/0001-cart-persistence/design.md).
- **Concept:** [`docs/03` — design.md](../docs/03-artifacts.md#designmd--the-technical-design) and
  [ADR](../docs/03-artifacts.md#adr--architecture-decision-record).
- **Up/down stream:** satisfies the [spec](write-a-spec.md); feeds the [task breakdown](write-tasks.md);
  its test strategy anchors the [DoD](write-dor-dod-gates.md); records exceptions to the
  [constitution](write-a-constitution.md).
