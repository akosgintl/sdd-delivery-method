---
name: problem-statement-writer
description: >-
  Draft a Specification-Driven Development problem statement — a real problem stated before any
  solution, with a named who, evidenced impact, a quantified success metric, explicit scope, and
  user stories that point to specs. Use when the user asks to write, draft, or frame a problem
  statement / the "why" / user needs / user stories at the start of an initiative or feature.
metadata:
  layer: sdd-skills
  role: writer
  artifact: problem-statement
---

# problem-statement-writer

Draft a problem statement — the **why** the feature bundle hangs from, stated before anyone picks a
solution.

## Read first
- `../_shared/problem-statement/template.md`, `../_shared/problem-statement/checklist.md`,
  `../_shared/problem-statement/example.md`.
- `../_shared/banned-words.md` (the metric must be quantified).

## Procedure
1. **Start from evidence, not a request** — tickets, analytics, an interview quote, an observed
   workaround. A problem with no evidence is a preference.
2. **Strip the solution back out** — when handed "we need X", ask "what would X let you do?" until no
   solution is named. Use Jobs-to-be-Done to separate need from solution.
3. **Name who, specifically** — "logged-in customers mid-checkout", not "users".
4. **Quantify impact and the success metric** — the metric becomes the spec's goal.
5. **Write assumptions and constraints down; bound the scope** (explicit out-of-scope).
6. **Turn it into one or two user stories**, each with a benefit clause, each pointing to a spec.
7. **Self-apply `../_shared/problem-statement/checklist.md`.**

## Output contract
- Write to `docs/product/problem-<slug>.md` (default; or fold into a spec's §1 / a PRD's §1 if the
  user prefers). Honor a user-given path.
- **Never overwrite an existing problem statement** — defer to `problem-statement-rewriter`.
- Report the path and the success metric downstream specs should adopt.
