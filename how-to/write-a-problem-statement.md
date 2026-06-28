# How to write a good problem statement (and user stories)

A problem statement is the **why** the whole feature bundle hangs from: a crisp description of a
real problem, stated *before* anyone has picked a solution. Get it wrong and everything downstream
is precise about the wrong thing — a perfectly testable spec for a feature nobody needed. Its job is
to keep you honest about the problem long enough to be sure you're solving it, and to hand the spec
a measurable definition of success. This is the *doing* companion to
[`docs/04 §1–2`](../docs/04-from-needs-to-spec.md#1-eliciting-user-needs).

> A problem statement describes a **problem, not a solution**. If it names a screen, a button, a
> table, or a technology, you've skipped a step — ask "what would that let someone do?" until you're
> back at the pain.

## When you write it

**Phase 0 (Discover)**, at the very start, owned by product / business analysis. It's a snapshot:
written once to frame the work, then it stops changing — the living detail moves into the
[PRD](write-a-prd.md) and the [spec](write-a-spec.md). For anything larger than one feature, the
problem statement is the opening section of a [PRD](write-a-prd.md); for a single feature it can be a
few lines in the spec's "Summary & context."

## Anatomy: what goes in (and what doesn't)

| Part | Answers | Smell if missing |
|------|---------|------------------|
| Who has the problem | which user/segment, in their words | a solution looking for a victim |
| What the problem is | the pain, independent of any fix | "we need a dashboard" (that's a fix) |
| Why it matters / impact | the cost of *not* solving it | a problem nobody will pay to solve |
| Success metric | the number that says we're done | "users are happier" (unmeasurable) |
| Constraints & assumptions | the boundaries; stated guesses | hidden assumptions that bite later |
| Out of scope | what this is *not* about | scope creep by silence |

A **user story** is the same need carried into the backlog in user-centered language —
*"As a `<role>`, I want `<capability>` so that `<benefit>`."* It is **not** a spec: it's a
placeholder for a conversation and a pointer to the spec that will formalize it. Every story must
resolve to one or more *testable* requirements, or it isn't ready.

## The recipe

1. **Start from evidence, not a request.** Support tickets, analytics, an interview quote, an
   observed workaround. A problem with no evidence is a preference.
2. **Strip the solution back out.** When a stakeholder hands you "we need X", ask *"what would X let
   you do?"* repeatedly until you reach a problem statement that mentions no solution at all. Use
   Jobs-to-be-Done — *"When `<situation>`, I want to `<motivation>`, so I can `<outcome>`"* — it
   separates need from solution by construction.
3. **Name who, specifically.** "Users" is too broad to verify; "logged-in customers mid-checkout"
   is checkable. Triangulate across users — weight by evidence, not by who shouted loudest.
4. **Quantify the impact and the success metric.** Replace "people lose work" with "X% of sessions
   end in lost carts after a crash; target: cut that by half." The metric becomes the spec's goal.
5. **Write assumptions down so they can be challenged.** Every "we assume…" is a thing that can be
   wrong; on paper it's cheap to kill, in code it's expensive.
6. **Bound it.** State what's explicitly out of scope so the spec inherits clean edges.
7. **Turn it into one or two stories**, each with a benefit clause, each pointing forward to the
   spec that will make it testable.

## Before → After

**Solution-first request → problem stated as a problem**

> ✗ *Before:* "We need a cart dashboard so users can manage their carts."
> *(Names a solution — a dashboard — and a vague verb, "manage". Nobody can tell what problem it
> solves or how we'd know it worked.)*
>
> ✓ *After:* "**Who:** logged-in customers shopping during a sale. **Problem:** when their browser
> crashes mid-session they lose their cart and abandon the purchase. **Impact:** ~8% of sale-day
> sessions hit this; crash-related abandonment is measurable. **Success:** reduce crash-related
> abandonment by 50%. **Out of scope:** syncing carts across different devices."

**Storyless story → testable user story**

> ✗ *Before:* "Cart persistence." *(A title, not a story — no role, no value, nothing to verify.)*
>
> ✓ *After:* "**As a** logged-in customer, **I want** my cart to survive a browser crash **so that**
> I don't lose items I was about to buy." → resolves to `FR-1`, `FR-2` in
> [`0001/spec.md`](../examples/specs/0001-cart-persistence/spec.md).

## Smell test — rewrite or remove if you see…

- **A solution wearing a problem's clothes.** Any noun that's a feature, screen, or technology.
- **No named who.** "Users", "people", "the business" — too vague to validate against.
- **No metric.** If you can't state how you'll measure success, you can't tell when you're done.
- **Unstated assumptions.** The riskiest ones are the ones you didn't notice you were making.
- **A story with no "so that".** Without the benefit clause it's a task, not a need.
- **A story treated as the spec.** Stories point *to* specs; they don't replace them.

## Checklist

- [ ] States a problem, not a solution (no screens, tables, or tech named).
- [ ] Names *who* has the problem, specifically.
- [ ] States the impact / cost of not solving it, with evidence.
- [ ] Has a quantified success metric the spec can adopt as a goal.
- [ ] Assumptions and constraints are written down.
- [ ] Out-of-scope is explicit.
- [ ] Each user story has a role + capability + benefit and points to a spec that will make it
      testable.

## Links

- **Template:** [`templates/vision-prd.md`](../templates/vision-prd.md) (problem statement is §1; for
  a single feature it folds into [`templates/specification.md`](../templates/specification.md) §1).
- **Concept:** [`docs/04 §1` — Eliciting user needs](../docs/04-from-needs-to-spec.md#1-eliciting-user-needs)
  and [`§2` — Capturing intent](../docs/04-from-needs-to-spec.md#2-capturing-intent-user-stories-and-acceptance-criteria);
  artifact definitions in [`docs/03`](../docs/03-artifacts.md#problem--need-statement).
- **Next:** scale it up into a [PRD](write-a-prd.md), or straight into a [spec](write-a-spec.md);
  every story becomes [EARS requirements](write-ears-requirements.md) with
  [acceptance criteria](write-acceptance-criteria.md).
