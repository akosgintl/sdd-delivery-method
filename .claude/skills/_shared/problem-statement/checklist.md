# Problem statement — quality checklist & smell test (bundled reference)

The rubric a `problem-statement-writer` self-applies and a `problem-statement-reviewer` checks
against. A problem statement is the **why** the whole feature bundle hangs from: a crisp description
of a real problem, stated *before* anyone picks a solution. Uses `../_shared/banned-words.md`.

## Anatomy — what goes in, smell if missing

| Part | Answers | Smell if missing |
|------|---------|------------------|
| Who has the problem | which user/segment, in their words | a solution looking for a victim |
| What the problem is | the pain, independent of any fix | "we need a dashboard" (that's a fix) |
| Why it matters / impact | the cost of *not* solving it, with evidence | a problem nobody will pay to solve |
| Success metric | the number that says we're done | "users are happier" (unmeasurable) |
| Constraints & assumptions | the boundaries; stated guesses | hidden assumptions that bite later |
| Out of scope | what this is *not* about | scope creep by silence |
| User stories | role + capability + benefit, pointing to a spec | a story with no "so that" |

## Smell test — flag if you see…

- **A solution wearing a problem's clothes** — any noun that's a feature, screen, or technology. `[BLOCKER]`
- **No named who** — "users", "people", "the business". `[MAJOR]`
- **No metric** — can't state how success is measured. `[MAJOR]`
- **No evidence** — a problem with no tickets/analytics/observation is a preference. `[MAJOR]`
- **Unstated assumptions.** `[MINOR]`
- **A story with no "so that"** — it's a task, not a need. `[MAJOR]`
- **A story treated as the spec.** `[MINOR]`

## Checklist

- [ ] States a **problem, not a solution** (no screens, tables, or tech named).
- [ ] Names *who* has the problem, specifically.
- [ ] States the impact / cost of not solving it, **with evidence**.
- [ ] Has a **quantified success metric** the spec can adopt as a goal.
- [ ] Assumptions and constraints are written down.
- [ ] Out-of-scope is explicit.
- [ ] Each user story has role + capability + benefit and points to a spec that will make it testable.
