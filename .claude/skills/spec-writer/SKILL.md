---
name: spec-writer
description: >-
  Draft a Specification-Driven Development feature spec (spec.md) from an upstream need/PRD/story —
  EARS functional requirements, quantified NFRs, acceptance criteria, edge cases, and complete YAML
  front-matter. Use when the user asks to write, draft, create, or scaffold a spec / spec.md /
  feature specification / requirements for a feature.
metadata:
  layer: sdd-skills
  role: writer
  artifact: spec
---

# spec-writer

Draft a feature `spec.md` — the precise, testable description of intended behavior (the contract).
Describe **what** and **why**, never **how** (implementation belongs in `design.md`).

## Read first
- `../_shared/spec/template.md` — the eleven-section skeleton to emit.
- `../_shared/spec/checklist.md` — the quality bar to self-apply before finishing.
- `../_shared/spec/example.md` — a fully worked gold spec to imitate in shape and rigour.
- `../_shared/ears.md`, `../_shared/banned-words.md`, `../_shared/gherkin.md`, `../_shared/conventions.md`.

## Procedure
1. **Gather the upstream input** — the PRD / problem statement / story and the `need` link. If no
   need is traceable, say so and ask for one before writing (a spec with no need optimizes the
   wrong thing precisely).
2. **Fill front-matter first** — `id: NNNN-slug`, `status: draft`, `owner`, dates, `need`.
3. **Write goals *and* non-goals together** — for each goal, name the tempting adjacent thing you
   are *not* doing.
4. **Write functional requirements in EARS** (`../_shared/ears.md`), each singular, each with a
   stable `FR-n` ID. Cover the trio per behavior: happy path, boundary, unwanted (If/Then).
5. **Quantify the NFRs** — numbers and conditions, no banned words (`../_shared/banned-words.md`).
6. **Write acceptance criteria 1:1 to requirements** (`../_shared/gherkin.md`) — every `FR`/`NFR`
   has ≥1 criterion; every criterion names its ID.
7. **Pull edge cases into §7**, capture §8 data/interfaces, §9 dependencies/assumptions.
8. **Drive §10 Open questions to zero** if the spec is meant to be `ready`; otherwise leave
   `status: draft`.
9. **Self-apply `../_shared/spec/checklist.md`.** Fix everything you can before handing off.

## Output contract
- Write to `specs/NNNN-slug/spec.md` (default Layout A; honor a path the user gives instead).
- **Never overwrite an existing `spec.md`** — if it exists, stop and defer to `spec-rewriter`.
- Report the path and any checklist items you could not satisfy (e.g. an unresolved need).
