# `_shared/` ↔ study sync map

The skills are self-contained: `_shared/` **deliberately duplicates** rules that also live in the
study (`docs/`), so the skills travel without the study. The cost is a **manual sync obligation** —
when a rule changes on either side, update its twin. This file is the checklist of twins; consult it
whenever you touch a rule in `_shared/` **or** in `docs/`.

| `_shared/` file (skills) | Study source (`docs/`) | Rule kept in sync |
|--------------------------|------------------------|-------------------|
| `ears.md` | `04-from-needs-to-spec.md` §4 | the five EARS patterns; shall/shall not; compound & soft smells |
| `banned-words.md` | `04-from-needs-to-spec.md` §3.3 | the banned vague-word list + quantify rule |
| `gherkin.md` | `04-from-needs-to-spec.md` (acceptance) | Given/When/Then; 1:1 acceptance ↔ requirement |
| `conventions.md` | `05-storage-and-organization.md`, `03-artifacts.md` | `FR-/NFR-/T-` IDs, feature/ADR numbering, status vocabularies, `__FR<n>` test-naming |
| `spec/{template,checklist}.md` | `03-artifacts.md`, `04-from-needs-to-spec.md` | the 11 spec sections + smell test |
| `traceability/checklist.md` | `09-quality-and-traceability.md` §4, `07-definition-of-done.md` | coverage is a `done` gate (planned test names at `ready`); never hand-patch the matrix |
| `gates/{dor-template,gate-criteria}.md` | `06-definition-of-ready.md` | DoR criteria |
| `gates/{dod-template,gate-criteria}.md` | `07-definition-of-done.md` | DoD criteria incl. the two SDD-defining clauses |
| `constitution/{template,checklist}.md` | `01-principles.md`, `03-artifacts.md` | six clause buckets; quantified quality bars; enforceability |
| `adr/{template,checklist}.md` | `03-artifacts.md` | one decision/file; append-only immutability |
| `glossary/{template,checklist}.md` | `04-from-needs-to-spec.md` §3.3, `03-artifacts.md` | one meaning per contested term; banned synonyms |
| `workflow.md` | `02-lifecycle.md`, `08-roles-and-workflow.md` | the P0→P5 pipeline; the write→review→rewrite loop |
| `languages/*.md` | *(no twin yet)* | localized EARS keywords + banned-word lists per output language; the "Output language" blocks in `ears.md`/`banned-words.md`/`gherkin.md` point to them |

## How to use it

- **Changed a rule in `docs/`?** Find its `_shared/` twin above and update it (the original sync
  obligation, also stated in `CLAUDE.md`).
- **Changed a rule in `_shared/`** (e.g. to fix a skill)? Update the study twin too — otherwise the
  study and the skills drift. *(This has bitten us: a traceability-skill fix required patching
  `docs/09`/`docs/07` in the same session.)*
- **No twin listed?** Then it's skills-only orchestration (`review-format.md` sequencing, the
  `sdd-loop` control flow, the `languages/` packs) with no study counterpart to sync. If a language
  mechanism is later documented in `docs/`, add its row here and keep the two in sync.
