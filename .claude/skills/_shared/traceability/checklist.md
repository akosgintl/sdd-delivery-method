# traceability.md — quality checklist & audit rubric (bundled reference)

The rubric the `traceability-writer` (generator) targets and the `traceability-reviewer` (audit)
checks against. The matrix links **need → requirement → acceptance → test → code**. For most teams
it is **generated** from the stable IDs referenced across spec/tasks/tests — so the writer assembles
it and re-running the writer is how it is "regenerated". Uses `../_shared/conventions.md`.

## What the matrix carries

- **Forward trace** (coverage): one row per requirement — `Req ID | Requirement (short) | Source
  need | Task(s) | Test(s) | Code | Verified`. Answers "is every requirement built & tested?"
- **Backward trace** (justification): `Code / module | Realizes | Originating need`. Answers "why
  does this code exist / what breaks if the requirement changes?"
- **Gaps:** requirements with no test (must be empty for `done`); code with no requirement
  (deletion candidates or a missing requirement).
- Front-matter: `id, artifact: traceability, updated, spec:`.

## Test-naming convention (how a test declares its requirement)

`test_<desc>__FR<n>` (double-underscore suffix), or `@requirement("NNNN/FR-<n>")`, or Gherkin
`@FR-<n>`. Coverage matching relies on this.

## Audit checks (reviewer) — findings escalate UPSTREAM, never patch the matrix

- **Forward completeness:** every `FR`/`NFR` defined in the spec appears as exactly one forward row.
  A spec requirement missing from the matrix ⇒ regenerate. `[BLOCKER]`
- **Coverage gap:** a requirement with no test in its row. The fix is **upstream** — add a test/task
  in spec/tasks; do not fake a test id here. `[BLOCKER]` for a `done` / in-progress target. **At
  `ready`** (spec ready-to-build, no code yet) a fully-planned-but-untested row is the *expected*
  state, not a blocker — record it as a `MINOR`/note. The gap becomes a BLOCKER once the feature is
  being verified toward `done`.
- **Backward completeness:** every code/module entry realizes ≥1 requirement; code with no
  requirement is flagged (deletion candidate or a missing requirement). `[MAJOR]`
- **ID drift:** any `FR`/`NFR`/`T` id in the matrix that the spec/tasks do not define. `[BLOCKER]`
- **Test-name convention** not followed, so the id can't be machine-matched. `[MAJOR]`
- **Verified boxes** inconsistent with status (e.g. `☐` boxes while the spec claims `done`). `[MAJOR]`
- **Front-matter** missing/`spec:` unresolved/`updated` stale. `[MINOR]`

## Checklist

- [ ] Every spec `FR`/`NFR` is a forward row with a Source need, Task(s), and Test(s).
- [ ] Every test name follows the `__FR<n>` (or tag/annotation) convention.
- [ ] Backward trace present; no code without a requirement.
- [ ] Gaps section lists requirements-with-no-test (empty for `done`) and code-with-no-requirement.
- [ ] All IDs match the spec/tasks exactly (no drift).
- [ ] `Verified` state is consistent with the spec's status.
- [ ] Front-matter complete; `spec:` resolves; `updated` current.
