# Glossary — Ubiquitous Language

> Agreed definitions of domain terms, shared by specs, code, tests, and AI agents. Ambiguity in
> terms is ambiguity in the spec. Lives at a fixed, well-known path. See
> [docs/03](../docs/03-artifacts.md#glossary--ubiquitous-language).

| Term | Definition | Notes / synonyms to avoid |
|------|------------|---------------------------|
| <Term> | <Single, agreed meaning used everywhere — in prose, code identifiers, and tests.> | <Ambiguous synonyms NOT to use> |
| Cart | A logged-in user's set of selected line items prior to checkout. | not "basket", not "order" |
| Expired (cart) | A persisted cart with no activity for > 24h; not eligible for restore. | |

*Rule: if a word in a spec could mean two things to two readers, define it here or replace it.*
