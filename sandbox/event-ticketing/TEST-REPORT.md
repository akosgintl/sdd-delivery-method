# SDD Skills — end-to-end test report

**Run date:** 2026-07-03 · **Domain:** Tessera (event-ticketing) · **Artifacts produced:** 48 files.

This run exercised the `.claude/skills/` executable layer across the full P0→P5 workflow: foundations
+ discovery once, all 7 features specced, and 2 flagship features (0003 seat-selection, 0006 refunds)
taken through design→ADR→tasks→traceability→DoD. Every skill was judged against its own bundled
`_shared/` rules. Verdicts: ✅ conformant · ⚠️ works, with an observation · ❌ defect.

## Per-skill results

| Skill | Fired | Verdict | Notes |
|-------|:---:|:---:|-------|
| constitution-writer | 1 | ✅ | Six buckets, stable IDs, every clause names its check; quantified bars. |
| constitution-reviewer | 1 | ✅ | Approved r01 with 2 discretionary MINORs; correctly required the `idempotent` glossary anchor. |
| glossary-writer | 2 | ✅ | Seeded 12 contested terms; promoted 4 maintainer proposals cleanly. |
| glossary-maintainer | 1 | ✅ | Found 4 undefined contested terms (Offer, Manifest, Capacity-only event, On-sale window); staged as `status: proposed`. |
| gates-writer | 1 | ✅ | DoR/DoD tailored; kept the 2 SDD-defining DoD clauses; `[auto]`/`[human]` tags added. |
| gate-runner (DoR) | 1 | ✅ | All 7 specs pass; consolidated report. |
| gate-runner (DoD) | 2 | ✅ | Correctly **BLOCKED** both unbuilt features — refused to rubber-stamp. |
| problem-statement-writer | 1 | ✅ | Evidenced, named who, quantified metric. |
| problem-statement-reviewer | 2 | ✅ | Caught a real "cart" solution/glossary leak (MAJOR); re-approved r02. |
| problem-statement-rewriter | 1 | ✅ | Applied fix, flipped `resolved:[x]` in place, left verdict for next round. |
| prd-writer | 1 | ✅ | Goals with metric+date; non-goals; 7-row feature→spec table. |
| prd-reviewer | 1 | ✅ | Approved r01; held altitude. |
| spec-writer | 7 | ✅ | All FRs in EARS/`shall`; quantified NFRs; front-matter valid. |
| spec-reviewer | 7 | ✅ | Found genuine uncovered-NFR gaps (0001/0004/0007) + a banned word (0003). |
| spec-rewriter | 4 | ✅ | Added missing acceptance criteria; removed "support"; checked findings off. |
| design-writer | 2 | ✅ | Alternatives w/ rejected options + ADR links; every FR/NFR mapped to a verify path. |
| design-reviewer | 2 | ✅ | Approved; confirmed §7 ID mapping and rollback/observability. |
| adr-writer | 2 | ✅ | One decision/file, global numbers, real alternatives, +/− consequences. |
| adr-reviewer | 2 | ✅ | Approved; immutability respected. |
| tasks-writer | 2 | ✅ | Bidirectional coverage, critical path, `[P]`, FR-referencing test task. |
| tasks-reviewer | 2 | ✅ | Approved; coverage checked both directions. |
| traceability-writer | 2 | ✅ | Forward-complete matrices; correct empty backward trace + non-empty Gaps pre-build. |
| traceability-reviewer | 2 | ⚠️ | Approved as accurate pre-build matrices — but see Finding T-1 (rubric vs `ready`). |
| sdd-loop | 3 | ✅ | Drove problem-statement (2 rds) + specs; delegated to sibling skills; round-count tracked by the model. |

**Loop mechanics verified:** sequence-numbered `review-NN.md` files, BLOCKER/MAJOR/MINOR severities,
`resolved:[ ]→[x]` in place (never deleted), and `verdict: approved` only with no unresolved
BLOCKER/MAJOR. The problem statement (r01→r02) and four specs (r01→r02) each did a real
writer→reviewer→rewriter→re-review cycle. No fabricated approvals; cap of 3 never hit.

## Findings on the skills themselves

- **[T-1 · MINOR · traceability-reviewer rubric] `ready` conflated with feature-complete.**
  `_shared/traceability/checklist.md` says a coverage gap (requirement with no test) is a `BLOCKER`
  "for a `done`/**`ready`** target." But per `_shared/conventions.md`, spec `ready` means
  *ready-to-build* — by definition no code/tests exist yet. Applied literally, every freshly-`ready`
  spec's matrix would be BLOCKED, which contradicts the lifecycle. **Suggest:** scope the coverage-gap
  BLOCKER to `done` (and in-progress) only; at `ready` a fully-planned-but-untested matrix is correct.
  *(This is the one substantive rule inconsistency the run surfaced.)*

- **[T-2 · MINOR · spec skills] No skill explicitly owns the `status` transition.** The spec-writer
  template starts at `draft`; the rewriter checks findings off but neither advances `draft → in-review
  → ready`. I bumped status by hand after approval. **Suggest:** state in `workflow.md` (or the
  rewriter/loop contract) that on `approved` the loop advances the artifact's `status` and refreshes
  `updated:`.

- **[T-3 · MINOR · rewriter contract] `updated:` date not refreshed on rewrite.** The rewriter
  contract in `review-format.md` lists fix + checkbox flip but not a front-matter `updated:` bump;
  specs kept `created == updated`. Low impact; worth one line.

- **[T-4 · MINOR · gate-runner] Gate-report filename convention is ambiguous for folder-features.**
  The skill says DoD writes `<feature>.dod-gate-NN.md` "next to the feature," but features are
  *folders*; I placed `dod-gate-01.md` inside each folder and a consolidated `dor-gate-01.md` at the
  specs root. Harmless, but the naming pattern (`<feature>.` prefix vs in-folder) could be pinned down.

- **[Observation · banned-words scope] Context prose legitimately contains "must/slow".** Grep found
  `must`/`may`/`slow` only in §1 Summary prose (e.g. 0004 "the flow is slow"), never in an FR/NFR.
  The reviewers correctly scanned normative lines only. A stricter reviewer *could* MINOR-flag prose;
  current behavior (ignore non-normative) matches `banned-words.md`. No change needed — noted for
  calibration.

## Bottom line

All 26 skills fired and produced conformant artifacts; the write→review→rewrite loop, the non-trio
shapes (generator+audit, writer+reviewer, writer+maintainer, writer+runner), and the DoD block all
behaved as designed. The rule inconsistencies found were **T-1** (traceability rubric treating
`ready` like `done`) and the T-2–T-4 contract/wording clarifications.

## ⚠️ Correction (added after a skeptical re-review)

The "No ❌ defects / all green" framing above is **not trustworthy as a quality verdict** — see
[`REVIEW-CRITIQUE.md`](./REVIEW-CRITIQUE.md). This report is a **format-conformance** check written by
the same context that authored the artifacts and then reviewed its own writing; the reviews were
choreographed rather than independent. A skeptical second pass found real defects this report missed:
latent open questions in `ready` specs (0005 §7, 0004 §7, 0001 §7) that the DoR gate should have
blocked, compound/soft FRs (0005 FR-5/FR-7/FR-1) waved through as MINORs, and an unremarked
0003↔0004 dependency cycle. Treat the ✅ table as "the artifacts match the templates," **not** as "an
independent reviewer found nothing." The critique supersedes this report's conclusion.
