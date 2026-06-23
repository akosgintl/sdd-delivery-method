# 04 — From User Needs to Specification

This is the practical heart of the method: the craft of moving from a fuzzy human need to a
precise, testable specification. It covers elicitation, requirements analysis, the qualities of a
good requirement, writing styles (**EARS** and **BDD/Gherkin**), and a worked end-to-end example.

```
  NEED                  REQUIREMENT               SPECIFICATION
  (problem, in          (what must be true,       (precise, testable
   user's words)         analyzed)                 behavior + criteria)
  ───────────────  ──►  ──────────────────  ──►   ──────────────────────
  §1 Elicit             §3 Analyze & qualify       §4 Write (EARS/Gherkin)
  §2 Capture as         §3 Functional vs            §5 Assemble the spec
     stories/PRD            non-functional          §6 Worked example
```

## 1. Eliciting user needs

The goal of elicitation is to understand the **problem** before anyone proposes a **solution**.
The most common origin of a bad spec is a need that was never actually understood.

**Techniques** (pick by context):
- **Interviews** — open questions about goals, pains, and current workarounds.
- **Observation / contextual inquiry** — watch real usage; people's described behavior and
  actual behavior differ.
- **Jobs-to-be-Done** — "When `<situation>`, I want to `<motivation>`, so I can `<expected
  outcome>`." Excellent at separating need from solution.
- **Job tickets / support data / analytics** — evidence of real pain.
- **Workshops / event storming** — for cross-functional domains.

**Anti-patterns in elicitation:**
- *Solution-first.* "We need a dashboard" is a solution; the need is "I can't tell when a job is
  stuck." Always ask "what would that let you do?" until you reach a problem.
- *The loudest stakeholder.* Triangulate across users; weight by evidence, not volume.
- *Assuming the obvious.* Write assumptions down so they can be challenged.

**Output of Phase 0:** a **problem statement** (and, for larger work, a Vision/PRD) that states
*who*, *what problem*, *why it matters*, *how we'll measure success*, and *what's out of scope*.

## 2. Capturing intent: user stories and acceptance criteria

User stories carry the need into the backlog in user-centered language:

> **As a** logged-in customer, **I want** my shopping cart to survive a browser crash **so that**
> I don't lose items I was about to buy.

A story is **not** a specification — it is a placeholder for a conversation and a pointer to the
spec that will formalize it. Each story gets **acceptance criteria**: the conditions that, when
met, mean the story is satisfied. Acceptance criteria are the seed of the specification's
testable requirements.

> **Story → Spec rule:** every story must resolve to one or more *testable* requirements in a
> spec. If a story can't be made testable, it isn't ready (see [DoR](06-definition-of-ready.md)).

## 3. From requirements to *good* requirements

A **requirement** is a single statement of something the system must do or a quality it must
have. Analysis turns raw desires into requirements that are fit to specify.

### 3.1 Functional vs non-functional

- **Functional requirements (FRs)** — *what the system does*: behaviors, rules, inputs/outputs,
  state changes. "When a draft is unsaved for 2 seconds, the system shall persist it locally."
- **Non-functional requirements (NFRs)** — *qualities and constraints*: performance, security,
  availability, accessibility, privacy, compliance, usability, observability, cost.
  NFRs are routinely under-specified and are a frequent source of production failure. Specify
  them with numbers: "p95 latency < 300 ms at 1000 RPS," not "fast."

A useful NFR checklist (the "-ilities"): performance, scalability, availability/reliability,
security, privacy, accessibility (e.g. WCAG 2.2 AA), usability, observability, maintainability,
portability, compliance, localization, cost.

### 3.2 The qualities of a good requirement

Drawn from ISO/IEC/IEEE 29148 (the modern successor to IEEE 830), each requirement should be:

| Quality | Meaning | Smell to avoid |
|---------|---------|----------------|
| **Necessary** | Traces to a real need | "nice to have" with no source |
| **Unambiguous** | One interpretation only | "etc.", "and/or", "as appropriate" |
| **Testable / verifiable** | You can prove it true or false | "user-friendly", "fast", "robust" |
| **Consistent** | Doesn't contradict another req | conflicting rules across specs |
| **Complete** | No "TBD"; edges covered | missing error/empty/boundary cases |
| **Feasible** | Achievable within constraints | wishes that ignore physics/budget |
| **Singular** | One requirement per statement | "and" joining two behaviors |
| **Bounded** | Scoped; states non-goals | scope creep by omission |
| **Traceable** | Has a stable ID, links up/down | orphan requirements |

The **single most important** of these is **testability** ([P3](01-principles.md#p3--every-requirement-must-be-testable)).
A quick test for testability: *can you write a pass/fail check for this sentence?* If not, rewrite
it until you can.

### 3.3 Words to ban (or define)
"fast, slow, easy, intuitive, robust, secure, scalable, flexible, support, handle, manage,
appropriate, etc., and/or, optimize, user-friendly." Each is either replaced by a measurable
statement or given a precise definition in the glossary.

## 4. Writing requirements: EARS

**EARS** (Easy Approach to Requirements Syntax) is a lightweight set of sentence templates that
make natural-language requirements precise and consistent without the cost of formal methods. It
was developed at Rolls-Royce for aero-engine control software and is now widely used in
spec-driven workflows (Amazon Kiro generates requirements in EARS by default).

Every EARS requirement uses the keyword **shall** for the system's obligation, and one of five
patterns:

### 4.1 The five EARS patterns

**1. Ubiquitous** — always true, no precondition.
> *The `<system>` shall `<response>`.*
> The payment service **shall** record every authorization attempt in the audit log.

**2. Event-driven** — triggered by an event (keyword **When**).
> *When `<trigger>`, the `<system>` shall `<response>`.*
> **When** the user submits the checkout form, the system **shall** validate all required fields
> before contacting the payment provider.

**3. State-driven** — true while in a state (keyword **While**).
> *While `<state>`, the `<system>` shall `<response>`.*
> **While** the account is suspended, the system **shall** reject all outbound transfers.

**4. Optional-feature** — applies only if a feature is present (keyword **Where**).
> *Where `<feature is included>`, the `<system>` shall `<response>`.*
> **Where** two-factor authentication is enabled, the system **shall** require a second factor on
> login from an unrecognized device.

**5. Unwanted behavior** — handling errors/undesired conditions (keywords **If/Then**).
> *If `<unwanted trigger>`, then the `<system>` shall `<response>`.*
> **If** the payment provider does not respond within 5 seconds, **then** the system **shall**
> cancel the attempt and show a retry prompt without charging the customer.

**Complex / compound** — combine the above when genuinely necessary:
> *While `<state>`, when `<trigger>`, the `<system>` shall `<response>`.*
> **While** the store is in maintenance mode, **when** a customer attempts checkout, the system
> **shall** display the maintenance notice and **shall not** create an order.

### 4.2 Why EARS works
- It forces you to name the **trigger/precondition**, which is where ambiguity hides.
- The unwanted-behavior pattern makes you specify **error and edge cases** explicitly — usually
  the most valuable and most-skipped content.
- It is readable by non-engineers yet precise enough to translate directly into tests.
- "shall / shall not" cleanly separates obligation from prohibition.

### 4.3 EARS tips
- One requirement per statement (singular). Split compound sentences.
- Make responses observable. "shall persist the draft to local storage within 200 ms" beats
  "shall handle the draft."
- Cover the trio for every behavior: the happy path, the boundary, and the unwanted condition.

## 5. Writing requirements: BDD / Gherkin (acceptance scenarios)

Where EARS states a rule, **Gherkin** states an *example* of the rule in action — and that
example is directly executable as a test. The two compose: EARS for the normative requirement,
Gherkin scenarios as its acceptance tests.

```gherkin
Feature: Cart survives a browser crash

  Scenario: Unsaved cart is restored after a crash
    Given a logged-in customer with 3 items in their cart
    And the cart has not yet been checked out
    When the browser crashes and the customer reopens the site within 24 hours
    Then the cart shall contain the same 3 items

  Scenario: Cart is not restored after expiry
    Given a logged-in customer with items in their cart
    When 24 hours pass without activity
    Then the cart shall be cleared
