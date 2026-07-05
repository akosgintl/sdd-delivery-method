# Glossary — quality checklist & smell test (bundled reference)

The rubric a `glossary-writer` self-applies and a `glossary-maintainer` lints against. A glossary is
the project's shared vocabulary — one agreed meaning per domain term, used identically in prose, code
identifiers, and tests. The discipline is **restraint**: a term earns an entry only if two people
could otherwise mean two different things by it.

## Anatomy — three columns

**Term · Definition · Notes / synonyms to avoid.** The third column is the secret weapon: it names
the ambiguous synonyms you must *not* use, so the glossary actively prevents drift.

| Goes in | Stays out |
|---------|-----------|
| Domain terms with contested meaning ("cart", "expired", "active user") | everyday words of the artifact's base language (e.g. general English/Hungarian); shared jargon |
| Words a spec uses normatively | words that appear once and never recur |
| Terms whose synonyms cause confusion ("basket" vs "cart") | terms already precise from context |
| Status/state vocabulary the system acts on | implementation detail (→ design/code) |

A good entry is **single-meaning**, **used verbatim** (same word in code and tests), and
**synonym-pruned**.

## Smell test — flag if you see…

- **A definition that's just the dictionary.** `[MINOR]`
- **"Usually" / "typically" / "or"** — a hedged definition hiding a second term; split it. `[MAJOR]`
- **No banned-synonym note** on a word with common synonyms ("basket", "order"). `[MAJOR]`
- **Glossary says one thing, code says another** — the identifier must match. `[MAJOR]`
- **Entries nobody references** — a term no spec uses normatively is clutter. `[MINOR]`
- **Method terms redefined** ("spec", "requirement", "DoD") — those are method vocabulary, link don't restate. `[MINOR]`
- **An undefined contested term** used normatively in a spec/design/prd. `[MAJOR]`

## Checklist

- [ ] Every entry exists because the term was genuinely ambiguous.
- [ ] Exactly one meaning per term (genuine double-meanings are split into two terms).
- [ ] Each entry names the synonyms *not* to use.
- [ ] The agreed term is the one used in code identifiers and test names.
- [ ] It lives at a fixed, well-known path and is referenced from specs.
- [ ] Stale, no-longer-contested entries have been pruned.
