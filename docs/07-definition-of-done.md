# 07 — Definition of Done (DoD)

The **Definition of Done** is the gate work must pass to be called complete. In SDD it carries an
extra, defining obligation beyond ordinary agile DoD: **the implementation is verified against
the specification, and the specification has been reconciled with what was actually built.**

> **DoD answers:** "Is this genuinely complete — verified against the contract, tested,
> documented, and is the spec still true?"

## 1. Why SDD's DoD is special

In most teams DoD means "coded, tested, merged." SDD adds two non-negotiable clauses:

1. **Verified against the spec** — every acceptance criterion in `spec.md` is demonstrably met,
   not just "the code runs." Done is defined by the *contract*, not by the author's sense of
   completion.
2. **Spec reconciled** — if anything about intended behavior changed during construction, the
   spec was updated in the same change ([P1](01-principles.md#p1--the-specification-is-the-source-of-truth),
   [P4](01-principles.md#p4--specifications-are-living-and-versioned)). A merged behavior change
   with a stale spec is **not done** — it has created a lie with authority.

These two clauses are what prevent **spec rot**, the slow death of every SDD adoption.

## 2. The SDD Definition of Done

Work is **Done** when **all** of the following hold. (Copy-ready version:
[`templates/definition-of-done.md`](../templates/definition-of-done.md).)

### A. Specification satisfied
- [ ] **Every acceptance criterion** in the spec is met and demonstrated.
- [ ] **Every functional requirement** (`FR-*`) is implemented and covered by at least one
      automated test that references its ID.
- [ ] **Non-functional requirements** (`NFR-*`) are verified — performance/security/accessibility
      budgets measured against their stated numbers, not assumed.
- [ ] **Edge cases and error behavior** specified in the spec are implemented and tested.

### B. Specification reconciled (the SDD-specific clause)
- [ ] The **spec reflects what was actually built.** Any behavior decided or changed during
      construction is in the spec, in the **same PR**.
- [ ] **Design doc / ADRs updated** for any approach decisions made during build.
- [ ] **Traceability is complete:** need → requirement → test all linked; no orphan requirements,
      no untested requirements.
- [ ] Spec **status** advanced to `done`; front-matter `updated` date current.

### C. Engineering quality
- [ ] Code is **reviewed and merged** to the target branch.
- [ ] **Automated tests pass** in CI (unit, integration, and acceptance/contract tests).
- [ ] **Test coverage** meets the constitution's bar; acceptance scenarios are automated where
      feasible.
- [ ] **Static analysis / linting / type checks** pass.
- [ ] **Security checks** pass (dependency scan, secrets scan, SAST as applicable).
- [ ] No new **TODO/FIXME** that contradicts the spec; no known defects above the agreed severity.

### D. Operability & documentation
- [ ] **User-facing docs / changelog / quickstart** updated.
- [ ] **Observability** in place (logging/metrics/alerts) for the new behavior, per constitution.
- [ ] **Feature flag / rollout / migration** handled if applicable; rollback path known.
- [ ] **Runbook / on-call notes** updated if the change affects operations.

### E. Accepted
- [ ] The **product owner / stakeholder accepts** the work against the spec's acceptance criteria.
- [ ] It is **deployed** to the agreed environment (or merged and ready to deploy, per your flow).

> The litmus test for SDD-Done: *open the spec and the running system side by side — do they
> agree on every acceptance criterion, with a test proving each one?*

## 3. Calibrating DoD to risk

As with DoR, the items are stable; the *depth* scales with risk.

| Work type | How DoD applies |
|-----------|-----------------|
| High-risk / regulated | Full DoD; formal acceptance sign-off; NFR evidence retained for audit; traceability matrix complete. |
| Standard feature | Full DoD as above, proportionate. |
| Small change / bug fix | All clauses still apply, but lighter: the "spec" may be the PR description, acceptance is the reviewer confirming the testable criteria, spec-reconcile means updating whatever doc described the old behavior. |
| Spike | "Done" = learning captured (e.g. in `research.md`) and a decision/next-step recorded — *not* production behavior. |

Note the bug-fix row: even a tiny fix must (a) be verified against its stated expected behavior
and (b) update any spec/doc that described the now-changed behavior. The clauses never *vanish*;
they shrink.

## 4. Automating the DoD

The more of DoD that CI enforces, the less it relies on memory and goodwill:

- **Tests & coverage** — CI gate on passing tests and coverage threshold.
- **Spec-touch check** — a PR that changes behavior-bearing code but no `spec.md` gets flagged
  for human confirmation ("is this really not a behavior change?").
- **Requirement coverage** — a script asserts every `FR-*` in `ready`/`done` specs is referenced
  by a test (see [09 §4](09-quality-and-traceability.md)).
- **Spec lint** — no open questions / `TBD` in a `done` spec; front-matter valid; status
  consistent.
- **Security/lint/type gates** — standard CI.

Automate the *mechanical* clauses; keep human judgment for *acceptance* and *"does the spec
genuinely describe reality?"*

## 5. The reconciliation step in practice

Reconciliation is the habit that makes the difference. During Phase 5 (Verify & Reconcile):

1. **Re-read the spec** against the merged implementation. For each FR/NFR/acceptance criterion,
   confirm the behavior matches — or update the spec to the agreed reality.
2. **Capture decisions** made under construction pressure as ADRs or design updates (so the *why*
   isn't lost).
3. **Close the loop on traceability** — ensure each requirement points to its verifying test.
4. **Advance status** and dates.

If reconciliation regularly turns up *large* divergence between spec and build, that's a signal
your DoR was too weak (you were building from ambiguity) — feed it back into the Ready gate.

## 6. DoD for AI-assisted SDD

When an agent implemented the work:
- [ ] A **human reviewed the output against the spec** — not just for "does it run," but "does it
      satisfy every acceptance criterion?" The agent's confidence is not evidence.
- [ ] The agent's **deviations from the spec are reconciled** — either the code is corrected, or
      the spec is updated with a recorded decision.
- [ ] Generated tests genuinely **verify the spec**, not merely mirror the implementation
      (a classic agent failure mode is tests that assert whatever the code happens to do).

See [10 — AI-Assisted SDD](10-ai-assisted-sdd.md).

## 7. DoR ↔ DoD symmetry

| Definition of Ready | Definition of Done |
|---------------------|--------------------|
| Spec exists, testable, agreed | Spec satisfied and reconciled |
| Acceptance criteria defined | Acceptance criteria demonstrated |
| NFRs specified (quantified) | NFRs verified against the numbers |
| Edge cases specified | Edge cases implemented & tested |
| Feasibility checked | Quality gates passed |
| Stakeholders agree to build | Stakeholders accept the result |

The two gates are mirror images across the construction phase. A disciplined Ready gate makes the
Done gate fast; a sloppy Ready gate makes Done a negotiation. **The spec is the constant on both
sides** — defined at Ready, satisfied and reconciled at Done.

> Continue to [08 — Roles & Workflow](08-roles-and-workflow.md).
