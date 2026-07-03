# Review findings format (bundled reference)

Every reviewer skill emits a findings file in this format; every rewriter skill consumes it.
Self-contained — the schema below is authoritative for all `*-reviewer` / `*-rewriter` skills.

## File name & sequence

`<artifact-basename>.review-NN.md`, sitting **next to the artifact it reviews**.

- `NN` is zero-padded and **increments per review round**: the first review of `spec.md` is
  `spec.review-01.md`; after a rewrite, the next review is `spec.review-02.md`; and so on.
- A reviewer writes the **next** number = (highest existing `NN` for that artifact) + 1.
- A rewriter reads the **highest-numbered** review file.
- Review files are **append-only history**: never delete or overwrite an earlier round.

Examples: `spec.review-01.md`, `design.review-02.md`, `prd-checkout-revamp.review-01.md`.

## Schema

```markdown
---
artifact: spec            # spec | design | tasks | traceability | prd | problem-statement | constitution | adr
target: ./spec.md         # relative path to the reviewed artifact
round: 1                  # matches NN
reviewed: YYYY-MM-DD
verdict: changes-requested   # changes-requested | approved
---

# Review of <target> — round <NN>

## Findings

- [BLOCKER] FR-3 · not in EARS form / not testable · spec.md:24
  rule: EARS (../_shared/ears.md)
  fix: rewrite as "While <state>, the system shall <observable response>."
  resolved: [ ]

- [MAJOR] NFR-2 · vague word "secure", not quantified · spec.md:40
  rule: banned-words (../_shared/banned-words.md)
  fix: quantify — e.g. "encrypted at rest with AES-256; stores no payment-card data".
  resolved: [ ]

- [MINOR] §1 · summary is two paragraphs; tighten to one · spec.md:14
  rule: spec checklist
  fix: condense to a single problem+why-now paragraph.
  resolved: [ ]
```

Each finding is: `[SEVERITY] <id or section> · <one-line defect> · <file:line>` then indented
`rule:` (which convention/checklist it breaks + the `../_shared/…` source), `fix:` (a concrete,
applyable suggestion), and `resolved: [ ]`.

## Severity vocabulary

| Severity | Meaning | Blocks approval? |
|----------|---------|------------------|
| **BLOCKER** | Violates a hard rule; the artifact cannot advance its status | Yes |
| **MAJOR** | A real defect that should be fixed before the review passes | Yes |
| **MINOR** | Nit / polish; author's discretion | No |

## Verdict rule

`verdict: approved` **iff** no unresolved BLOCKER or MAJOR remains. MINOR findings do not block.

## Rewriter contract

The rewriter, working from the highest-numbered review file:
1. Applies each unresolved finding's `fix` to the artifact.
2. Flips that finding's `resolved: [ ]` → `resolved: [x]` **in place** (never deletes it).
3. Leaves `verdict` as-is — the *next* reviewer round re-judges and writes a new `review-NN.md`.
4. If a finding cannot be resolved in this artifact (root cause is upstream), it does **not**
   fake a fix: it adds an `escalation:` line naming the upstream artifact and leaves `resolved: [ ]`.

## Variants

- **Gate report** (`gate-runner`): same header with `artifact: gate`, `gate: DoR|DoD`; findings
  are gate criteria that failed, severity BLOCKER; `verdict: pass | blocked`.
- **Glossary proposals** (`glossary-maintainer`): findings are proposed/undefined terms; each `fix`
  is a proposed definition; the maintainer also appends the entries to the glossary as
  `status: proposed`.
