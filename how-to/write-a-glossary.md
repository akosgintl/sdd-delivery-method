# How to write a good glossary (ubiquitous language)

A glossary is the project's **shared vocabulary**: one agreed meaning per domain term, used
identically in prose, code identifiers, and tests. It is a force multiplier — ambiguity in a *term*
is ambiguity in every spec that uses it, every test that asserts it, and every prompt an
[agent](../docs/appendices/A-glossary.md#agent) reads. The discipline is restraint: a glossary that defines
everything teaches nothing, while one that nails the handful of genuinely contested words collapses
most "but I thought you meant…" arguments before they start. This is the *doing* companion to
[`docs/03 — Glossary`](../docs/03-artifacts.md#glossary--ubiquitous-language) and the worked example in
[`appendix A`](../docs/appendices/A-glossary.md).

> A term earns a glossary entry only if **two people could otherwise mean two different things by
> it.** The glossary exists to close that gap — not to restate the dictionary.

## When you write it

**Once per project**, early, then **grown continuously** — every time a spec review surfaces a word
two people read differently, the resolution lands here. Owned jointly by product and engineering
(the whole point is that both sides use the same words). It lives at a fixed, well-known path so
specs, code, and tests can all point at it.

## Anatomy: what goes in (and what doesn't)

The template is three columns — **Term · Definition · Notes / synonyms to avoid** — and that third
column is the secret weapon: it names the ambiguous synonyms you must *not* use, so the glossary
actively prevents drift rather than passively recording it.

| Goes in | Stays out |
|---------|-----------|
| Domain terms with contested meaning ("cart", "expired", "active user") | general English; software jargon everyone shares |
| Words your spec uses normatively | words that appear once and never recur |
| Terms whose synonyms cause confusion ("basket" vs "cart") | terms already precise from context |
| Status/state vocabulary the system acts on | implementation detail (→ design/code) |

A good entry is **single-meaning** (one definition, not "usually X but sometimes Y"), **used
verbatim** (the same word appears in code and tests), and **synonym-pruned** (it tells you what
*not* to say).

## The recipe

1. **Harvest from real conflict.** Don't brainstorm a dictionary. Mine spec reviews, bug reports,
   and standups for the words people keep clarifying. Those are your entries.
2. **Write one meaning per term.** If a word genuinely has two meanings, that's two terms (e.g.
   "cart" vs "expired cart") — split them, don't hedge.
3. **Name the banned synonyms.** For each term, list the words people reach for instead ("not
   'basket', not 'order'"). This is what stops the term decaying back into ambiguity.
4. **Make code and tests obey it.** The agreed term should be the identifier in the codebase and the
   word in the test names. A glossary the code ignores is decoration.
5. **Cross-link, don't duplicate.** If a term is a method concept (requirement, spec, DoR), point at
   [`appendix A`](../docs/appendices/A-glossary.md) rather than redefining it; the project glossary is for
   *domain* terms.
6. **Prune as you grow.** Drop entries that stopped being contested; a glossary that only contains
   live ambiguities stays readable.

## Before → After

**Vague entry → single, enforceable meaning**

> ✗ *Before:* "**Cart** — the user's items." *(Whose items? When? Is a saved-for-later list a cart?
> Two readers will diverge.)*
>
> ✓ *After:* "**Cart** — a logged-in user's set of selected line items prior to checkout. *Notes:*
> not 'basket', not 'order'." Now `FR-1` ("When an item is added to the cart…") has exactly one
> reading, and the code uses `Cart`, not `Basket`.

**Two meanings smuggled into one entry → two terms**

> ✗ *Before:* "**Active** — a user who is logged in, or has bought something this month."
> *(Two definitions; a requirement saying "active users shall…" is now untestable.)*
>
> ✓ *After:* "**Signed-in user** — has a live session. **Active customer** — placed ≥1 order in the
> trailing 30 days." Each is independently checkable; specs pick the one they mean.

## Smell test — rewrite or remove if you see…

- **A definition that's just the dictionary.** If general English already pins it down, cut it.
- **"Usually" / "typically" / "or".** Hedged definitions mean you haven't split a hidden second
  term.
- **No banned-synonym note** on a word that *has* common synonyms ("basket", "order").
- **Glossary says one thing, code says another.** The identifier in the codebase must match.
- **Entries nobody references.** A term no spec uses normatively is clutter — prune it.
- **Method terms redefined.** "Spec", "requirement", "DoD" belong in [`appendix A`](../docs/appendices/A-glossary.md);
  link, don't restate.

## Checklist

- [ ] Every entry exists because the term was genuinely ambiguous.
- [ ] Exactly one meaning per term (genuine double-meanings are split into two terms).
- [ ] Each entry names the synonyms *not* to use.
- [ ] The agreed term is the one used in code identifiers and test names.
- [ ] Method-level terms link to [`appendix A`](../docs/appendices/A-glossary.md) rather than being redefined.
- [ ] It lives at a fixed, well-known path and is referenced from specs.
- [ ] Stale, no-longer-contested entries have been pruned.

## Links

- **Template:** [`templates/glossary.md`](../templates/glossary.md).
- **Concept:** [`docs/03` — Glossary / Ubiquitous language](../docs/03-artifacts.md#glossary--ubiquitous-language);
  worked method glossary in [`appendix A`](../docs/appendices/A-glossary.md).
- **Related:** the glossary is what [DoR](write-dor-dod-gates.md) checks ("terms align with the
  glossary"); it kills the [banned vague words](write-ears-requirements.md) by giving them precise
  meanings.
