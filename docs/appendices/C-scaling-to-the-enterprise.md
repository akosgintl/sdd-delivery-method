# C — Scaling SDD to the Enterprise

> *Applied annex.* The study chapters ([00–11](../00-overview.md)) define the method at the level of a
> single team; the appendices ([A](A-glossary.md), [B](B-property-graph.md)) are reference.
> This chapter applies the method to a **large, multi-squad organization where AI coding agents do
> much of the construction** — what team to stand up, who owns what, how to govern across teams, and
> how to roll it out. It **synthesizes** [08](../08-roles-and-workflow.md), [10](../10-ai-assisted-sdd.md),
> [11](../11-adoption-and-antipatterns.md), and [05](../05-storage-and-organization.md) rather than
> restating them; follow the links for the underlying detail.

## 1. Why scaling needs a different cut

At one team, the question is *which responsibilities are owned* ([08 §1](../08-roles-and-workflow.md#1-responsibilities-not-necessarily-separate-people)).
Across many squads it becomes a different question: **team topology and governance**. Job titles do
not scale; *contracts and gates* do. Two failure modes appear only at scale and the design below
targets both:

- **Divergence** — every squad invents its own spec shape, its own "definition of ready", its own
  agent prompts. Quality becomes a lottery.
- **Bottlenecking** — one architecture board reviews every spec, and the upstream that was supposed
  to *remove* friction becomes the friction.

The cut that solves both: a thin **shared standing layer** (constitution + paved road) that is
*authored centrally and consumed everywhere*, over **autonomous squads** that own their features
end-to-end. The method's existing artifacts are exactly the interface between the two.

> Rule of thumb: **standardize the contract and the gates; decentralize the work.** If a central
> team is reviewing the *content* of every spec, you have rebuilt the bottleneck you were paid to
> remove.

## 2. Persona → responsibility map

Map your enterprise personas onto the SDD responsibilities from
[08 §1](../08-roles-and-workflow.md#1-responsibilities-not-necessarily-separate-people). With agents
central, the human centre of gravity moves to the **two ends** — **Specify** and **Verify** — while
agents take the mechanical middle ([08 §5](../08-roles-and-workflow.md#5-workflow-with-ai-agents-in-the-loop),
[10 §1](../10-ai-assisted-sdd.md#1-why-sdd-and-ai-agents-fit)).

| Persona | Primary SDD responsibility | What changes when agents build |
|---------|----------------------------|--------------------------------|
| Product Manager | **Need owner** — *Accountable* for the spec (the business contract) | Owns problem/PRD; signs the DoD acceptance |
| Business Analyst | **Spec author** — *Responsible* for the testable spec | The highest-leverage human seat: the spec is the prompt-of-record |
| Architect / Tech Lead | **Designer** (design + ADRs); shared **Constitution keeper** | Encodes constraints agents must honour; reviews approach, not keystrokes |
| Engineers | **Verifier + agent supervisor** | Less hand-coding; more reviewing agent output *against the spec*, owning reconcile |
| QA / Test Engineer | **Verifier**; authors executable acceptance tests | Spec→test-first, so tests encode the contract, not whatever the code does |
| UX, Security, SRE | **Consulted** at spec/design review | Quantify the NFRs they own ([09](../09-quality-and-traceability.md)); no vague "secure/fast" |
| AI coding agents | **Implementer** (plan → code → tests) | Fed spec + design + tasks + constitution; **never** trusted unverified |
| Platform / enabling team | Owns the **paved road** (templates, CI, harness) | Makes every squad's agents obey the same standing law |
| Delivery lead / EM | Owns the **two gates** (DoR / DoD enforcement) | Protects upstream time; treats prevented rework as a win |

> The SDD-specific point survives scaling intact: **Product is Accountable for the spec; the spec
> author is Responsible; engineering and QA are Consulted so the contract is feasible and testable.**
> Agents are *Responsible* implementers but **never Accountable** — a human always owns the gate.

## 3. Team topology

Borrow the Team Topologies vocabulary; it lines up cleanly with the standing-layer / squad split.

```
        ┌─────────────────────────────────────────────────────────────┐
        │  SPEC-REVIEW / ARCHITECTURE GUILD  (cross-cutting, part-time) │
        │  keeps the shared constitution coherent · high-risk reviews   │
        └───────────────▲───────────────────────────▲──────────────────┘
                        │                            │
   ┌────────────────────┴───────┐    ┌───────────────┴────────────────┐
   │  STREAM-ALIGNED SQUAD  A   │    │  STREAM-ALIGNED SQUAD  B   ...  │
   │  need owner · spec author  │    │  need owner · spec author       │
   │  designer · verifier       │    │  designer · verifier            │
   │  + supervised AI agents    │    │  + supervised AI agents         │
   └────────────────────▲───────┘    └───────────────▲────────────────┘
                        │     consumes the paved road │
        ┌───────────────┴────────────────────────────┴────────────────┐
        │  PLATFORM / ENABLING TEAM   (the multiplier)                 │
        │  templates · shared constitution · CI checks · agent harness │
        │  · specs/ conventions · spec-lint · traceability coverage    │
        └──────────────────────────────────────────────────────────────┘
```

**Stream-aligned squads (≈5–9 people).** Own features end-to-end: Discover → Specify → Spec review →
supervise agents through Build → Verify → Accept. The **minimum viable seating** is four
responsibilities present (one person may hold two): a **need owner**, a **spec author**, a
**designer**, and a **verifier**. The squad owns its features' specs in *its* repo
([05](../05-storage-and-organization.md)).

**Platform / enabling team (the multiplier).** Owns the **paved road** so squads don't each reinvent
it: the artifact [`templates/`](../../templates/), the **shared constitution**, the CI checks
(requirement-coverage and spec-lint, [09](../09-quality-and-traceability.md)), the **agent
harness/pipeline**, and the `specs/` storage conventions ([05](../05-storage-and-organization.md)). In
an agent-central org this team is stood up **early** — squads need the harness and the constitution
in the agent's context on day one, not as a Stage-6 afterthought (contrast the single-team sequence
in [11 §1](../11-adoption-and-antipatterns.md#1-how-to-adopt-sdd-incrementally)).

**Spec-review / architecture guild (part-time, cross-cutting).** A rotating group — not a standing
board — that keeps the shared constitution coherent and runs spec review on **high-risk** features
only ([P8](../01-principles.md#p8--specify-in-proportion-to-risk)). Most spec reviews stay inside the
squad as async PR review on `spec.md` ([08 §4](../08-roles-and-workflow.md#4-ceremonies-mapped-to-the-method-not-prescribed)).

## 4. Extended RACI across personas

This widens the core RACI ([08 §2](../08-roles-and-workflow.md#2-raci-for-the-core-artifacts)) with the
two roles scaling adds — the **platform team** and **AI agents**.
`R`=Responsible, `A`=Accountable, `C`=Consulted, `I`=Informed.

| Artifact | Product | Spec author | Architect | Engineers | QA | Platform | AI agents |
|----------|:------:|:-----------:|:---------:|:---------:|:--:|:--------:|:---------:|
| Constitution (shared) | C | C | A/R | C | C | R | I |
| Paved road (templates, CI, harness) | I | C | C | C | C | A/R | I |
| Problem / PRD | A/R | C | I | I | I | I | I |
| Specification (`spec.md`) | A | R | C | C | C | I | I |
| Technical design / ADRs | I | C | A/R | C | I | I | C |
| Task breakdown | I | C | C | A/R | C | I | R |
| Code + tests | C | C | I | A | A/R(tests) | I | R |
| Acceptance (DoD) | A | C | I | R | C | I | I |

> Read the agent column carefully: agents are **R** for tasks and code, never **A**. Every row an
> agent touches has a human accountable for the gate it passes through. The platform team is **A/R**
> for the *standing layer* and otherwise stays out of squad delivery — that is what keeps it a
> multiplier rather than a bottleneck.

## 5. Governance across many teams

The interface between the shared layer and the squads is the same set of artifacts the method
already defines — so governance is mostly *where things live*, not new process:

- **One shared org constitution** — standing law, kept by the architecture guild and the platform
  team, and placed **in the agent's context** (`CLAUDE.md` / steering / `.specify/`) so every
  generation in every squad honours it without re-prompting ([10 §4](../10-ai-assisted-sdd.md#4-practices-that-keep-agent-built-systems-faithful)).
  CI-enforce its checkable clauses.
- **Per-feature specs in each squad's repo** — specs live *with the code* they govern
  ([05 §1](../05-storage-and-organization.md#1-first-principle-specs-live-with-the-code)), never in a
  detached wiki (anti-pattern [G](../11-adoption-and-antipatterns.md#g-specs-in-a-separate-tool-drift-by-design)).
  Product/PRD artifacts may live upstream; the *contract* lives with the code.
- **Stable IDs scoped per feature, globally referenced** — `FR-3` inside a feature, `0001/FR-3`
  across the org ([05](../05-storage-and-organization.md)). This is what makes cross-squad traceability
  and incident post-mortems possible at scale.
- **Spec review is the highest-leverage cross-team ceremony** ([08 §2–3](../08-roles-and-workflow.md#3-the-workflow-with-reviews-and-gates)).
  Keep it inside the squad by default; escalate to the guild only by risk.

> The whole governance model in one line: **shared standing law + per-feature contracts in-repo +
> stable IDs.** Everything else is consumption of the paved road.

## 6. The operating model with agents central

At org scale the [08 §5](../08-roles-and-workflow.md#5-workflow-with-ai-agents-in-the-loop) /
[10 §2](../10-ai-assisted-sdd.md#2-the-agent-operated-lifecycle) split holds in every squad
simultaneously:

```
 Humans (per squad):  Discover ─ Specify ─ Review spec ──────────── Verify vs spec ─ Accept
                                              │  ◆ DoR ◆                  ▲  ◆ DoD ◆
 Agents (per squad):                          └─► Plan ─ Implement ─ Test ─┘
 Platform (shared):   ── constitution + harness + CI keep every squad's agents on the paved road ──
```

So the staffing investment is **not** more implementers — it is more capable **spec authors** and
**verifiers**, plus the **one platform team** that makes the agents trustworthy across squads. A
squad that can hand an agent an unambiguous, testable contract and judge the result against it
out-delivers a larger squad that hand-codes from tickets ([10 §6](../10-ai-assisted-sdd.md#6-the-strategic-point)).

## 7. Enterprise rollout

Use the incremental stages and the 90-day sketch from
[11 §1 / §5](../11-adoption-and-antipatterns.md#1-how-to-adopt-sdd-incrementally) — with three
enterprise adjustments:

1. **Pilot one squad first.** Prove the upstream catches ambiguity in a single stream-aligned squad
   before you ask the org to change. Value before mandate.
2. **Stand up the paved road in parallel, early.** Because agents are central, pull the Stage-4
   tooling (CI checks, harness, constitution-in-context) **forward** — the pilot squad needs it
   immediately, not at the end. The platform team's first customer is the pilot.
3. **Expand squad by squad, not all at once.** Each new squad adopts the *finished* paved road, so
   onboarding cost falls with every squad. The guild forms once two or three squads are live and
   cross-team consistency starts to matter.

Watch the anti-patterns that bite hardest at scale and with agents:
[E — artifacts without gates (theatre)](../11-adoption-and-antipatterns.md#e-artifacts-without-gates-theater),
[F — uniform ceremony for all work](../11-adoption-and-antipatterns.md#f-uniform-ceremony-for-all-work)
(calibrate to risk — [P8](../01-principles.md#p8--specify-in-proportion-to-risk)), and
[I — trusting fluent AI output](../11-adoption-and-antipatterns.md#i-trusting-fluent-ai-output-in-agent-contexts).
The irreducible core does not change with scale: **a testable spec per feature, gated by Ready and
Done, living in the repo** — now multiplied across squads by a shared constitution and a paved road.

---

This concludes the applied annex. Continue to [`D — EARS Requirements Cheat-Sheet`](D-ears-cheat-sheet.md),
or return to the [README](../../README.md) for the full table of contents — or revisit
[08 — Roles & Workflow](../08-roles-and-workflow.md) for the single-team foundation this chapter scales.
