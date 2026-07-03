# EARS requirement syntax (bundled reference)

EARS (Easy Approach to Requirements Syntax) is five sentence templates that force every functional
requirement to name its trigger and its observable response. Every EARS requirement uses **shall**
(obligation) or **shall not** (prohibition). Self-contained reference for the spec skills.

> The brutal test of a requirement: **can you write a pass/fail check for it?** If not, it's a wish.
> EARS exists to make the answer "yes."

## The five patterns

| # | Pattern | Keyword | Use when the behavior is… | Shape |
|---|---------|---------|---------------------------|-------|
| 1 | Ubiquitous | *(none)* | always true, no precondition | The `<system>` shall `<response>`. |
| 2 | Event-driven | **When** | triggered by an event | When `<trigger>`, the `<system>` shall `<response>`. |
| 3 | State-driven | **While** | true only during a state | While `<state>`, the `<system>` shall `<response>`. |
| 4 | Optional-feature | **Where** | present only with an optional feature | Where `<feature>`, the `<system>` shall `<response>`. |
| 5 | Unwanted behavior | **If/Then** | handling an error/undesired case | If `<trigger>`, then the `<system>` shall `<response>`. |

**Compound** (combine only when genuinely necessary):
> While `<state>`, when `<trigger>`, the `<system>` shall `<response>` and shall not `<prohibition>`.

### Canonical examples
1. **Ubiquitous:** The payment service **shall** record every authorization attempt in the audit log.
2. **Event-driven:** **When** the user submits the checkout form, the system **shall** validate all
   required fields before contacting the payment provider.
3. **State-driven:** **While** the account is suspended, the system **shall** reject all outbound transfers.
4. **Optional-feature:** **Where** two-factor authentication is enabled, the system **shall** require
   a second factor on login from an unrecognized device.
5. **Unwanted:** **If** the payment provider does not respond within 5 seconds, **then** the system
   **shall** cancel the attempt and show a retry prompt without charging the customer.

## Recipe (per requirement)

1. Name the single behavior. One behavior → one requirement.
2. Pick the pattern (ask in order: always true? event? state? optional feature? unwanted?).
3. Nail the trigger/precondition — this is where ambiguity dies. "When the user submits the form"
   beats "when the user is done".
4. Make the response observable/measurable — "shall persist within 200 ms", not "shall handle".
5. Keep it singular — if you wrote "and" joining two behaviors, split into two requirements.
6. Cover the trio for every behavior: happy path, boundary, **and** the unwanted (If/Then) case.
7. Assign a stable `FR-<n>` ID and write the pass/fail check in your head.

## Smell test — rewrite if you see…

- **No trigger** — a When/While/If line whose precondition is fuzzy ("when appropriate").
- **Unobservable response** — "shall handle / support / manage / process".
- **"and" / "and/or" / "etc."** — compound or open-ended; split or close it.
- **A quality, not a behavior** — "shall be fast/secure/scalable" is an NFR; quantify and move it.
- **No error case** — only happy-path FRs = half-specified. Add the If/Then lines.
- **"should" / "may" / "could"** — negotiable words; requirements use **shall / shall not**.

## Checklist

- [ ] Each requirement uses one of the five patterns with the correct keyword.
- [ ] Every requirement uses **shall** / **shall not** (no should/may/could).
- [ ] The trigger/precondition is specific and unambiguous.
- [ ] The response is observable and, where relevant, quantified (numbers, units, limits).
- [ ] Each statement is singular — no "and"-joined behaviors.
- [ ] Happy path, boundaries, and unwanted/error conditions are all covered.
- [ ] No banned vague words (see `banned-words.md`) in any normative line.
- [ ] Each requirement has a stable `FR-<n>` ID and a writable pass/fail check.
