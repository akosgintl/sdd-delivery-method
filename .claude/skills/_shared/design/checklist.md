# design.md — quality checklist & smell test (bundled reference)

The rubric a `design-writer` self-applies and a `design-reviewer` checks against. The design is
**how** we satisfy the spec; observable behavior belongs in the spec, not here. A good design is
judged by whether a reader can see **why this approach and not the alternatives**, and whether every
spec requirement has a stated path to being built and verified. Uses `../_shared/conventions.md`.

## The nine sections (load-bearing ones + the trap)

| Section | Carries | The trap |
|---------|---------|----------|
| Overview | the chosen approach in a few sentences | starting with detail, not the shape |
| Approach & alternatives | options table with the **rejected** ones + why | listing only the winner |
| Components & responsibilities | modules/services and what each owns | a diagram with no responsibilities |
| Interfaces & contracts | APIs/schemas/events; which spec req each realizes | contracts with no link to requirements |
| Data model | entities, relationships, constraints, migrations | re-deriving behavior the spec already fixed |
| Integration points & dependencies | external systems, flags, sequencing | hidden coupling found at build time |
| Test strategy | how each `FR`/`NFR` is verified, by ID | "we'll write tests" with no mapping |
| Risks & mitigations | what could go wrong, likelihood, plan | optimism |
| Rollout & operability | deploy, migration, rollback, observability | shipping with no way to operate it |

Front-matter: `id, artifact: design, status (draft|in-review|agreed|done), owner, updated, spec:`.

> **ADR rule:** significant / hard-to-reverse decisions (and any constitution exception) get their
> own ADR — one decision per file, **immutable once accepted**. To change a decision, write a new
> superseding ADR; never edit or delete the old one.

## Smell test — flag if you see…

- **Behavior in the design** — observable-from-outside content belongs in the spec. `[MAJOR]`
- **Only the chosen option** — no rejected alternatives = no visible reasoning. `[MAJOR]`
- **Boxes without responsibilities**, or contracts that map to no requirement. `[MAJOR]`
- **A test strategy that doesn't reference spec IDs** — can't anchor the DoD. `[BLOCKER]`
- **A spec `FR`/`NFR` with no build+verify path** in the design. `[BLOCKER]`
- **No rollback / observability plan.** `[MAJOR]`
- **An edited or deleted ADR**, or a constitution exception with no ADR. `[BLOCKER]`
- **Front-matter `spec:` link missing or unresolved.** `[MAJOR]`

## Checklist

- [ ] Every spec `FR`/`NFR` has a stated path to being built and verified (test strategy maps each ID).
- [ ] The alternatives table includes rejected options with reasons.
- [ ] Components list responsibilities, not just names.
- [ ] Interfaces and data model link back to the requirements they realize.
- [ ] Rollout, rollback, and observability are planned.
- [ ] Significant / hard-to-reverse decisions (and constitution exceptions) are ADRs.
- [ ] ADRs are one-per-file, immutable, superseded (never edited) when they change.
- [ ] Front-matter complete; `spec:` resolves; `status` legal; `updated` current.
