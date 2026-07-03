# Glossary — Ubiquitous Language (ShopCo Web)

> Agreed definitions of domain terms, shared by specs, code, tests, and AI agents.

| Term | Definition | Notes / synonyms to avoid |
|------|------------|---------------------------|
| Cart | A logged-in user's set of selected line items prior to checkout. | not "basket", not "order" |
| Expired (cart) | A persisted cart with no activity for > 24h; not eligible for restore. | not "stale" |
| Signed-in user | A user with a live authenticated session. | not "active user" |
| Active customer | A customer who placed ≥ 1 order in the trailing 30 days. | not "active user" |
| Line item | One product + quantity entry within a cart. | not "row", not "item" (alone) |

*Rule: if a word in a spec could mean two things to two readers, define it here or replace it.*
