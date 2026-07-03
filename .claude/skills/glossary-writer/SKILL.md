---
name: glossary-writer
description: >-
  Seed or extend a Specification-Driven Development project glossary (ubiquitous language) — one
  agreed, single-meaning definition per genuinely-contested domain term, with the banned synonyms to
  avoid — and promote accepted proposals from the glossary-maintainer. Use when the user asks to
  write, seed, start, or add terms to a glossary / ubiquitous language / domain vocabulary, or to
  accept maintainer proposals.
metadata:
  layer: sdd-skills
  role: writer
  artifact: glossary
---

# glossary-writer

Seed (or grow) the project glossary — the shared vocabulary specs, code, and tests all obey. Restraint
is the discipline: a term earns an entry only if two people could otherwise mean two different things.

## Read first
- `../_shared/glossary/template.md`, `../_shared/glossary/checklist.md`, `../_shared/glossary/example.md`.

## Procedure
1. **Harvest from real conflict**, not a dictionary — mine spec reviews, bug reports, and standups
   for words people keep clarifying. Those are your entries.
2. **Write one meaning per term.** A genuine double-meaning is two terms — split, don't hedge.
3. **Name the banned synonyms** for each term ("not 'basket', not 'order'").
4. **Keep code/tests in mind** — the agreed term should be the codebase identifier and test-name word.
5. **Promote accepted proposals:** if the `glossary-maintainer` appended entries marked
   `status: proposed`, and the user has accepted them, finalize them (remove the `proposed` marker).
6. **Prune** entries that stopped being contested.
7. **Self-apply `../_shared/glossary/checklist.md`.**

## Output contract
- Write/extend the glossary at its well-known path (default `glossary.md` at repo root or `docs/`;
  honor a user-given path). Seeding a fresh file is fine; otherwise **append/amend**, never clobber
  existing agreed entries.
- Report the path and the terms added/promoted/pruned.
