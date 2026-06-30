# D — EARS Requirements Cheat-Sheet

The one-page card to keep open while you write requirements. It compresses the EARS section of
[`04 — From Needs to Spec`](../04-from-needs-to-spec.md#4-writing-requirements-ears) and the craft
guide [`write-ears-requirements.md`](../../how-to/write-ears-requirements.md) into a reference you can
scan in ten seconds — it does not replace them, so read those for the *why*.

> Rule of thumb: if you can't name the **trigger or precondition**, you haven't found the
> requirement yet — you've found a wish.

## The five patterns

Every EARS requirement uses **shall** (obligation) or **shall not** (prohibition), plus one shape:

| # | Pattern | Skeleton | One-line example |
|---|---------|----------|------------------|
| 1 | **Ubiquitous** (always true) | *The `<system>` shall `<response>`.* | The payment service **shall** record every authorization attempt in the audit log. |
| 2 | **Event-driven** (`When`) | *When `<trigger>`, the `<system>` shall `<response>`.* | **When** the user submits checkout, the system **shall** validate all required fields first. |
| 3 | **State-driven** (`While`) | *While `<state>`, the `<system>` shall `<response>`.* | **While** the account is suspended, the system **shall** reject all outbound transfers. |
| 4 | **Optional-feature** (`Where`) | *Where `<feature included>`, the `<system>` shall `<response>`.* | **Where** 2FA is enabled, the system **shall** require a second factor on login from a new device. |
| 5 | **Unwanted behavior** (`If/Then`) | *If `<unwanted trigger>`, then the `<system>` shall `<response>`.* | **If** the provider does not respond within 5 s, **then** the system **shall** cancel the attempt without charging. |

**Compound** (combine only when genuinely necessary):
> *While `<state>`, when `<trigger>`, the `<system>` shall `<response>`.*
> **While** the store is in maintenance mode, **when** a customer attempts checkout, the system
> **shall** display the maintenance notice and **shall not** create an order.

## Modality

- **`shall`** = the system's obligation. **`shall not`** = prohibition. Nothing else carries
  normative weight — drop "should", "may", "will", "must" from requirement statements.
- One requirement per statement (**singular**). Split every "and"/"or" into separate requirements.
- Make the response **observable**: "shall persist the draft to local storage within 200 ms" beats
  "shall handle the draft."

## Banned vague words (quantify instead)

Never normative: **fast, easy, robust, secure, scalable, support, handle, and/or, user-friendly.**
Each hides an untestable claim — replace it with a number or a defined term. See
[`04 §3.3`](../04-from-needs-to-spec.md#33-words-to-ban-or-define) and quantify NFRs in
[`E`](E-tooling-comparison.md)'s sibling reference, the spec template
[`templates/specification.md`](../../templates/specification.md).

> ✗ "The system shall be fast and handle many users."
> ✓ "When 1,000 users are concurrently active, the system **shall** return the cart page in ≤300 ms
> at the 95th percentile."

## The testability test

For every requirement, ask: **could I write one pass/fail check for it, with no further questions?**

- If yes → it's a requirement.
- If no → it's still a need or an opinion; sharpen the trigger, the system, or the response until a
  test falls out. Cover the trio for each behavior: **happy path, boundary, unwanted condition.**

---

Continue to [`E — Tooling Comparison Matrix`](E-tooling-comparison.md), or return to the
[README](../../README.md) for the full table of contents.