```

Gherkin's **Given/When/Then** maps cleanly onto EARS' precondition/trigger/response, which is why
the two reinforce each other. Use Gherkin when concrete examples aid understanding or when you
want acceptance criteria to *be* the automated tests (Cucumber, SpecFlow, Behave, etc.).

## 6. Assembling the specification

A specification is more than a pile of requirements. The recommended structure (mirrored in
[`templates/specification.md`](../templates/specification.md)):

1. **Header / metadata** — ID, title, status, owner, links to need/story, version.
2. **Summary & context** — the problem in one paragraph; why now.
3. **Goals & non-goals** — what this does and explicitly does *not* do.
4. **Functional requirements** — EARS statements with stable IDs (`FR-1`, `FR-2`, …).
5. **Non-functional requirements** — measurable, with IDs (`NFR-1`, …).
6. **Acceptance criteria / scenarios** — Gherkin or a checklist; the verifiable definition of
   satisfaction.
7. **Edge cases & error handling** — the boundary and unwanted-behavior cases.
8. **Data & interfaces** — schemas/contracts touched (or links to them).
9. **Dependencies & assumptions.**
10. **Open questions** — *must be empty before the spec is Ready.*
11. **Rationale / decisions** — the *why* (or links to ADRs).

### Specification quality checklist (apply before review)
- [ ] Every requirement has a stable ID and is singular, unambiguous, and testable.
- [ ] Goals and **non-goals** are both stated.
- [ ] Happy path, boundaries, and error behavior are all covered.
- [ ] NFRs are quantified.
- [ ] Banned vague words are gone or defined.
- [ ] Acceptance criteria map 1:1 to requirements.
- [ ] Open questions are resolved (or the spec is explicitly marked *draft*).
- [ ] It traces up to a need and down to tests.

## 7. Worked example: needs → spec (excerpt)

**Need (Phase 0).** "Customers complain they lose their cart when their browser crashes during a
sale, and abandon the purchase." Impact: measurable cart-abandonment after crashes; success
metric: reduce crash-related abandonment by 50%.

**Story (Phase 0).**
> As a logged-in customer, I want my cart to survive a browser crash so that I don't lose items.

**Requirements (Phase 1, EARS).**
- **FR-1 (event-driven):** *When* an item is added to or removed from the cart, the system
  *shall* persist the updated cart to durable per-user storage within 500 ms.
- **FR-2 (event-driven):** *When* a logged-in customer with a persisted cart opens the site, the
  system *shall* restore the most recently persisted cart.
- **FR-3 (state-driven):** *While* a cart is older than 24 hours without activity, the system
  *shall* treat it as expired and *shall not* restore it.
- **FR-4 (unwanted):** *If* persisting the cart fails, *then* the system *shall* retry up to 3
  times and *shall* surface a non-blocking warning if all retries fail.
- **NFR-1:** Cart restoration *shall* complete within 1 s at p95 for carts of up to 100 items.
- **Non-goal:** synchronizing carts across *different* devices is out of scope for this feature.

**Acceptance scenarios (Phase 1, Gherkin).** See [§5](#5-writing-requirements--bdd--gherkin-acceptance-scenarios)
above — those two scenarios verify FR-2 and FR-3.

The full populated bundle for this kind of example lives in [`examples/`](../examples/).

## 8. Common failure modes (and the fix)

| Failure mode | Fix |
|--------------|-----|
| Spec describes *how* (implementation) | Re-state as observable *what*; move how to `design.md`. |
| Requirements not testable | Apply the §3.2 testability test; rewrite until pass/fail. |
| Only the happy path is specified | Use the EARS unwanted-behavior pattern for every behavior. |
| Vague NFRs | Quantify; attach numbers and conditions. |
| Scope ambiguity | Add an explicit non-goals section. |
| Open questions left in | Block Ready until resolved; record answers as rationale/ADRs. |
| Story treated as the spec | Stories point to specs; formalize the behavior. |

> Continue to [05 — Storage & Organization](05-storage-and-organization.md).
