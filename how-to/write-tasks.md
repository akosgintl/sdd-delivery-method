# How to write a good task breakdown

The `tasks.md` is the **ordered, dependency-aware decomposition** of the design into units small
enough to build and verify one at a time. It's the bridge from *how we'll build it*
([design](write-a-technical-design.md)) to *who does what next*. Its value is two-fold and easy to
lose: every task must **trace to a requirement** (so nothing built is unmoored from the spec, and
nothing in the spec is left unbuilt), and the **dependency order** must be explicit (so the critical
path and the parallelizable work are visible instead of discovered the hard way). A task list that's
just a flat to-do with no IDs, no dependencies, and no requirement links is a sticky-note pile, not a
breakdown. This is the *doing* companion to
[`docs/03 — tasks.md`](../docs/03-artifacts.md#tasksmd--the-task-breakdown).

> Two questions every task must answer: **"which requirement does this advance?"** and **"what must
> be done before it?"** A task that advances no `FR`/`NFR` is either missing its trace or shouldn't
> exist.

## When you write it

**Phase 3**, after the [design](write-a-technical-design.md) is agreed, owned by the delivery team.
It's **living**: status moves `draft → ready → in-progress → done`, and individual tasks tick from
`todo` to `done` as work flows. It points back at both the spec and the design via front-matter.

## Anatomy: what each task carries

The [template](../templates/tasks.md) is a table; the columns *are* the discipline:

| Column | Holds | Why it matters |
|--------|-------|----------------|
| **ID** (`T-n`) | a stable handle | so dependencies and commits can reference it |
| **Task** | one verifiable unit of work | small enough to finish and check independently |
| **Dep** | the tasks it depends on | makes the dependency graph (and critical path) real |
| **Req** | the `FR`/`NFR` it advances | traceability — the load-bearing column |
| **Est** | rough size (S/M/L) | flags tasks too big to be one task |
| **Status** | todo / in-progress / done | the live picture of flow |
| `[P]` | parallelizable marker | shows what can run alongside the critical path |

Below the table: a **critical path** (the longest dependency chain) and a **parallelizable** note —
the two things that turn a list into a plan. What stays *out*: design decisions (those are settled in
`design.md`) and behavior (that's the spec).

## The recipe

1. **Derive tasks from the design's components and test strategy**, not from imagination. Each
   component to build, each interface to implement, each requirement to verify becomes one or more
   tasks.
2. **Make each task independently verifiable.** The test of a good task: when it's done, someone can
   *check* it's done without waiting on three other tasks. If you can't, split it.
3. **Tag every task with the requirement(s) it advances.** Then check coverage *both ways*: every
   `FR`/`NFR` has at least one task, and every task has at least one requirement. Gaps in either
   direction are bugs in the plan.
4. **Add a dedicated test/acceptance task** that references the FR IDs — the design's test strategy
   becomes a real task, usually depending on the implementation tasks.
5. **Wire dependencies, then read off the critical path.** Mark the longest chain; mark independent
   work `[P]`. This is what lets the team parallelize instead of stumbling into blockers.
6. **Right-size with estimates.** An `L` task is usually two tasks wearing a trench coat — split it
   until each is `S`/`M` and independently checkable.
7. **Note sequencing rationale, spikes, and external blockers** so the order isn't a mystery to the
   next reader.

## Before → After

**Flat to-do → traced, ordered breakdown**

> ✗ *Before:*
> ```
> - Build cart persistence
> - Make it reload
> - Write tests
> ```
> *(No IDs, no dependencies, no requirement links, and "build cart persistence" is far too big to
> verify. You can't tell what's blocking what or whether every requirement is covered.)*
>
> ✓ *After (excerpt — full version in the example bundle):*
> | ID | Task | Dep | Req | Est | Status |
> |----|------|-----|-----|-----|--------|
> | T-1 | Define cart storage schema | — | FR-1 | S | todo |
> | T-2 | Implement persist-on-change | T-1 | FR-1 | M | todo |
> | T-3 | Implement restore-on-open | T-1 | FR-2 | M | todo |
> | T-4 | Implement 24h expiry `[P]` | T-1 | FR-3 | S | todo |
> | T-5 | Retry + warning on persist failure | T-2 | FR-4 | S | todo |
> | T-6 | Acceptance tests referencing FR IDs | T-2,T-3,T-4 | FR-1..4 | M | todo |
>
> **Critical path:** T-1 → T-2 → T-5 → T-6. **Parallelizable:** T-4 alongside T-2/T-3 once T-1 is done.

## Smell test — rewrite or remove if you see…

- **A task with no `Req`.** Either it's missing its trace, or it's gold-plating — cut or link it.
- **A requirement with no task.** A coverage hole; the spec won't get fully built.
- **`L` estimates.** Almost always a task that should be split until it's independently verifiable.
- **No dependency column / no critical path.** Then it's a list, not a plan — sequencing is invisible.
- **Design or behavior smuggled in.** "Decide whether to use Redis" is a design decision, not a task;
  settle it in `design.md` (or an ADR) first.
- **No test/acceptance task.** Verification isn't free — it's a task, with FR IDs.

## Checklist

- [ ] Every task has a stable `T-n` ID and is small enough to verify independently.
- [ ] Every task names the `FR`/`NFR` it advances; coverage checked both directions.
- [ ] Dependencies are explicit and the critical path is identified.
- [ ] Parallelizable tasks are marked `[P]`.
- [ ] At least one task verifies acceptance, referencing FR IDs.
- [ ] No design decisions or behavior have leaked in from `design.md` / `spec.md`.
- [ ] Front-matter links the spec and design; status reflects reality.

## Links

- **Template:** [`templates/tasks.md`](../templates/tasks.md).
- **Worked example:** [`examples/specs/0001-cart-persistence/tasks.md`](../examples/specs/0001-cart-persistence/tasks.md).
- **Concept:** [`docs/03` — tasks.md](../docs/03-artifacts.md#tasksmd--the-task-breakdown).
- **Up/down stream:** derived from the [technical design](write-a-technical-design.md); each task
  traces to a [requirement](write-ears-requirements.md); completion is checked at
  [DoD](write-dor-dod-gates.md); traceability is recorded in the
  [matrix](../templates/traceability-matrix.md).
