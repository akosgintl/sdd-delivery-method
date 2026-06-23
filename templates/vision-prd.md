---
type: prd
title: <initiative name>
status: draft            # draft | active | shipped | archived
owner: <product manager>
updated: YYYY-MM-DD
---

# PRD: <initiative name>

> Product-level "why and what" for an initiative. Upstream of specs — one PRD typically spawns
> several feature specs. Keep at the why/what altitude; behavioral precision lives in the specs.
> See [docs/03](../docs/03-artifacts.md#vision--prd-product-requirements-document).

## 1. Problem & opportunity
<Who has the problem, what it is, the cost of not solving it. Evidence.>

## 2. Goals & success metrics
- Goal: <outcome>. Metric: <how measured>. Target: <number by when>.

## 3. Target users & needs
<Personas / segments and their jobs-to-be-done.>

## 4. Scope
- **In scope:** <capabilities at a high level>.
- **Out of scope / non-goals:** <explicitly excluded>.

## 5. Constraints & assumptions
<Regulatory, technical, timeline, budget; key assumptions.>

## 6. Feature breakdown → specs
> Each row becomes a spec under `specs/`.

| Feature | Spec | Status |
|---------|------|--------|
| <feature 1> | specs/NNNN-slug/spec.md | <status> |

## 7. Risks & open questions
<Initiative-level risks; questions to resolve before/while specifying.>
