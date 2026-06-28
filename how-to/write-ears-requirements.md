# How to write good EARS requirements

A requirement is the atom of a spec, and EARS is how you stop that atom from being mush. **EARS**
(Easy Approach to Requirements Syntax) is five sentence templates that force every requirement to
name its trigger and its observable response — which is exactly where vague requirements hide their
ambiguity. This guide is the *doing* companion to the theory in
[`docs/04 §4`](../docs/04-from-needs-to-spec.md#4-writing-requirements-ears); read that for *why*
EARS works, read this when you're writing the lines.

> The test of a requirement is brutal and simple: **can you write a pass/fail check for it?** If
> not, it's a wish, not a requirement. EARS exists to make the answer "yes."

## When you write it

In the **specify** phase, turning analyzed needs into the functional requirements of a `spec.md`
(section 4). Owned by whoever authors the spec — analyst, PM, or engineer. Each requirement gets a
stable ID (`FR-1`, `FR-2`, …) that never gets renumbered. See the
[lifecycle](../docs/02-lifecycle.md) for where this sits.

## Anatomy of an EARS requirement

Every EARS line is: **[keyword] [precondition/trigger], the [system] shall [observable response]**.
The keyword picks the pattern; the trigger is the part people forget; the response must be something
you could watch happen.

| Pattern | Keyword | Use when the behavior is… | Shape |
|---------|---------|---------------------------|-------|
| Ubiquitous | *(none)* | always true, no precondition | The `<system>` shall `<response>`. |
| Event-driven | **When** | triggered by an event | When `<trigger>`, the `<system>` shall `<response>`. |
| State-driven | **While** | true only during a state | While `<state>`, the `<system>` shall `<response>`. |
| Optional-feature | **Where** | present only if a feature is | Where `<feature>`, the `<system>` shall `<response>`. |
| Unwanted behavior | **If/Then** | handling an error/undesired case | If `<trigger>`, then the `<system>` shall `<response>`. |

Always use **shall** (obligation) or **shall not** (prohibition). For the canonical worked example
of each pattern, see [`docs/04 §4.1`](../docs/04-from-needs-to-spec.md#41-the-five-ears-patterns).

## The recipe

1. **Start from a need or story.** Name the single behavior you're specifying. One behavior → one
   requirement.
2. **Pick the pattern.** Ask, in order: Is it *always* true? → ubiquitous. Kicked off by an
   *event*? → When. Only true *during a state*? → While. Only present *with an optional feature*? →
   Where. Handling something *unwanted*? → If/Then.
3. **Nail the trigger/precondition.** This is where ambiguity dies. "When the user submits the
   form" beats "When the user is done." Be specific about *what* and *when*.
4. **Make the response observable and measurable.** You must be able to watch it or measure it:
   "shall persist the draft to local storage within 200 ms," not "shall handle the draft."
5. **Keep it singular.** One obligation per statement. If you wrote "and," you probably have two
   requirements — split them.
6. **Cover the trio.** For every behavior, write the happy path, the boundary, *and* the unwanted
   condition (If/Then). The error case is the most valuable and most-skipped line.
7. **Assign a stable ID and test it.** Give it `FR-n`, then apply the brutal test: write the
   pass/fail check in your head. If you can't, rewrite until you can.

## Before → After

**Vague desire → event-driven requirement**

> ✗ *Before:* "Users can save their work."
> *(Who? When? What does "save" guarantee? Untestable.)*
>
> ✓ *After:* **FR-1 (event-driven):** When a logged-in user edits a draft and then stops typing for
> 2 seconds, the system **shall** persist the draft to durable storage within 500 ms.

**Hand-wave → unwanted-behavior requirement**

> ✗ *Before:* "The system should handle payment errors gracefully."
> *("Gracefully" and "handle" are banned — see [§3.3](../docs/04-from-needs-to-spec.md#33-words-to-ban-or-define).)*
>
> ✓ *After:* **FR-7 (unwanted behavior):** If the payment provider does not respond within 5
> seconds, then the system **shall** cancel the attempt and display a retry prompt **without**
> charging the customer.

**Compound sentence → two singular requirements**

> ✗ *Before:* "When the cart is updated the system shall save it and sync it to other devices."
> *(Two behaviors welded with "and" — you can't pass/fail them independently.)*
>
> ✓ *After:*
> **FR-2 (event-driven):** When an item is added to or removed from the cart, the system **shall**
> persist the updated cart within 500 ms.
> **FR-3 (event-driven):** When a cart is persisted, the system **shall** sync it to the user's
> other active sessions within 2 s.

## Smell test — rewrite if you see…

- **No trigger.** A "When/While/If" requirement whose precondition is fuzzy ("when appropriate").
- **Unobservable response.** "shall handle / support / manage / process" — replace with the
  concrete thing that happens.
- **"and" / "and/or" / "etc."** — compound or open-ended. Split it or close it.
- **A quality, not a behavior.** "shall be fast/secure/scalable" is a non-functional requirement —
  quantify it and move it to the NFR section (*write-nfrs.md*, planned).
- **No error case.** A feature with only happy-path FRs is half-specified. Add the If/Then lines.
- **"should" instead of "shall."** "Should" is negotiable; requirements aren't. Use shall/shall not.

## Checklist

- [ ] Each requirement uses one of the five EARS patterns with the correct keyword.
- [ ] Every requirement uses **shall** / **shall not** (no "should", "may", "could").
- [ ] The trigger/precondition is specific and unambiguous.
- [ ] The response is observable and, where relevant, quantified (numbers, units, limits).
- [ ] Each statement is singular — no "and"-joined behaviors.
- [ ] The happy path, boundaries, and unwanted/error conditions are all covered.
- [ ] No [banned vague words](../docs/04-from-needs-to-spec.md#33-words-to-ban-or-define) in any
      normative line.
- [ ] Each requirement has a stable `FR-n` ID and you can write a pass/fail check for it.

## Links

- **Template:** [`templates/specification.md` §4](../templates/specification.md) — where FRs live.
- **Concept:** [`docs/04 §4` — Writing requirements: EARS](../docs/04-from-needs-to-spec.md#4-writing-requirements-ears)
  and the [tips in §4.3](../docs/04-from-needs-to-spec.md#43-ears-tips).
- **Worked example:** [`examples/…/0001-cart-persistence/spec.md`](../examples/specs/0001-cart-persistence/spec.md) —
  real FRs, each tagged with its pattern.
- **Next:** turn each FR into checks with
  [`write-acceptance-criteria.md`](write-acceptance-criteria.md).
