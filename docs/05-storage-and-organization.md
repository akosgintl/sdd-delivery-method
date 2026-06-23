# 05 — Storage & Organization of Artifacts

Where artifacts live, how they're named, and how they're versioned is not a clerical detail — it
is what makes the specification usable as a source of truth. This chapter gives a concrete,
recommended layout and the conventions that keep it healthy.

## 1. First principle: specs live with the code

> **Store specifications in the repository, beside the code they govern, versioned with it.**

This is the single most important storage decision. Specs in a wiki or a separate document store
drift, because they are not part of the change that alters behavior. Specs in the repo:

- are reviewed in the **same pull request** as the code that satisfies them;
- have a **history** you can `git blame` and bisect;
- are **branchable** — a feature branch carries its own evolving spec;
- are **diffable** in review, so behavior changes are visible;
- travel with the code to every clone, fork, and AI agent context.

The narrow exception: *upstream product artifacts* (PRDs, roadmaps) may live in a product tool
(Notion, Confluence, Jira) because their audience and cadence are different — but the **spec**
(the engineering contract) belongs in the repo, with a link back to the PRD.

## 2. Recommended repository layout

Two layouts are common. Choose based on how tightly specs bind to code.

### Layout A — centralized `specs/` tree (recommended default)

Best when you want all specs discoverable in one place and a clean separation from source.

```
repo/
├── .specify/                      # or /docs/ — project-level "standing" artifacts
│   ├── constitution.md            #   non-negotiable principles (project law)
│   ├── definition-of-ready.md
│   ├── definition-of-done.md
│   └── glossary.md
├── docs/
│   ├── product/                   # PRDs, vision (or links to product tool)
│   │   └── prd-checkout-revamp.md
│   └── adr/                       # cross-cutting Architecture Decision Records
│       ├── 0001-use-postgres.md
│       └── 0002-event-sourced-orders.md
├── specs/
│   ├── 0001-cart-persistence/     # one numbered folder per feature
│   │   ├── spec.md                #   THE specification (contract)
│   │   ├── design.md              #   technical design
│   │   ├── tasks.md               #   task breakdown
│   │   ├── traceability.md        #   need→req→test map (or inline IDs)
│   │   ├── contracts/             #   API/schema contracts (optional)
│   │   │   └── cart-api.openapi.yaml
│   │   ├── research.md            #   spikes/options (optional)
│   │   └── data-model.md          #   entities (optional)
│   ├── 0002-guest-checkout/
│   │   └── spec.md
│   └── README.md                  # index of all features + status
├── src/ ...                       # the code
└── tests/ ...                     # tests; acceptance tests reference spec IDs
```

### Layout B — co-located specs (alternative)

Best when features map cleanly to modules and you want the spec to sit *inside* the module.

```
repo/
├── .specify/constitution.md
├── src/
│   └── cart/
│       ├── spec.md                # spec lives with the module it governs
│       ├── design.md
│       ├── cart_service.py
│       └── cart_service_test.py
└── docs/adr/...
```

Co-location maximizes proximity (the spec is impossible to miss) but scatters specs, making
project-wide review and cross-feature traceability harder. **Default to Layout A** unless your
modules are unusually stable and self-contained.

### How the AI-SDD tools lay things out (for reference)
- **GitHub Spec Kit** uses `.specify/` for project assets (incl. `memory/constitution.md`) and
  numbered `specs/NNN-feature/` folders containing `spec.md`, `plan.md`, `tasks.md`, plus
  `research.md`, `data-model.md`, `contracts/`, `quickstart.md`. Layout A above is deliberately
  compatible.
- **Amazon Kiro** uses `.kiro/specs/<feature>/` containing `requirements.md` (EARS), `design.md`,
  `tasks.md`, with project "steering" files under `.kiro/steering/`.

You do not need these tools to use the layout — but adopting their conventions keeps you
tool-compatible if you add one later.

## 3. Naming conventions

Consistency here is what makes traceability cheap.

- **Feature folders:** `NNNN-kebab-case-slug` (e.g. `0001-cart-persistence`). Zero-padded
  numbers sort correctly and give every feature a short, stable handle.
