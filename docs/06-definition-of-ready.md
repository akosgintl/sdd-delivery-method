# 06 — Definition of Ready (DoR)

The **Definition of Ready** is the gate a piece of work must pass *before* it enters
construction. In SDD, it is fundamentally a question about the **specification**: *is the spec
clear, testable, agreed, and feasible enough that building from it is low-risk?*

> **DoR answers:** "Are we ready to build this — or would building now just encode ambiguity into
> code?"

## 1. Why DoR matters more in SDD

In ticket-driven delivery, "ready" often means "someone wrote a title and we have capacity." SDD
raises the bar: ready means the *contract exists and is sound*. This is the gate that realizes
[P7](01-principles.md#p7--two-gates-make-the-method-real-ready-and-done). Skipping it is the most
common reason SDD adoptions fail — teams produce specs but build before the specs are sound, and
conclude "the docs didn't help."

DoR is a **team agreement**, not a manager's checklist. The team owns it, applies it honestly,
and revises it in retrospectives. It is a *quality bar*, not a bureaucratic toll booth — keep it
lean enough that meeting it is normal, not heroic.

## 2. The SDD Definition of Ready

A work item is **Ready** when **all** of the following hold. (Copy-ready version:
[`templates/definition-of-ready.md`](../templates/definition-of-ready.md).)

### A. Intent is clear
- [ ] The **need / user story** it serves is stated and understood (who, what, why).
- [ ] The **business value or success metric** is articulated.
- [ ] It traces to a goal/PRD (or is explicitly standalone).

### B. The specification is sound
- [ ] A **`spec.md` exists** for the work.
- [ ] **Functional requirements** are written, each with a stable ID, **singular, unambiguous,
      and testable** (see [04 §3.2](04-from-needs-to-spec.md#32-the-qualities-of-a-good-requirement)).
- [ ] **Non-functional requirements** are specified and **quantified** where they apply.
- [ ] **Acceptance criteria** exist and map to the requirements (the verifiable definition of
      satisfaction).
- [ ] **Edge cases and error behavior** are covered, not just the happy path.
- [ ] **Goals and non-goals** are both stated (scope is bounded).
- [ ] **No open questions remain** — the "Open Questions" section is empty (clarification done).
- [ ] Banned vague terms are removed or defined; domain terms align with the **glossary**.

### C. It is feasible and bounded
- [ ] The work is **small enough** to be estimated and delivered in the team's normal unit
      (a sprint / a sensible flow item). If not, **split it** — each split gets its own spec.
- [ ] **Dependencies** are identified and either available or sequenced.
- [ ] **Technical feasibility** has been sanity-checked (a spike was run if there was real doubt).
- [ ] It **conforms to the constitution**, or an explicit, recorded exception ([ADR](03-artifacts.md#adr--architecture-decision-record)) exists.

### D. It is agreed
- [ ] The spec has been **reviewed** by engineering and the relevant stakeholder(s)/product owner.
- [ ] The team **understands** it well enough to start (no "we'll figure out what it means later").
- [ ] (If a separate design phase applies) the **technical approach is agreed** and significant
      decisions are recorded.

> A useful mnemonic: **INVEST + Testable + Agreed.** Independent, Negotiable, Valuable,
> Estimable, Small, Testable — plus reviewed and agreed by the people who must build and accept it.

## 3. Calibrating DoR to risk

Per [P8](01-principles.md#p8--specify-in-proportion-to-risk), the *depth* required to satisfy
each item scales with risk, but the *items themselves don't change*:

| Work type | How DoR applies |
|-----------|-----------------|
| High-risk / regulated feature | Full spec, quantified NFRs, reviewed design, possibly formal acceptance sign-off. |
| Standard feature | Full DoR as above, proportionate spec. |
| Small change / bug fix | Spec may collapse into a rich PR description with testable acceptance criteria; DoR still requires clear, testable intent. |
| Spike / research | DoR is a clear **learning goal and time-box**, *not* a behavior spec — the output of the spike feeds a future spec. |

The point: even a one-line fix has a "ready" bar — *is the intended behavior testable and
agreed?* — it's just cheaper to meet.

## 4. Who checks DoR, and when

- **When:** at the transition from Plan to Build — e.g. backlog refinement / "pull into sprint" /
  moving a card to the "Ready" column. It is checked *before* the team commits capacity.
- **Who:** the team collectively, with the spec author and product owner present. One person
  (often a lead) can run the checklist, but readiness is a shared judgment.
- **How (lightweight):** a checklist in the PR/issue template; for specs, a CI lint can enforce
  the mechanical items (front-matter present, no open questions, every FR has an ID) so humans
  focus on judgment items (is it *really* unambiguous and feasible?).

## 5. What happens when it's not Ready

Not-Ready is a normal, healthy outcome — it means the gate is working. Options:

- **Refine:** send it back for more specification/clarification (most common).
- **Spike:** if the blocker is unknown feasibility or unclear need, time-box a spike to learn,
  then re-spec.
- **Split:** if it's too big, divide into smaller specs that *are* ready.
- **Defer:** if the need itself is unclear, it's not ready to specify — return to discovery.

Never "start anyway and clarify in flight." That converts cheap prose ambiguity into expensive
code ambiguity — exactly what SDD exists to prevent.

## 6. DoR for AI-assisted SDD

When an AI agent will implement the work, DoR is **even more important**, because the agent will
fill ambiguity with plausible guesses rather than asking. Add:

- [ ] The spec is **self-contained** — an agent with no tribal knowledge could build from it.
- [ ] Relevant **constitution / steering rules** are in scope for the agent's context.
- [ ] Acceptance criteria are **mechanically checkable** (the agent and CI can verify them).
- [ ] Interfaces/contracts the agent must honor are **explicit** (schemas, signatures).

See [10 — AI-Assisted SDD](10-ai-assisted-sdd.md).

## 7. Relationship to Done

DoR and DoD are bookends. A sound DoR makes DoD achievable: if the acceptance criteria were
testable and agreed at Ready, then at Done you simply verify them. A weak DoR shows up as a
painful, contested DoD. **Invest at the Ready gate to make the Done gate cheap.**

> Continue to [07 — Definition of Done](07-definition-of-done.md).
