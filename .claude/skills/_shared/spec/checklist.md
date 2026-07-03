# spec.md — quality checklist & smell test (bundled reference)

The rubric a `spec-writer` self-applies and a `spec-reviewer` checks against. A spec is the precise,
testable description of intended behavior — the contract. It describes **what** and **why**, never
**how**. Uses `../_shared/ears.md`, `../_shared/banned-words.md`, `../_shared/gherkin.md`,
`../_shared/conventions.md`.

## The eleven sections (what each carries)

| # | Section | Carries |
|---|---------|---------|
| — | Front-matter | `id, title, status, owner, created, updated, need, supersedes` |
| 1 | Summary & context | the problem in a paragraph; why now; link to the need |
| 2–3 | Goals & **non-goals** | what it does, and what it explicitly does *not* |
| 4 | Functional requirements | EARS statements, `FR-n` IDs |
| 5 | Non-functional requirements | quantified NFRs, `NFR-n` IDs |
| 6 | Acceptance criteria | Gherkin / checklist, 1:1 to requirements |
| 7 | Edge cases & error behavior | boundary and unwanted-behavior cases |
| 8 | Data & interfaces | schemas/contracts touched (or links) |
| 9 | Dependencies & assumptions | what it relies on; stated guesses |
| 10 | Open questions | **must be empty before `ready`** |
| 11 | Rationale / decisions | the *why* (or links to ADRs) |

> The two most-skipped sections carry the most weight: **non-goals** (scope creep enters through
> omission) and **edge cases & error behavior** (highest-value content).

## Smell test — flag (rewrite or remove) if you see…

- **"How" in the spec.** Class names, tables, frameworks, algorithms → belongs in the design. `[MAJOR]`
- **No non-goals.** Unbounded scope. `[MAJOR]`
- **Only the happy path.** §7 empty or thin; no boundary or error behavior. `[MAJOR]`
- **Acceptance criteria that don't map to IDs**, or a requirement with no verifying criterion. `[MAJOR]`
- **Anything in §10 Open questions** while `status` is `ready`/`done`. `[BLOCKER]`
- **An FR not in EARS form / not testable**, or using "should/may" instead of "shall". `[BLOCKER]`
- **Banned vague words** left un-quantified in a normative line. `[MAJOR]`
- **Unquantified NFR.** `[MAJOR]`
- **Stale front-matter.** `status`/`updated` not reflecting reality. `[MINOR]`
- **ID drift.** An `FR`/`NFR` referenced elsewhere that this spec never defines, or renumbered IDs. `[BLOCKER]`

## Checklist (apply before review / to pass review)

- [ ] Front-matter complete; `status` in the legal vocabulary; `updated` current; `need` links up.
- [ ] Every requirement has a stable `FR-n`/`NFR-n` ID and is singular, unambiguous, and testable.
- [ ] Every FR uses one of the five EARS patterns with **shall** / **shall not**.
- [ ] Goals **and** non-goals are both stated.
- [ ] Happy path, boundaries, and error behavior are all covered (§4 + §7).
- [ ] NFRs are quantified; banned vague words are gone or defined.
- [ ] Acceptance criteria map 1:1 to requirements (every ID ↔ ≥1 criterion).
- [ ] Open questions are empty (or the spec is explicitly `draft`).
- [ ] It traces up to a need and down to tests.
