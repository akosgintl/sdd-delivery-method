# Specification-Driven Development (SDD) — A Delivery Method Study

A comprehensive, opinionated study of **Specification-Driven Development** as a software
delivery method: how to move deliberately from **user needs → requirements → specification →
design → implementation**, what **artifacts** are produced along the way, how those artifacts
should be **stored and organized**, and what a robust **Definition of Ready** and **Definition
of Done** look like.

This repository is both a *study* (the reasoning behind the method) and a *toolkit* (templates
and examples you can copy into a real project).

---

## What is Specification-Driven Development?

> **Specification-Driven Development (SDD)** is a delivery method in which an explicit,
> reviewed, version-controlled **specification** is the central artifact of the work. The
> specification — not code, not a ticket, not a conversation — is the primary source of truth.
> Code, tests, and documentation are *derived from and verified against* the specification.

The specification sits between **intent** (what users and the business need) and
**implementation** (the code that satisfies that need). SDD makes the act of writing that
specification a first-class, deliberate phase of delivery rather than an implicit by-product.

```
   USER NEED            REQUIREMENT             SPECIFICATION            IMPLEMENTATION
  (the problem)   →   (what must be true)   →   (precise behavior)   →   (code + tests)
   "I lose work        "The system must         "WHEN the user          function autosave()
    when my            preserve unsaved          stops typing for        { ... }
    browser            edits."                   2s, the system          + tests that assert
    crashes"                                     SHALL persist the       the 2s/persist rule
                                                 draft locally."
   ───────────────────────────────────────────────────────────────────────────────────────
   elicitation         analysis                 specification            construction
                                                 (the contract)          (verified against spec)
```

SDD has two converging lineages, and this study treats both:

1. **The requirements-engineering lineage** — IEEE/ISO requirements specifications,
   design-by-contract, formal methods, BDD/Gherkin, contract-first/API-first design. The idea
   that *a precise written contract should precede and govern construction* is decades old.
2. **The AI-assisted lineage (2024→)** — tools such as **GitHub Spec Kit**, **Amazon Kiro**,
   and others that make the specification the prompt-of-record for AI coding agents. When a
   machine writes the code, the specification is the highest-leverage thing a human can author.

This study is **methodology-first and tool-agnostic**. The AI tooling is covered as one
(increasingly important) way to *operate* the method — not as a prerequisite for it.

---

## Who this is for

- **Engineering leaders / delivery leads** deciding whether and how to adopt SDD.
- **Product managers / business analysts** who own the upstream needs-to-requirements flow.
- **Architects and senior engineers** who author specifications and technical designs.
- **Teams using AI coding agents** that need a disciplined upstream to feed them.

---

## How to read this repository

Read the `docs/` in order for the full study, or jump to the section you need.

| # | Document | What it answers |
|---|----------|-----------------|
| 00 | [Overview](docs/00-overview.md) | What SDD is, why it exists, when to use it (and when not to). |
| 01 | [Principles](docs/01-principles.md) | The core principles and the project "constitution". |
| 02 | [Lifecycle](docs/02-lifecycle.md) | The end-to-end flow and its phases/gates. |
| 03 | [Artifacts](docs/03-artifacts.md) | The complete artifact catalog — what each is, who owns it. |
| 04 | [From Needs to Specification](docs/04-from-needs-to-spec.md) | Elicitation, requirements, and writing the spec (incl. EARS). |
| 05 | [Storage & Organization](docs/05-storage-and-organization.md) | Repo layout, naming, versioning, traceability. |
| 06 | [Definition of Ready](docs/06-definition-of-ready.md) | When a spec is ready to be built. |
| 07 | [Definition of Done](docs/07-definition-of-done.md) | When work is genuinely complete. |
| 08 | [Roles & Workflow](docs/08-roles-and-workflow.md) | Who does what; ceremonies and reviews. |
| 09 | [Quality & Traceability](docs/09-quality-and-traceability.md) | Linking need → requirement → spec → test. |
| 10 | [AI-Assisted SDD](docs/10-ai-assisted-sdd.md) | Spec Kit, Kiro, and operating SDD with agents. |
| 11 | [Adoption & Anti-Patterns](docs/11-adoption-and-antipatterns.md) | Rollout, metrics, and failure modes. |
| 12 | [Glossary](docs/12-glossary.md) | *Reference appendix* — the method's core vocabulary, defined. |
| 13 | [Property Graph (21 Entities)](docs/13-property-graph-21.md) | *Reference appendix* — the glossary as nodes, relationships, Cypher, and a Mermaid diagram. |

### Templates, how-to guides, and examples

- [`templates/`](templates/) — copy-ready artifact templates (constitution, spec, design,
  tasks, ADR, traceability matrix, DoR/DoD checklists).
- [`how-to/`](how-to/) — the **craft layer**: recipe-style guides for writing each artifact *well*
  (before→after rewrites, smell tests, checklists). Read `docs/` for the theory; open a how-to when
  you sit down to author.
- [`examples/`](examples/) — a fully worked example feature showing every artifact populated.

---

## The one-paragraph summary

SDD treats the **specification as the contract and the source of truth**. You elicit user
needs, distill them into testable requirements, and crystallize those into a precise,
reviewable specification before significant construction begins. Each feature gets a small,
self-contained set of artifacts (spec, design, tasks, plus traceability to the originating
need and to tests) stored **in the repository, beside the code, versioned with it**. A
**Definition of Ready** gates entry into construction (the spec is clear, testable, and
agreed); a **Definition of Done** gates exit (the implementation is verified against the spec,
tested, documented, and the spec updated to match reality). Done well, SDD reduces rework,
makes intent auditable, and — critically in the age of AI coding agents — gives both humans and
machines an unambiguous target to build toward.

---

*This study is maintained as living documentation. See [`docs/00-overview.md`](docs/00-overview.md)
to begin.*
