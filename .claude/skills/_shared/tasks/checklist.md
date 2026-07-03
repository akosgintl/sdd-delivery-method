# tasks.md — quality checklist & smell test (bundled reference)

The rubric a `tasks-writer` self-applies and a `tasks-reviewer` checks against. `tasks.md` is the
ordered, dependency-aware decomposition of the design into units small enough to build and verify
one at a time. Uses `../_shared/conventions.md`.

> Two questions every task must answer: **"which requirement does this advance?"** and **"what must
> be done before it?"** A task that advances no `FR`/`NFR` is either missing its trace or shouldn't exist.

## Anatomy — the columns are the discipline

| Column | Holds | Why it matters |
|--------|-------|----------------|
| **ID** (`T-n`) | a stable handle | dependencies and commits reference it |
| **Task** | one verifiable unit of work | small enough to finish and check independently |
| **Dep** | the tasks it depends on | makes the dependency graph / critical path real |
| **Req** | the `FR`/`NFR` it advances | traceability — the load-bearing column |
| **Est** | rough size (S/M/L) | flags tasks too big to be one task |
| **Status** | todo / in-progress / done | the live flow picture |
| `[P]` | parallelizable marker | shows what can run alongside the critical path |

Below the table: a **critical path** (longest dependency chain) and a **parallelizable** note.
Front-matter links **both** `spec:` and `design:`. What stays out: design decisions and behavior.

## Smell test — flag if you see…

- **A task with no `Req`** — missing trace or gold-plating. `[MAJOR]`
- **A requirement with no task** — coverage hole; the spec won't be fully built. `[BLOCKER]`
- **`L` estimates** — usually a task that should be split until independently verifiable. `[MINOR]`
- **No dependency column / no critical path** — a list, not a plan. `[MAJOR]`
- **Design or behavior smuggled in** ("decide whether to use Redis") — settle it in design/ADR first. `[MAJOR]`
- **No test/acceptance task** referencing FR IDs. `[MAJOR]`
- **`T-n` ID drift** or a `Req` citing an `FR`/`NFR` the spec never defines. `[BLOCKER]`
- **Front-matter missing `spec:` or `design:` link.** `[MAJOR]`

## Checklist

- [ ] Every task has a stable `T-n` ID and is small enough to verify independently.
- [ ] Every task names the `FR`/`NFR` it advances; **coverage checked both directions** (every
      requirement ↔ ≥1 task; every task ↔ ≥1 requirement).
- [ ] Dependencies are explicit and the critical path is identified.
- [ ] Parallelizable tasks are marked `[P]`.
- [ ] At least one task verifies acceptance, referencing FR IDs.
- [ ] No design decisions or behavior have leaked in from `design.md` / `spec.md`.
- [ ] Front-matter links the spec and design; `status` reflects reality.
