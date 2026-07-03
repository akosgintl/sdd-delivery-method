# How to write a good traceability matrix

The `traceability.md` is the connective tissue of a feature: the chain that links **need →
requirement → acceptance → test → code**, made explicit enough to answer two questions a repo can't
answer on its own — *"is every requirement actually built and tested?"* (forward/coverage) and *"why
does this code exist, and what breaks if I change this requirement?"* (backward/impact). Its defining
property, and the thing every other guide here doesn't share: for most teams the matrix is
**generated**, not hand-authored — harvested from the stable IDs already referenced across
`spec.md`, `tasks.md`, and the tests. A hand-maintained matrix that drifts from the code is worse
than none, because it lies with authority. This is the *doing* companion to
[`docs/09`](../docs/09-quality-and-traceability.md).

> The matrix is a **by-product of disciplined IDs**, not a document someone remembers to update.
> Two failure modes bracket it: a matrix maintained by hand that has quietly gone stale, and a
> "coverage" column that hides a gap behind a test name nobody wrote.

## When you write it

**Phase 5** (verify), and continuously as a generated view thereafter. For most teams it is produced
by a small script (or the [`traceability-writer`](../.claude/skills/traceability-writer/SKILL.md)
skill) that scans the feature's artifacts; you only *write* it by hand where audit/compliance demands
explicit, signed-off evidence ([P8 — specify in proportion to risk](../docs/01-principles.md)). It
points back at its spec via front-matter (`spec: ./spec.md`) and lives beside the rest of the feature
bundle.

## Anatomy: the three parts

The [template](../templates/traceability-matrix.md) has three sections; the discipline is in the IDs
that stitch them together:

| Part | Carries | The trap |
|------|---------|----------|
| **Forward trace** | one row per `FR`/`NFR`: need → task(s) → test(s) → code → `Verified` | a requirement silently missing from the table |
| **Backward trace** | each code/module → the requirements it realizes → the need | code that realizes nothing (dead or unspecified) |
| **Gaps** | requirements with no test; code with no requirement | leaving it blank instead of stating "none ✅" |

The load-bearing mechanism is the **test-naming convention** — `test_<desc>__FR<n>`, or
`@requirement("0001/FR-3")`, or a Gherkin `@FR-3` tag — which is what lets coverage be *matched by a
machine* instead of asserted by a human.

## The recipe

1. **Start from the spec's IDs.** List every `FR`/`NFR` the spec defines; each becomes exactly one
   forward-trace row. If a requirement can't get a row, the spec — not the matrix — is where you fix it.
2. **Pull the `need` from the spec's front-matter** into the Source-need column, so every row traces up.
3. **Fill Task(s) from `tasks.md`'s `Req` column** — the tasks that advance each requirement.
4. **Fill Test(s) using the naming convention.** Where a test doesn't exist yet, record the *planned*
   name (e.g. `test_expiry_boundary__FR3`) so the gap is visible, not hidden.
5. **Write the backward trace** from code/modules back to the requirements they realize — this is
   what makes impact analysis and "why does this exist?" answerable.
6. **Compute the Gaps section honestly.** Requirements with no test, and code with no requirement.
   State "none ✅" when clean; never paper over a gap with an invented test id.
7. **Leave `Verified` as `☐`** until the referencing test passes in CI; all boxes must be `☑` for the
   spec to reach `done` ([DoD](write-dor-dod-gates.md)).
8. **Prefer generation.** If you're editing rows by hand, ask whether a script or the
   `traceability-writer` skill should own this instead — regenerate rather than reconcile.

## Before → After

**Hand-kept matrix that drifted → generated, honest coverage view**

> ✗ *Before:*
> ```
> | Req  | Test        | Verified |
> | FR-1 | (tested)    | ✅       |
> | FR-2 | ...         | ✅       |
> ```
> *(No need, no tasks, no code column, "(tested)" names no runnable test, and FR-3/FR-4/FR-5 are
> simply absent — you can't tell whether they're covered or forgotten. The ✅s are assertions,
> not evidence.)*
>
> ✓ *After (excerpt — full version in the example bundle):*
> | Req | Requirement (short) | Source need | Task(s) | Test(s) | Code | Verified |
> |-----|---------------------|-------------|---------|---------|------|:--------:|
> | FR-2 | restore on reopen | PRD §2.1 | T-4 | `test_restore_after_crash__FR2` | `CartService.restore` | ☐ |
> | FR-5 | per-user isolation | PRD §4 | T-1,T-2 | `test_cross_user_isolation__FR5` | userId-keyed store | ☐ |
>
> **Gaps:** Requirements with no test: **none** ✅. Code with no requirement: **none** ✅.
>
> See the fully worked version in [`examples/specs/0001-cart-persistence/traceability.md`](../examples/specs/0001-cart-persistence/traceability.md).

## Smell test — rewrite or regenerate if you see…

- **A spec requirement with no row.** The matrix isn't complete — regenerate from the spec's IDs.
- **A "coverage" cell that names no runnable test** ("tested", "done", a description) — it can't be
  machine-verified.
- **A gap patched inside the matrix** instead of fixed upstream. A missing test is a task in
  `tasks.md`, not a row you fake here.
- **Code in the backward trace that realizes no requirement** — dead code, or a missing requirement.
- **`Verified` boxes ticked while tests are red** (or the spec isn't `done`).
- **A hand-maintained matrix drifting from the IDs** — move to generation.
- **ID drift:** an `FR`/`NFR`/`T` in the matrix that the spec/tasks never define.

## Checklist

- [ ] Every spec `FR`/`NFR` is exactly one forward-trace row, tracing up to a need and down to task(s) and test(s).
- [ ] Every test reference follows the `__FR<n>` (or tag/annotation) convention.
- [ ] Backward trace is present; no code realizes zero requirements.
- [ ] Gaps section lists requirements-with-no-test (empty for `done`) and code-with-no-requirement.
- [ ] All IDs match the spec/tasks exactly — no drift.
- [ ] `Verified` state matches CI reality and the spec's status.
- [ ] Front-matter links the spec; `updated` is current.
- [ ] If maintained by hand, there's a compliance reason; otherwise it's generated.

## Links

- **Template:** [`templates/traceability-matrix.md`](../templates/traceability-matrix.md).
- **Worked example:** [`examples/specs/0001-cart-persistence/traceability.md`](../examples/specs/0001-cart-persistence/traceability.md).
- **Concept:** [`docs/09` — Quality & Traceability](../docs/09-quality-and-traceability.md).
- **Up/down stream:** harvests IDs from the [spec](write-a-spec.md) and [tasks](write-tasks.md);
  its coverage check anchors the [DoD](write-dor-dod-gates.md).
