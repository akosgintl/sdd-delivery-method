# ADR — quality checklist & smell test (bundled reference)

The rubric an `adr-writer` self-applies and an `adr-reviewer` checks against. An ADR is one decision
per file, **immutable once accepted** — to change a decision you write a new ADR that supersedes the
old one; you never edit or delete an accepted ADR (only its Status line may change to mark
supersession). Uses `../_shared/conventions.md`.

## Anatomy

- Header: `ADR-NNNN` (globally numbered, never reused), **Status** (`proposed | accepted |
  superseded by ADR-MMMM | deprecated`), Date, Deciders, Related (spec/feature id; constitution
  clause if this is an exception).
- **Context** — the forces: problem, constraints, why now.
- **Decision** — the choice, active voice ("We will …").
- **Alternatives considered** — real rejected options, each with *why not*.
- **Consequences** — positive **and** negative/trade-offs; follow-ups (incl. an expiry/review date
  if it's a constitution exception).

## Smell test — flag if you see…

- **An edited or deleted accepted ADR** (anything beyond the Status line changed). `[BLOCKER]`
- **Only the chosen option** — no rejected alternatives = no visible reasoning. `[MAJOR]`
- **No negative consequences** — every decision has trade-offs; missing them is optimism. `[MAJOR]`
- **A constitution exception with no expiry/review date.** `[MAJOR]`
- **A reused / non-global ADR number**, or a superseding ADR that edits the old one's body. `[BLOCKER]`
- **Vague context** — the reader can't tell why the decision was necessary. `[MINOR]`
- **Status not in the legal set.** `[MAJOR]`

## Checklist

- [ ] One decision, one file; a **globally unique** `ADR-NNNN` number (never reused).
- [ ] Status is legal and current; a superseded ADR links to its successor and is otherwise unedited.
- [ ] Context explains the forces and why now.
- [ ] Decision is stated in active voice.
- [ ] Alternatives include real rejected options with reasons.
- [ ] Consequences list positives **and** negatives/trade-offs (+ expiry if a constitution exception).
- [ ] Immutability respected — to change a decision, a new superseding ADR is written.