- **Requirement IDs:** `FR-<n>` / `NFR-<n>`, **scoped to the feature**. Globally unique reference
  is `0001/FR-3` (feature + requirement). Stable IDs are the backbone of traceability — **never
  renumber**; retire IDs instead.
- **ADRs:** `NNNN-short-title.md`, numbered globally, never deleted (superseded ADRs are marked,
  not removed).
- **Status values:** use a small fixed vocabulary — `draft → in-review → ready → in-progress →
  done → superseded`. Put status in the spec's front-matter.

### Front-matter (machine-readable metadata)
Put structured metadata at the top of each spec so it can be indexed and validated:

```yaml
---
id: 0001-cart-persistence
title: Shopping cart survives a browser crash
status: ready            # draft|in-review|ready|in-progress|done|superseded
owner: a.gintl
created: 2026-06-01
updated: 2026-06-18
need: docs/product/prd-checkout-revamp.md#cart-loss
supersedes: null
---
```

## 4. Versioning specifications

Because specs live in Git, **Git is your versioning system** — but observe these conventions:

- **The spec changes in the same PR as the behavior it describes.** This is enforced by the
  [DoD](07-definition-of-done.md). A behavior change with no spec diff should fail review.
- **Status reflects reality.** A `done` spec describes shipped behavior; a `draft` may describe
  intent. Don't leave specs stuck in `in-progress` after release.
- **Supersession over mutation for big pivots.** When a feature is fundamentally redesigned,
  consider a new feature folder that `supersedes:` the old one, rather than rewriting history —
  this preserves the record of what was once true and why it changed.
- **Tag releases.** A Git tag/release lets you reconstruct "what was the spec at the time we
  shipped v2.3?" — valuable for audits and incident analysis.
- **ADRs are immutable.** Never edit a decided ADR's decision; add a new ADR that supersedes it.

## 5. Linking and traceability mechanics

Traceability ([09](09-quality-and-traceability.md)) is implemented through **stable IDs
cross-referenced across artifacts** — the lightest mechanism that works:

```
PRD section  ──►  spec.md (FR-3)  ──►  tasks.md (T-7 "implements FR-3")
                       │                      │
                       └──────────►  tests/cart_persistence_test.py
                                     # test name or tag references FR-3
```

- The **spec** links *up* to the need/PRD in its front-matter.
- **Tasks** name the requirement IDs they advance.
- **Tests** reference requirement IDs (in the test name, a tag, or a comment), so a coverage tool
  can answer "which requirements have tests?"
- An optional **traceability matrix** file makes this explicit for audited/regulated work; for
  most teams the cross-references *are* the matrix and a script can generate the view.

## 6. Discoverability

- Maintain a **`specs/README.md` index** listing every feature, its status, and owner. Regenerate
  it from front-matter with a small script so it never goes stale.
- Keep the **constitution and DoR/DoD at a fixed, well-known path** so every contributor (and
  agent) can find the standing rules.
- A **glossary** at a fixed path keeps the ubiquitous language one click away.

## 7. Lifecycle hygiene (avoiding spec rot)

The failure mode that kills SDD is specs that lie. Defenses:

- **DoD enforcement:** updating the spec is part of "done," checked in review and ideally in CI.
- **CI lint on specs:** validate front-matter, forbid `TBD`/open-questions in `ready`/`done`
  specs, check that every `FR-*` is referenced by at least one test (see [09](09-quality-and-traceability.md)).
- **Status audits:** periodically flag specs stuck in `in-progress`, or `done` specs whose tests
  are failing.
- **Retire, don't delete:** mark removed features' specs `superseded` with a pointer, preserving
  the historical record.

## 8. Storage decision checklist

- [ ] Specs live **in the repo**, beside code, versioned with it.
- [ ] Each feature has a **numbered folder** with at least `spec.md`.
- [ ] Project **constitution** and **DoR/DoD** live at fixed, known paths.
- [ ] Requirements have **stable IDs**; nothing is renumbered.
- [ ] Specs carry **front-matter** (id, status, owner, links).
- [ ] A **`specs/README.md` index** exists (ideally generated).
- [ ] The **DoD requires** the spec to be updated in the same PR.
- [ ] ADRs are **append-only**; supersession is explicit.

> Continue to [06 — Definition of Ready](06-definition-of-ready.md).
