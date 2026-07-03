---
name: traceability-writer
description: >-
  Generate a Specification-Driven Development traceability matrix (traceability.md) by scanning a
  feature's spec, tasks, and design for stable IDs — building the forward trace (need → requirement
  → task → test → code → verified), the backward trace, and the Gaps section. Re-running it is how
  the matrix is regenerated. Use when the user asks to generate, build, create, or regenerate a
  traceability matrix / traceability.md / requirement coverage matrix.
metadata:
  layer: sdd-skills
  role: writer-generator
  artifact: traceability
---

# traceability-writer (generator)

Generate the traceability matrix for a feature from the IDs already present in its artifacts. This
is a **generator**: it does not invent coverage — it reports what the spec/tasks/design/tests
actually reference. Re-running it is the "regenerate" step of the workflow.

## Read first
- `../_shared/traceability/template.md`, `../_shared/traceability/checklist.md`,
  `../_shared/traceability/example.md`.
- `../_shared/conventions.md` (IDs + test-naming convention).

## Procedure
1. **Collect inputs** from the feature folder: `spec.md` (every `FR`/`NFR` id + short text + the
   `need` link), `tasks.md` (the `Req` column → tasks per requirement), `design.md` (test strategy),
   and any tests discoverable by the `__FR<n>` / `@FR-<n>` / `@requirement(...)` convention.
2. **Build the forward trace** — one row per spec `FR`/`NFR`: Source need, Task(s), Test(s), Code,
   and a `☐` Verified box. Where a test or code artifact does not yet exist, emit it as *planned*
   (e.g. the expected test name) rather than leaving the requirement out.
3. **Build the backward trace** — each known code/module → the requirements it realizes → the need.
4. **Compute Gaps** — requirements with no test; code with no requirement. State them plainly (do
   not hide a gap by fabricating a test id).
5. **Fill front-matter** (`spec:` link, `updated`).

## Output contract
- Write to `specs/NNNN-slug/traceability.md` (default Layout A; honor a user-given path).
- Re-generation **overwrites** the generated matrix (this artifact is generated, not hand-authored)
  — but preserve any manually-flipped `☑` Verified marks if present.
- Report the path and the Gaps found (these are the signal the `traceability-reviewer` audits).
