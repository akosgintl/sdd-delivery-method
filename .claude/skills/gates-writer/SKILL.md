---
name: gates-writer
description: >-
  Scaffold a Specification-Driven Development project's Definition of Ready (DoR) and Definition of
  Done (DoD) gate files — team-owned, checkable criteria, with the two SDD-defining DoD clauses
  (verified against the spec; spec reconciled in the same PR). Use when the user asks to write, set
  up, scaffold, or customize DoR / DoD / the ready & done gates / quality gates.
metadata:
  layer: sdd-skills
  role: writer
  artifact: gates
---

# gates-writer

Scaffold the project's two gates — DoR (bar to clear *before* build) and DoD (bar to clear to be
called *done*). Team-owned, every item **checkable**.

## Read first
- `../_shared/gates/dor-template.md`, `../_shared/gates/dod-template.md`, `../_shared/gates/gate-criteria.md`.

## Procedure
1. **Bootstrap from the templates**, then cut to the team — lean enough that meeting DoR is normal,
   not heroic.
2. **Make every item checkable** — name *how* it's verified; rewrite adjectives ("clear") into
   mechanical checks (front-matter present, §10 empty, every `FR` has a test referencing its ID).
3. **Keep the two SDD-defining DoD clauses non-negotiable:** *verified against the spec* and *spec
   reconciled in the same PR*.
4. **Note which items are automatable in CI** vs which need human judgment (acceptance; "does the
   spec describe reality?").
5. **Add the AI-assisted items** if agents implement work.
6. **Write the "not Ready" response** (refine / spike / split / defer).

## Output contract
- Write `definition-of-ready.md` and `definition-of-done.md` at the project's well-known path
  (default repo root or `docs/`/`.specify/`; honor a user-given path).
- **Idempotent:** if a gate file already exists, do **not** clobber it — report it and offer to amend.
- Report the paths and which items are CI-automatable.
