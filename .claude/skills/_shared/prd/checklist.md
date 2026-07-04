# PRD (vision) — quality checklist & smell test (bundled reference)

The rubric a `prd-writer` self-applies and a `prd-reviewer` checks against. A PRD is the
product-level **why and what** for an initiative too big for one feature; it sits upstream of specs
and its power (and main failure mode) is **altitude** — it must stay at goals/users/scope/metrics and
leave behavioral precision to the specs. Uses `../_shared/banned-words.md`.

## Anatomy — what goes in, what stays out

| Section | Holds | Stays out |
|---------|-------|-----------|
| Problem & opportunity | who hurts, what it costs, evidence | the solution design |
| Goals & success metrics | outcomes + numbers + dates | feature-level acceptance criteria |
| Target users & needs | personas, jobs-to-be-done | UI specifics |
| Scope | in-scope capabilities + **non-goals** | EARS requirements |
| Constraints & assumptions | regulatory, technical, timeline, budget | implementation choices (→ design) |
| Feature breakdown → specs | table of features, each → a spec | the specs themselves |
| Risks & open questions | initiative-level risks | per-feature edge cases |

The distinguishing section is **feature breakdown → specs**: each row is a feature that becomes its
own `spec.md` — the seam between the PRD and the per-feature bundles.

## Smell test — flag if you see…

- **Requirements-level detail** (EARS lines, edge cases, schemas) — push down into specs. `[MAJOR]`
- **Goals without metrics** — a goal you can't measure is a slogan. `[MAJOR]`
- **No non-goals** — unbounded scope. `[MAJOR]`
- **A feature list that isn't a list of specs** — if a row can't become its own `spec.md`, it isn't
  decomposed yet. `[MAJOR]`
- **Restating the constitution** — project-wide rules live there, not in every PRD. `[MINOR]`
- **Banned vague words** in a goal/metric ("make the cart more reliable"). `[MAJOR]`
- **A solution presented as the problem** in §1. `[MAJOR]`
- **Hidden cross-feature cycles / no build order** — if feature rows depend on each other circularly
  (feature A needs B and B needs A), name the seam (events/contracts) and a build order, or the
  integration risk stays implicit. `[MINOR]`

## Checklist

- [ ] Opens with an **evidenced problem**, not a solution.
- [ ] Every goal has a **metric and a target date**.
- [ ] Target users and their jobs-to-be-done are named.
- [ ] Scope states both in-scope capabilities **and** non-goals.
- [ ] The feature-breakdown table maps each feature to a `spec.md` path.
- [ ] Constraints, assumptions, and initiative-level risks are recorded.
- [ ] Cross-feature dependencies (and any circular/build-order risk between feature rows) are noted
      where features are not independent.
- [ ] No behavioral / requirement-level detail has leaked in from the specs.
- [ ] Front-matter (`type: prd, title, status, owner, updated`) complete; `status` legal.
