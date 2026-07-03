---
name: design-writer
description: >-
  Draft a Specification-Driven Development technical design (design.md) that satisfies an approved
  spec — approach with rejected alternatives, components & responsibilities, interfaces/data model
  tied to requirement IDs, a test strategy mapping every FR/NFR, and rollout/rollback/observability.
  Use when the user asks to write, draft, or create a technical design / design.md / architecture for
  a feature whose spec exists.
metadata:
  layer: sdd-skills
  role: writer
  artifact: design
---

# design-writer

Draft a feature `design.md` — **how** we satisfy the spec. Observable behavior stays in the spec;
here you decide structure, interfaces, data model, and the trade-offs behind them.

## Read first
- `../_shared/design/template.md` — the nine-section skeleton.
- `../_shared/design/checklist.md` — the quality bar (self-apply before finishing).
- `../_shared/design/example.md` — a worked gold design to imitate.
- `../_shared/conventions.md` (IDs, ADR/status vocab).

## Procedure
1. **Read the spec** it must satisfy; restate its `FR`/`NFR` IDs as your acceptance target so
   nothing is left unbuilt. Refuse to proceed if the spec is missing or not at least `ready`.
2. **Sketch the approach in a paragraph** (Overview) before any detail.
3. **Write the alternatives table with real rejected options** — pros, cons, and *why not* each.
4. **Assign responsibilities, not just boxes** (Components).
5. **Tie interfaces and the data model back to spec IDs** — every contract names the requirement
   it realizes.
6. **Write the test strategy as a map:** each `FR`/`NFR` → how it is verified (this anchors the DoD).
7. **Plan rollout & operability** — migration, feature flag, rollback path, logs/metrics/alerts.
8. **Promote significant / hard-to-reverse decisions to ADRs** (use `adr-writer`); link them from §2.
   A constitution exception MUST become an ADR with an expiry/review date.
9. **Self-apply `../_shared/design/checklist.md`.**

## Output contract
- Write to `specs/NNNN-slug/design.md` (default Layout A; honor a user-given path).
- **Never overwrite an existing `design.md`** — defer to `design-rewriter`.
- Report the path, the FR/NFR→verification map, and any decisions you flagged for an ADR.
