# SDD workflow & loop rules (bundled reference)

The canonical rules the `sdd-loop` driver and every reviewer/rewriter follow. Self-contained.

## End-to-end pipeline

```
P0 Foundations (once per project)
   constitution-writer ⟲ constitution-reviewer   ·   glossary-writer (seed)   ·   gates-writer
P1 Discovery (per initiative)
   problem-statement  [loop]  →  prd  [loop]
P2 Spec (per feature)
   spec  [loop]  →  glossary-maintainer (scan spec for undefined terms)  →  ══ DoR gate ══
P3 Design (per feature)
   design  [loop]   +   adr-writer ⟲ adr-reviewer   (per significant decision)
P4 Plan (per feature)
   tasks  [loop]
P5 Trace / verify (per feature)
   traceability-writer (generate)  →  traceability-reviewer (audit)
   →  build (outside the skills)    →  ══ DoD gate ══
```
`[loop]` = the inner cycle below.

## Inner loop (every trio artifact: spec, design, tasks, prd, problem-statement)

```
writer → v1
repeat:
    reviewer → <artifact>.review-NN.md   (verdict + BLOCKER/MAJOR/MINOR findings)
    if verdict == approved: break                     # approved ⇔ no unresolved BLOCKER/MAJOR
    rewriter → apply unresolved findings, flip resolved:[x] → v(N+1)
    if round == CAP: stop and surface the open review-NN.md to a human
```

- **Approval rule:** BLOCKER and MAJOR block; MINOR is the author's discretion.
- **CAP:** default **3** rounds. On cap-out, do not force an approval — stop and hand the open
  `review-NN.md` to a human. This prevents writer/reviewer ping-pong.
- **On approval, advance status.** When the loop reaches `approved`, the driver advances the
  artifact's front-matter `status` to the next lifecycle value (spec `in-review → ready`, design
  `in-review → agreed`, tasks `draft → ready`, problem-statement/PRD `draft → agreed/active`) and
  refreshes its `updated:` date. Approval that leaves `status` untouched makes a finished artifact
  look un-progressed and blocks the next gate.
- **Review with fresh eyes.** The reviewer step must judge, not rationalize — run it with only the
  artifact + these `_shared/` rules, ideally as a *separate* sub-agent that never saw the writer's
  reasoning. A self-review sharing the writer's context misses the defects the author was blind to; a
  clean same-context review is weak evidence, not proof. See `../sdd-loop/SKILL.md` §Reviewer
  independence.

## Non-trio shapes

- **traceability** (writer + reviewer, no rewriter): the writer *generates* the matrix from IDs;
  re-running it is the "regenerate" step. Reviewer findings **escalate upstream** (add a test/task
  in spec/tasks) — the matrix is never hand-patched to fake coverage.
- **adr / constitution** (writer + reviewer, no rewriter): "change" = author a new superseding ADR
  (append-only) or a dated amendment. The reviewer checks completeness + immutability.
- **glossary** (writer + maintainer): the maintainer lints and proposes; the writer promotes
  accepted proposals.
- **gates** (writer + gate-runner): the runner is a pass/blocked checkpoint, not a review loop.

## Outer loops (cross-artifact)

- **Upstream escalation.** When a reviewer's root cause is upstream (e.g. `tasks-reviewer` finds an
  FR that can't be tasked because it's untestable), re-open the upstream loop (spec), then ripple
  down: re-review design → tasks → traceability.
- **Traceability escalation.** A coverage gap ("FR-3 has no test") is fixed in spec/tasks (their
  rewriters), then `traceability-writer` regenerates and `traceability-reviewer` re-audits.
- **Gate bounce.** DoR fail → back into the spec loop. DoD fail → back into the offending
  artifact's loop.
- **Ripple / staleness.** When an upstream artifact changes after downstream ones already exist,
  mark the downstream **stale** and re-run its *reviewer* (a cheap re-check, not always a full
  rewrite). A spec change ⟹ design + tasks + traceability get re-reviewed.
