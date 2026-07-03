---
name: glossary-maintainer
description: >-
  Lint a Specification-Driven Development project against its glossary — scan specs/design/PRD for
  undefined contested terms, circular definitions, duplicate/synonym drift, and missing cross-links —
  then emit a proposals report (glossary.review-NN.md) and append proposed entries to the glossary as
  status: proposed for human confirmation. Use when the user asks to maintain, lint, audit, or extend
  the glossary, or to find undefined/ambiguous terms.
metadata:
  layer: sdd-skills
  role: maintainer-linter
  artifact: glossary
---

# glossary-maintainer (linter)

Keep the glossary alive: find where the project's vocabulary has drifted or gone undefined, propose
fixes, and stage new entries for a human to confirm. This is the glossary's "review" role — there is
no glossary rewriter; the `glossary-writer` promotes accepted proposals.

## Read first
- `../_shared/glossary/checklist.md` — the lint rules (smell test, severities).
- `../_shared/glossary/example.md`.
- `../_shared/review-format.md` — the findings/proposals format (glossary-proposal variant), naming.

## Procedure
1. **Load the glossary** and the artifacts to scan (specs/design/PRD in scope); find existing
   `glossary.review-NN.md`; write `glossary.review-<highest+1>.md`.
2. **Lint:**
   - **undefined contested terms** used normatively in a spec/design/PRD but not in the glossary;
   - **circular / hedged definitions** ("usually X or Y") — a hidden second term;
   - **duplicate / synonym drift** (a spec says "basket" where the glossary says "cart");
   - **glossary↔code mismatches** and **missing cross-links**;
   - **stale entries** no spec references (prune candidates).
3. **Emit findings** per `../_shared/review-format.md`: each is a term, the issue, `file:line`, and a
   proposed definition as the `fix`.
4. **Stage proposals:** append the proposed new entries to the glossary marked `status: proposed`
   (so nothing is silently added — a human, via `glossary-writer`, promotes them).

## Output contract
- Write `glossary.review-NN.md` (the proposals report) next to the glossary, and append
  `status: proposed` entries to the glossary itself.
- Report undefined terms, drift, and prune candidates. Accepted proposals are promoted by
  `glossary-writer`.
