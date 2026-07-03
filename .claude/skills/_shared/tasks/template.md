---
id: NNNN-feature-slug
artifact: tasks
status: draft            # draft | ready | in-progress | done
updated: YYYY-MM-DD
spec: ./spec.md
design: ./design.md
---

# Task Breakdown: <title>

> Ordered, dependency-aware decomposition of the design into independently verifiable tasks.
> Each task names the requirement(s) it advances (traceability).

## Legend
- **ID** `T-n` · **Dep** depends-on · **Req** requirement(s) advanced · **Est** estimate · `[P]` parallelizable

| ID | Task | Dep | Req | Est | Status |
|----|------|-----|-----|-----|--------|
| T-1 | <e.g. Define cart storage schema> | — | FR-1 | <S> | todo |
| T-2 | <e.g. Implement persist-on-change> | T-1 | FR-1 | <M> | todo |
| T-3 | <e.g. Implement restore-on-open> | T-1 | FR-2 | <M> | todo |
| T-4 | <e.g. Implement 24h expiry> [P] | T-1 | FR-3 | <S> | todo |
| T-5 | <e.g. Retry + warning on persist failure> | T-2 | FR-4 | <S> | todo |
| T-6 | <e.g. Acceptance tests referencing FR IDs> | T-2,T-3,T-4 | FR-1..4 | <M> | todo |

## Critical path
<T-1 → T-2 → T-5 → T-6 (longest dependency chain).>

## Parallelizable
<T-4 can proceed alongside T-2/T-3 once T-1 is done.>

## Notes
<Sequencing rationale, spikes needed, external blockers.>
