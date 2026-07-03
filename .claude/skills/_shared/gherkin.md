# Acceptance criteria — BDD / Gherkin (bundled reference)

Where EARS states a *rule*, Gherkin states an *example* of that rule in action — and the example is
directly executable as a test. The two compose: EARS for the normative requirement, Gherkin
scenarios (or a checklist) as its acceptance criteria. Self-contained reference for the spec skills.

## Form

```gherkin
Scenario: <name>                    # verifies FR-<n>
  Given <precondition>
  And   <more context>
  When  <action / trigger>
  Then  <expected, observable outcome>
```

Given/When/Then maps cleanly onto EARS' precondition/trigger/response — which is why they reinforce
each other. A plain checklist item is an acceptable lighter-weight alternative:

```
- [ ] Changing the cart persists within 500 ms (FR-1).
```

## The 1:1 rule

Acceptance criteria **map 1:1 to requirements**:
- Every `FR`/`NFR` has **at least one** acceptance criterion (Gherkin scenario or checklist line).
- Every criterion names the requirement ID it verifies (`# verifies FR-2`, or `(FR-1)` inline).
- A criterion that traces to no requirement, or a requirement with no criterion, is a defect.

## Worked example

```gherkin
Scenario: Unsaved cart is restored after a crash        # verifies FR-2
  Given a logged-in customer with 3 items in their cart
  And the cart has not been checked out
  When the browser crashes and the customer reopens the site within 24 hours
  Then the cart shall contain the same 3 items

Scenario: Cart is not restored after expiry             # verifies FR-3
  Given a logged-in customer with items in their cart
  When 24 hours pass without any cart activity
  Then the cart shall be treated as expired and shall not be restored
```

## Smell test

- **Criterion with no requirement ID**, or a requirement with no verifying criterion.
- **"Then" is not observable** — the expected outcome can't be watched or measured.
- **Testing the "how"** — the scenario asserts an implementation detail rather than the behavior.
- **Only happy-path scenarios** — no boundary or error scenario for a requirement that has one.

## Checklist

- [ ] Every `FR`/`NFR` has ≥ 1 acceptance criterion.
- [ ] Every criterion names the requirement ID it verifies.
- [ ] Each `Then` states an observable, checkable outcome.
- [ ] Boundary and error scenarios exist wherever the requirement has them.
