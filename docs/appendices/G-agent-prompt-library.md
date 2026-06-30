# G — Agent Prompt Library

Copy-ready, **tool-agnostic** prompts for driving an AI coding agent through the SDD lifecycle. They
encode the practices from [`10 §4`](../10-ai-assisted-sdd.md#4-practices-that-keep-agent-built-systems-faithful)
— self-contained spec, standing rules in context, clarify-before-code, spec→test→code, verify against
the spec, reconcile — so each generation honours the contract instead of guessing. Paste them into any
agent (Claude Code, Cursor, Copilot, Aider) or adapt to a tool's slash commands.

> Rule of thumb: the agent has **no tribal knowledge**. If a constraint isn't in the spec or the
> constitution it has loaded, it does not exist for the agent. Every prompt below assumes the
> constitution is in the agent's standing context (`CLAUDE.md` / steering / `.specify/`).

## The phases

Each prompt maps to a lifecycle phase ([`10 §2`](../10-ai-assisted-sdd.md#2-the-agent-operated-lifecycle)).
Replace `‹…›` placeholders.

### 1. Specify
```
Read the need in ‹problem-statement / PRD link›. Draft specs/‹NNNN-slug›/spec.md following
templates/specification.md: YAML front-matter, the need, then functional requirements in EARS form
(shall / shall not) and quantified NFRs. One testable statement per requirement, with a stable ID
(FR-n / NFR-n). Add acceptance criteria (Given/When/Then), explicit non-goals, and edge/error cases.
Do NOT propose implementation. List every assumption you had to make as an open question.
```

### 2. Clarify (drive open questions to zero)
```
Review specs/‹NNNN-slug›/spec.md only as a skeptical reviewer. List every ambiguity, missing edge
case, unquantified NFR, and untestable requirement. For each, ask the one question whose answer
makes it testable. Do not write code or design. Stop when the open-questions list is empty or only
contains items needing a human decision — flag those for me.
```

### 3. Plan / design
```
Using specs/‹NNNN-slug›/spec.md as the contract, write design.md per templates/technical-design.md:
the approach, interfaces/contracts, data model, and test strategy. Record each significant decision
as an ADR. Where a requirement is infeasible or disproportionately costly, STOP and tell me — propose
a spec change with rationale rather than silently working around it. Obey the constitution.
```

### 4. Tasks
```
Decompose design.md into tasks.md per templates/tasks.md: ordered, independently verifiable tasks,
each mapped to the requirement ID(s) it advances. Mark what is parallelizable and what is on the
critical path. Keep tasks small enough to verify in isolation.
```

### 5. Implement (spec → test → code)
```
Implement tasks.md against the spec. For each requirement, FIRST write the test derived from its
acceptance criteria, then the code that makes it pass — the test must encode the contract, not mirror
the implementation. Reference the requirement ID in each test. Honour the constitution. If you make
any behavior decision the spec doesn't cover, STOP and update the spec in the same change (or ask).
```

### 6. Verify (against the spec, not vibes)
```
Act as an adversarial verifier. For each acceptance criterion in spec.md, find the test that proves
it and run it; report PASS/FAIL per criterion with evidence. Flag any requirement with no test, any
test that asserts implementation rather than the criterion, and any NFR not actually measured. Do not
trust the code's apparent intent — only the criteria.
```

### 7. Reconcile (close the loop)
```
Compare the merged code against spec.md and design.md. List every place behavior diverges from the
spec. For each, either correct the code or update the spec/ADR to match reality — propose the diff.
Confirm traceability is complete: need → requirement → test all linked. The spec must end true.
```

## Using these well

- **Keep specs small.** Per-feature specs fit the agent's context and keep generations focused
  ([P6](../01-principles.md#p6--the-smallest-useful-unit-is-the-feature-not-the-project) matters
  *more* with agents).
- **Gate, don't trust.** The DoR before phase 5 and the DoD after phase 6 are where quality is won —
  see [`06`](../06-definition-of-ready.md) and [`07`](../07-definition-of-done.md). Agent confidence
  is not evidence.
- **Separate the verifier.** Run phase 6 as a *different* agent session (or a human) than the one that
  implemented — self-grading agents rationalize ([anti-pattern I](../11-adoption-and-antipatterns.md#i-trusting-fluent-ai-output-in-agent-contexts)).

> The prompts are the easy part. The leverage is the spec they consume and the gates that judge their
> output — that's the whole thesis of [`10 §6`](../10-ai-assisted-sdd.md#6-the-strategic-point).

---

Return to the [README](../../README.md) for the full table of contents, or back to
[`A — Glossary`](A-glossary.md) to start the appendices over.
