# How to write good acceptance criteria

A requirement says what the system *shall* do. **Acceptance criteria** prove it — they are the
concrete, pass/fail conditions that turn a requirement from a claim into something a tester (or a
test runner) can sign off. Get them right and "done" becomes objective; get them wrong and every
demo turns into an argument about what "working" meant. This is the *doing* companion to
[`docs/04 §5`](../docs/04-from-needs-to-spec.md#5-writing-requirements-bdd--gherkin-acceptance-scenarios).

> A requirement without acceptance criteria is an opinion. Acceptance criteria are where the
> requirement either becomes testable or gets exposed as a wish.

## When you write it

In the **specify** phase, alongside the requirements they verify, in section 6 of the `spec.md`.
Written by the spec author with the tester/QA's eye — ideally *before* any code, because they are
the seed of the automated tests. They must be settled before the spec passes the
[Definition of Ready](../docs/06-definition-of-ready.md).

## Anatomy: requirement vs. criterion vs. scenario

- A **requirement** (EARS) states the *rule*: "When X, the system shall Y."
- An **acceptance criterion** states a *checkable condition of satisfaction* for that rule.
- A **scenario** (Gherkin **Given/When/Then**) is a concrete *example* of the rule passing or
  failing — and it maps cleanly onto EARS: **Given** = precondition/state, **When** = trigger,
  **Then** = the observable response.

Two valid forms — pick per criterion:

| Form | Looks like | Use when |
|------|-----------|----------|
| Gherkin scenario | `Given … When … Then …` | the example aids understanding, or you want it to *become* the automated test |
| Checklist criterion | `- [ ] <observable, pass/fail condition>` | a simple boolean check needs no narrative |

## The recipe

1. **Start from a requirement.** Every `FR-n` needs at least one criterion. Note which FR each
   criterion verifies (`# verifies FR-3`).
2. **Pick the form** — Gherkin scenario or checkbox — by whether a concrete example earns its keep.
3. **Write Given / When / Then.** Given = the precondition and state; When = the single trigger;
   Then = the observable outcome. One trigger per scenario.
4. **Use concrete data.** "3 items," "after 24 hours," "within 500 ms" — not "some items," "later."
   Concrete examples are testable; abstract ones drift.
5. **Make the *Then* assertable.** It must name a state or output you can check: "the cart contains
   the same 3 items," not "the cart works."
6. **Cover the trio.** For each behavior write the happy path, a boundary, and the unwanted/error
   case. The error scenarios are the ones that catch real bugs.
7. **Check 1:1 coverage.** Every requirement has ≥1 criterion; every criterion traces to a
   requirement. Orphans on either side are a smell.

## Before → After

**Untestable criterion → concrete scenario**

> ✗ *Before:*
> ```gherkin
> Scenario: Cart works after crash
>   Given a user with a cart
>   When the browser crashes
>   Then the cart should still work
> ```
> *(“a cart”, “should still work” — nothing here is assertable.)*
>
> ✓ *After:*
> ```gherkin
> Scenario: Cart is restored after a crash      # verifies FR-2
>   Given a logged-in customer with 3 items in their cart
>   And the cart has not been checked out
>   When the browser crashes and they reopen the site within 24 hours
>   Then the cart contains the same 3 items
> ```

**Missing the unwanted case → add the error scenario**

> ✗ *Before:* only the happy-path "cart is restored" scenario exists.
>
> ✓ *After:* add the boundary/expiry case so "done" covers it:
> ```gherkin
> Scenario: Expired cart is not restored        # verifies FR-3
>   Given a logged-in customer with items in their cart
>   When 24 hours pass with no activity
>   Then the cart is cleared
>   And no items are restored on next login
> ```

## Smell test — rewrite if you see…

- **Restates the requirement.** A criterion that just echoes the FR adds no check. It must describe
  *how you'd observe* the rule holding.
- **"works / correctly / as expected" in the *Then*.** Non-assertable. Name the concrete outcome.
- **UI-coupled steps.** "click the blue Save button" ties the test to a layout. Assert behavior
  ("the draft is persisted"), not pixels — unless the UI *is* the requirement.
- **Non-deterministic.** Depends on timing, ordering, or external state you don't control. Pin it
  down with explicit Given preconditions.
- **Multiple triggers in one scenario.** Two `When`s = two scenarios.
- **No error or boundary scenarios.** Happy-path-only criteria let real failures pass the gate.

## Checklist

- [ ] Every `FR-n` has at least one acceptance criterion, and each criterion names the FR it
      verifies.
- [ ] Each scenario has one trigger (`When`) and an observable, assertable outcome (`Then`).
- [ ] Concrete data is used (counts, times, limits) — no "some", "later", "correctly".
- [ ] Happy path, boundary, and unwanted/error cases are all represented.
- [ ] Criteria assert behavior, not UI specifics, and are deterministic.
- [ ] Coverage is 1:1+ both ways — no orphan requirements, no orphan criteria.

## Links

- **Template:** [`templates/specification.md` §6](../templates/specification.md) — the acceptance
  criteria / scenarios section.
- **Concept:** [`docs/04 §5` — BDD / Gherkin](../docs/04-from-needs-to-spec.md#5-writing-requirements-bdd--gherkin-acceptance-scenarios)
  and [§2 on stories → criteria](../docs/04-from-needs-to-spec.md#2-capturing-intent-user-stories-and-acceptance-criteria).
- **Worked example:** [`examples/…/0001-cart-persistence/spec.md`](../examples/specs/0001-cart-persistence/spec.md) —
  scenarios mapped to FRs.
- **Upstream:** write the requirements first with
  [`write-ears-requirements.md`](write-ears-requirements.md).
