# Language packs — localizing the normative rules

The SDD skills validate against **English keyword tokens** (`shall`, `When/While/Where/If/Then`) and an
**English banned-word list**. Artifacts, however, may be authored in any natural language. A **language
pack** carries the equivalents of those tokens so writers and — critically — reviewers keep their full
validation depth in that language, instead of silently passing localized vague words and mis-patterned
requirements.

> **Why this exists.** In a Hungarian test-run of the pipeline, reviewers kept full depth *only* because
> a Hungarian rule mapping was hand-injected into every prompt. Without it, a reviewer would have
> silently approved Hungarian vague words (`megbízható`, `gyors`) and a mis-classified EARS pattern. A
> language pack makes that mapping a first-class, self-loaded file so the validation is not lost.

## The invariant: additive, never replacing

- **English is the default and the source of truth.** A pack does **not** rewrite the English rules; it
  supplies the *parallel tokens* to be applied **in addition to** `ears.md`, `banned-words.md`, and
  `gherkin.md`.
- **Machine vocabulary stays English in every language.** Never localize: front-matter keys
  (`id, status, owner, need, …`), status values (`draft, in-review, ready, in-progress, done,
  superseded, agreed, proposed, accepted`), stable IDs (`FR-<n>, NFR-<n>, T-<n>, ADR-NNNN`),
  cross-references (`0007/FR-1`), test-naming (`__FR2`, `@requirement(...)`, `@FR-3`). These caused
  zero friction in the test-run and keep tooling/traceability portable.

## How a pack is loaded

Each of `ears.md`, `banned-words.md`, and `gherkin.md` carries an **"Output language"** block. When the
artifact under review is written in a non-English language `<lang>`, the writer/reviewer/rewriter also
reads `../_shared/languages/<lang>.md` and applies its mapping. **If no pack exists for that language,
that is itself a `[MAJOR]` finding** (process risk) — not a silent pass.

## Pack file format

A pack is one Markdown file named by ISO-639-1 code (`hu.md`, `de.md`, `fr.md`) containing, in order:

1. **EARS keyword mapping** — the five patterns' keywords and the obligation/prohibition/soft-modal
   words in the target language.
2. **Sanctioned-compound phrasing** — how "shall X and shall not Y" reads in the language.
3. **Banned vague-word list** — the target-language equivalents of `banned-words.md`'s offenders.
4. **Gherkin mapping** — Given/When/Then equivalents (acceptance criteria keywords).
5. **Stays-English list** — the machine vocabulary above, restated so the pack cannot be over-applied.

## Available packs

| Code | Language | Status |
|------|----------|--------|
| `hu` | Hungarian | validated (event-ticketing test-run) |

To add a language, copy `hu.md`, translate each section, and add a row here.
