# 12 — Glossary

The twenty terms below are the most frequently used entities across the study (chapters `00`–`11`),
ranked by occurrence and de-duplicated for synonyms — `requirement` absorbs `NFR`, `acceptance`
absorbs `criteria`, `code` absorbs `implementation`, `need` absorbs `problem`. They are the working
vocabulary of SDD: if a team agrees on these definitions, most arguments about "what we mean" go
away. Definitions are listed alphabetically; each links to the chapter that explains it in full.

> Rule of thumb: a term belongs here only if two people could otherwise mean two different things
> by it. The glossary exists to collapse that gap, not to decorate the docs.

---

**Acceptance criteria** — The concrete, pass/fail conditions that a [requirement](#requirement) must
satisfy to be considered met. Written before [code](#code), they turn intent into a checkable
contract and seed the [tests](#test). Often expressed as Given/When/Then (Gherkin) scenarios. A
requirement without acceptance criteria is an opinion, not a spec. See
[`04 — From Needs to Spec`](04-from-needs-to-spec.md).

**Agent** — An AI coding assistant (e.g. Claude Code) that reads the spec, design, and tasks and
produces or modifies [code](#code). In SDD the agent is a fast, literal executor: it amplifies a
precise [specification](#specification) and equally amplifies an ambiguous one. The human stays
accountable for intent and [review](#review). See [`10 — AI-Assisted SDD`](10-ai-assisted-sdd.md).

**Artifact** — Any durable, version-controlled document the method produces — `spec.md`,
`design.md`, `tasks.md`, the [constitution](#constitution), ADRs, the traceability matrix. Artifacts
are the interface between phases and between people; they outlive the conversation that created them.
See [`03 — Artifacts`](03-artifacts.md).

**Behavior** — What the system does, observable from the outside, independent of how it is built.
Specs describe behavior; designs describe structure. EARS requirements and acceptance criteria pin
behavior down so it can be tested rather than argued about. See
[`04 — From Needs to Spec`](04-from-needs-to-spec.md).

**Change** — A unit of intended modification to the system, traced from a [need](#need) through
spec, design, and tasks to merged [code](#code). SDD's claim is that catching a change while
it is still words is cheap; catching it after it is code is expensive. See
[`02 — Lifecycle`](02-lifecycle.md).

**Code** — The implementation: the executable output the whole method exists to make correct and
intentional. In SDD code is downstream of the spec — it is where decisions are *executed*, not where
they are *made*. See [`02 — Lifecycle`](02-lifecycle.md).

**Constitution** — The standing, project-wide set of rules and principles every spec must obey
(coding standards, security baselines, architectural constraints). It is the "law" of the repo:
written once, amended deliberately, applied to every [feature](#feature). See
[`01 — Principles`](01-principles.md).

**Design** — The artifact (`design.md`) describing *how* a [feature](#feature) will be built —
structure, interfaces, data, and the trade-offs behind them — bridging the spec (what) and the
[code](#code) (built). Significant design decisions are recorded as ADRs. See
[`03 — Artifacts`](03-artifacts.md).

**Definition of Done (DoD)** — The exit [gate](#gate): the checklist a [change](#change) must pass
before it is considered complete — tests green, acceptance criteria met, traceability updated, review
done. Done means *demonstrably* done, not "works on my machine." See
[`07 — Definition of Done`](07-definition-of-done.md).

**Definition of Ready (DoR)** — The entry [gate](#gate): the checklist a spec must pass before
construction starts — clear [need](#need), testable [requirements](#requirement), acceptance
criteria, no blocking open questions. DoR stops half-baked work from entering the build phase. See
[`06 — Definition of Ready`](06-definition-of-ready.md).

**Feature** — A bounded, independently specifiable unit of capability, stored as a numbered bundle
(`NNNN-kebab-case-slug`) containing its spec, design, tasks, and traceability. The feature is SDD's
unit of organization. See [`05 — Storage & Organization`](05-storage-and-organization.md).

**Gate** — A quality checkpoint between [phases](02-lifecycle.md) where work is verified before it proceeds —
chiefly the [Definition of Ready](#definition-of-ready-dor) and [Definition of Done](#definition-of-done-dod).
Gates make quality a precondition rather than an afterthought. See
[`02 — Lifecycle`](02-lifecycle.md).

**Need** — The underlying problem, motivation, or business value a [change](#change) exists to serve
— the "why" behind a spec, captured in front-matter and the problem statement. Specs that skip the
need optimize the wrong thing precisely. See [`04 — From Needs to Spec`](04-from-needs-to-spec.md).

**Requirement** — A single, testable statement of something the system shall (or shall not) do,
written in EARS form and carrying a stable ID (`FR-<n>` / `NFR-<n>`). Functional requirements
describe behavior; non-functional requirements (NFRs) quantify qualities with numbers. The atom of a
spec. See [`04 — From Needs to Spec`](04-from-needs-to-spec.md).

**Review** — The human act of reading an [artifact](#artifact) against intent and standards before it
advances — reviewing a spec before building costs minutes; reviewing only the [code](#code) costs
rework. SDD front-loads review onto cheap, editable words. See
[`08 — Roles & Workflow`](08-roles-and-workflow.md).

**SDD (Specification-Driven Development)** — The delivery method this study describes: write a
precise, testable [specification](#specification) first, gate it, then let humans and
[agents](#agent) build against it. The thesis is that intent belongs in reviewable artifacts, not in
people's heads or after-the-fact code. See [`00 — Overview`](00-overview.md).

**Specification (spec)** — The central artifact (`spec.md`): the agreed, testable statement of what a
[feature](#feature) must do — [needs](#need), [requirements](#requirement), and
[acceptance criteria](#acceptance-criteria) — and the contract everything downstream is built and
checked against. The most-used word in the method, and its center of gravity. See
[`03 — Artifacts`](03-artifacts.md).

**Test** — An automated, executable check that a [requirement](#requirement) holds, derived directly
from its [acceptance criteria](#acceptance-criteria). Tests are how a spec stays true over time: they
convert a written claim into a repeatedly verifiable one. See
[`09 — Quality & Traceability`](09-quality-and-traceability.md).

**Traceability** — The maintained linkage from [need](#need) → [requirement](#requirement) →
[design](#design) → task → [code](#code) → [test](#test), usually as a matrix. It answers
"why does this code exist?" and "what breaks if this requirement changes?" — and exposes orphans and
gaps. See [`09 — Quality & Traceability`](09-quality-and-traceability.md).

**Work** — The execution effort that turns a ready spec into done [code](#code). SDD's framing is
that work should begin only after intent is settled, so effort is spent *building the right thing*
rather than rediscovering what it was. See [`08 — Roles & Workflow`](08-roles-and-workflow.md).

---

This is a reference appendix to the study. Return to the [README](../README.md) for the table of
contents, or browse [`templates/`](../templates/) for the copy-ready artifacts these terms name.

> Continue to [`13 — Property Graph (21 Entities)`](13-property-graph-21.md), which turns this
> glossary into nodes, relationships, Cypher, and a Mermaid diagram.
