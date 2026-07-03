# SDD conventions (bundled reference)

The stable vocabulary every SDD skill enforces. Self-contained — do not rely on the study's
`docs/`, `templates/`, or `examples/` folders; this file carries what the skills need.

## Stable IDs — assigned once, never renumbered

| Thing | ID form | Scope | Example |
|-------|---------|-------|---------|
| Functional requirement | `FR-<n>` | per feature | `FR-3` |
| Non-functional requirement | `NFR-<n>` | per feature | `NFR-1` |
| Task | `T-<n>` | per feature | `T-7` |
| Feature folder | `NNNN-kebab-case-slug` | global, zero-padded | `0001-cart-persistence` |
| ADR | `NNNN-short-title.md` | global, append-only | `0007-cart-expiry-window.md` |
| Global requirement reference | `NNNN/FR-<n>` | cross-feature | `0001/FR-3` |

Rules:
- IDs are **stable**: once published, an ID is never reused for a different requirement and never
  renumbered. Deleting a requirement retires its ID; it is not backfilled.
- IDs are **singular**: one requirement/task per ID. If a statement joins two behaviors with
  "and", it is two requirements.
- The **same** ID string that the spec defines is the one tasks, tests, and traceability reference.
  Any drift (a task citing `FR-9` that the spec never defined) is a defect.

## Status vocabularies (front-matter `status:`)

- **spec:** `draft → in-review → ready → in-progress → done → superseded`
- **design:** `draft → in-review → agreed → done`
- **tasks:** `draft → ready → in-progress → done` (individual task rows use `todo → doing → done`)
- **ADR:** `proposed → accepted → superseded` (a superseded ADR's *status line only* is edited)

A `ready` or `done` spec MUST have an empty Open Questions section and no `TBD`.

## Spec front-matter (required fields)

`id, title, status, owner, created, updated, need, supersedes`

- `need:` links up to the originating PRD / story / problem statement.
- `supersedes:` is `null` or the id of the spec this replaces.
- Design/tasks/traceability carry `artifact:` plus link fields (`spec:`, `design:`) to their siblings.

## Test-naming convention (how a test declares the requirement it verifies)

A test references its requirement by one of:
- suffix: `test_cart_restored_after_crash__FR2` (double underscore + `FR<n>`)
- annotation: `@requirement("0001/FR-3")`
- Gherkin tag: `@FR-3`

Traceability and coverage checks rely on this convention to match tests back to `FR`/`NFR` IDs.

## Cross-links over duplication (in the study) vs bundling (in skills)

The study links a concept from one chapter and never restates it. **The skills tree is the
deliberate exception:** `_shared/` bundles copies so skills stay portable. When a rule changes in
the study's `docs/`, update the corresponding `_shared/` copy too (sync obligation).
