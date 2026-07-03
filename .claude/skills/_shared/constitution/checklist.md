# Constitution — quality checklist & smell test (bundled reference)

The rubric a `constitution-writer` self-applies and a `constitution-reviewer` checks against. The
constitution is the project's **standing law**: the short list of non-negotiable rules every spec and
implementation honors. Its power is being **short, stable, and enforceable**. Uses
`../_shared/banned-words.md`.

## Anatomy — six buckets of checkable clauses with stable IDs

| Section | Holds | Example clause |
|---------|-------|----------------|
| Engineering principles (`P-`) | how you build, universally | "Every feature ships with tests that verify its spec." |
| Architectural constraints (`A-`) | structural non-negotiables | "The presentation layer never accesses the database directly." |
| Quality bars (`Q-`) | the numeric floor | "p95 API latency < 300 ms at 1000 RPS." |
| Technology constraints (`T-`) | approved/banned tech, data rules | "PII stays in region X; retention ≤ 90 days." |
| Process rules (`R-`) | how work moves | "Each behavior change updates its `spec.md` in the same PR." |
| Amendments & exceptions | how the rules change | "Exceptions are recorded as an ADR with a stated expiry." |

A good constitution is **stable**, **short** (a page or two — longer means it's a style guide),
**enforceable** (each clause checkable, ideally in CI), and **universal within scope**.

## Smell test — flag if you see…

- **Unenforceable adjectives** ("robust", "clean", "scalable") with no number/check. `[MAJOR]`
- **Style-guide material** (formatting, naming, file layout) — push to a linter. `[MAJOR]`
- **Per-feature specifics** — anything not true for *every* feature belongs in a spec. `[MAJOR]`
- **No "how it's checked"** on a clause. `[MAJOR]`
- **No amendment/exception path.** `[BLOCKER]`
- **Length** past a page or two. `[MINOR]`
- **A quality bar left unquantified** (banned vague word). `[MAJOR]`

## Checklist

- [ ] Every clause is universal (applies to all features), non-negotiable, and enforceable.
- [ ] Quality bars are quantified with numbers and named standards.
- [ ] Each clause notes how it's enforced (CI / review / tooling).
- [ ] An amendment process and an ADR-based exception process are defined.
- [ ] No style-guide or per-feature detail has crept in.
- [ ] It's short enough to read in one sitting and uses stable IDs (`P-`, `A-`, `Q-`, `T-`, `R-`).
- [ ] Front-matter (`type, version, ratified, last_amended, owner`) present.
