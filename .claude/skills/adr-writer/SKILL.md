---
name: adr-writer
description: >-
  Author a new Specification-Driven Development Architecture Decision Record (ADR) — computing the
  next global ADR number, capturing context, decision, real alternatives, and positive+negative
  consequences — or supersede an existing accepted ADR by writing a new one and editing only the old
  one's Status line. Use when the user asks to write, create, record, or supersede an ADR / an
  architecture decision / a design decision record.
metadata:
  layer: sdd-skills
  role: writer
  artifact: adr
---

# adr-writer

Author a new ADR — one decision per file, **immutable once accepted**. ADRs are the append-only
history of why the design became what it is.

## Read first
- `../_shared/adr/template.md`, `../_shared/adr/checklist.md`, `../_shared/adr/example.md`.
- `../_shared/conventions.md` (global numbering, status vocab).

## Procedure
1. **Compute the next global number** — scan the ADR directory (default `docs/adr/`) for the highest
   `NNNN-*.md` and use `highest + 1`, zero-padded. **Never reuse a number.**
2. **Capture the decision** per the template: Context (the forces + why now), Decision (active voice,
   "We will …"), Alternatives (real rejected options + why not), Consequences (positive **and**
   negative/trade-offs, plus follow-ups).
3. **Set Status** — `proposed` or `accepted`; add Deciders, Date, Related (spec/feature id; the
   constitution clause if this is an exception — then include an expiry/review date).
4. **Superseding an existing decision?** Do **not** edit the old ADR's body — write the new ADR, and
   edit **only** the old one's Status line to `superseded by ADR-<new>`.
5. **Self-apply `../_shared/adr/checklist.md`.**

## Output contract
- Write to `docs/adr/NNNN-short-title.md` (default; honor a user-given path). Never overwrite an
  existing ADR file.
- Report the new ADR number, its status, and (if superseding) the old ADR's updated status line.
