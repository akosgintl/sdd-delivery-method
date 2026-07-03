---
id: NNNN-feature-slug
artifact: design
status: draft            # draft | in-review | agreed | done
owner: <architect/lead>
updated: YYYY-MM-DD
spec: ./spec.md
---

# Technical Design: <title>

> How we intend to satisfy `spec.md`. This is the **how**; behavior lives in the spec.

## 1. Overview
<The chosen approach in a few sentences.>

## 2. Approach & alternatives considered
| Option | Pros | Cons | Decision |
|--------|------|------|----------|
| <chosen> | | | ✅ chosen |
| <alt 1> | | | rejected — <why> |

<Significant decisions get an ADR — link them here.>

## 3. Components & responsibilities
<Modules/services involved and what each is responsible for. Diagram if helpful.>

## 4. Interfaces & contracts
<APIs, schemas, events. Link to contracts/ files. Note which spec requirements they realize.>

## 5. Data model
<Entities, relationships, constraints, migrations. Link to data-model.md if separate.>

## 6. Integration points & dependencies
<External systems, feature flags, sequencing with other work.>

## 7. Test strategy
> How each requirement will be verified. Tie back to spec IDs.
- FR-1 → <test type / approach>
- NFR-1 → <measurement approach (load test, scan, audit)>

## 8. Risks & mitigations
| Risk | Likelihood | Impact | Mitigation |
|------|-----------|--------|------------|

## 9. Rollout & operability
<Deployment, migration, feature flag, rollback, observability (logs/metrics/alerts).>
