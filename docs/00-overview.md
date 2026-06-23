# 00 — Overview: What SDD Is, Why It Exists, When to Use It

## 1. Definition

**Specification-Driven Development (SDD)** is a delivery method in which an explicit, reviewed,
version-controlled **specification** is the authoritative description of *what* the system must
do, and all construction is *derived from and verified against* that specification.

The defining commitment is sequencing and authority:

> **Specify before you build, and let the specification be the source of truth.**

This is a deliberately strong claim. It does *not* mean "write a giant document up front and
freeze it" (that failure mode is discussed in [11 — Anti-Patterns](11-adoption-and-antipatterns.md)).
It means that for each unit of work, a precise statement of intended behavior is authored,
agreed, and recorded *before* the bulk of the code is written — and that statement remains
accurate as the code evolves.

## 2. The problem SDD solves

Most delivery pain is traceable to **ambiguity that is discovered late**:

- Requirements live in someone's head, a chat thread, or a ticket title.
- "Done" means "the code I wrote runs" rather than "the agreed behavior is satisfied."
- Tests assert what the code *does*, not what it was *supposed* to do.
- When behavior is questioned six months later, no one can point to the intended contract.
- AI coding agents produce plausible code from vague prompts, then drift from intent.

SDD attacks this by **moving the moment of disambiguation earlier and making it durable**. The
expensive ambiguities — what should happen at the boundaries, what "valid" means, what the
system must *not* do — are resolved while they are cheap to resolve (in prose and review)
rather than expensive (in production incidents and rework).

```
Cost of resolving an ambiguity, by phase it's caught:

  spec review   │▓
  design        │▓▓▓
  code review   │▓▓▓▓▓▓▓
  QA            │▓▓▓▓▓▓▓▓▓▓▓▓
  production    │▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓
                └────────────────────────────────────►
SDD shifts disambiguation left, into the cheapest column.
```

## 3. What makes SDD distinct

SDD is often confused with adjacent practices. The distinction is about **what is authoritative**:

| Practice | Authoritative artifact | SDD's relationship |
|----------|------------------------|--------------------|
| Waterfall "big design up front" | A large up-front document, frozen | SDD specs are **per-feature, living, and small**; updated as understanding changes. |
| Ticket-driven / "just-in-time" agile | The ticket + conversation | SDD elevates the spec above the ticket; the ticket *points to* the spec. |
| Test-Driven Development (TDD) | The test | In SDD, tests are **derived from** the spec; the spec explains *why* the test asserts what it does. SDD and TDD compose well. |
| Behavior-Driven Development (BDD) | Gherkin scenarios | BDD scenarios are an excellent **specification format** for behavior; SDD subsumes BDD as one way to write the spec. |
| Design-by-Contract | Pre/postconditions in code | A code-level expression of the same idea; SDD's spec is the human-level contract that contracts-in-code implement. |
| API-first / Contract-first | OpenAPI / schema | The interface specification; SDD includes this but also covers behavior, NFRs, and rationale. |
| Documentation-driven dev | The docs/README | Close cousin; SDD is stricter about testability and traceability. |

The throughline: **SDD names the specification as the contract and insists it precede and
outlive the code.**

## 4. Core benefits

- **Less rework.** Ambiguity is resolved before it is encoded into code and tests.
- **Auditable intent.** Anyone can answer "what is this *supposed* to do, and why?" from the repo.
- **Parallelizable.** A clear spec lets design, test authoring, and stubbing proceed in parallel.
- **Reviewable in the cheap medium.** Disagreements surface in prose review, not in PRs.
- **AI-ready.** A precise spec is the highest-leverage prompt for a coding agent and the
  yardstick for judging its output. See [10 — AI-Assisted SDD](10-ai-assisted-sdd.md).
- **Onboarding & continuity.** New people (and new agents) read the spec, not the tribal lore.

## 5. Costs and honest trade-offs

SDD is not free. Be clear-eyed:

- **Up-front time.** Specifying costs effort before any visible product exists. The payoff is
  later and statistical, which makes it politically easy to cut.
- **Discipline tax.** A spec that drifts out of date is worse than no spec — it lies with
  authority. Keeping specs accurate requires the [Definition of Done](07-definition-of-done.md)
  to enforce it.
- **Over-specification risk.** Specifying things that don't need specifying (or specifying
  *how* instead of *what*) wastes effort and ossifies design. Calibrate depth to risk (§7).
- **Skill dependency.** Writing good, testable specifications is a learnable skill that not
  everyone has yet. Templates and review help close the gap.

## 6. When SDD pays off most

SDD's return scales with **ambiguity cost** and **change cost**. Favor it heavily when:

- The domain is complex or regulated (finance, health, safety, privacy).
- Multiple teams/systems integrate against the same behavior (contracts matter).
- The cost of a wrong behavior in production is high.
- AI agents are writing significant portions of the code.
- The work will be maintained for years by changing hands.

Use a **lightweight** variant (or skip formal specs) when:

- The work is a true throwaway spike or prototype to *learn*, not to keep.
- The change is trivial and self-evident (a copy fix, a dependency bump).
- The domain is genuinely understood and stable and the blast radius is tiny.

> **Rule of thumb:** *Specify in proportion to the cost of being wrong.* A payment-authorization
> rule deserves a precise spec; renaming a button does not.

## 7. Calibrating specification depth (the "right altitude")

A specification describes **what** and **why**, not **how**. The most common mistake is writing
implementation in the spec. Keep these on the correct side of the line:

| Belongs in the spec (WHAT/WHY) | Belongs in design/code (HOW) |
|--------------------------------|------------------------------|
| Observable behavior and outcomes | Algorithms, data structures |
| Inputs, outputs, and their valid ranges | Class/function decomposition |
| Business rules and invariants | Library and framework choices |
| Error and edge-case behavior | Internal module boundaries |
| Non-functional requirements (perf, security) | Specific configs to hit them |
| Acceptance criteria | Test implementation details |

The boundary moves with audience: an *API specification* legitimately includes the interface
shape (that *is* the what), while a *feature behavior specification* should stay implementation-free.

## 8. How the rest of this study is organized

- The **principles** ([01](01-principles.md)) ground the method and introduce the *constitution*.
- The **lifecycle** ([02](02-lifecycle.md)) defines the phases and gates from need to done.
- The **artifacts** ([03](03-artifacts.md)) catalog every document the method produces.
- The **needs-to-spec** chapter ([04](04-from-needs-to-spec.md)) is the practical heart:
  elicitation, requirements, and writing testable specifications (including EARS).
- **Storage** ([05](05-storage-and-organization.md)) defines how artifacts live in the repo.
- **DoR** ([06](06-definition-of-ready.md)) and **DoD** ([07](07-definition-of-done.md)) are the
  two gates that make the method real rather than aspirational.
- The remainder covers **roles**, **traceability**, **AI tooling**, and **adoption**.

> Continue to [01 — Principles](01-principles.md).
