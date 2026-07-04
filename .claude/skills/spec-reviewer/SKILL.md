---
name: spec-reviewer
description: >-
  Review a Specification-Driven Development feature spec (spec.md) against EARS compliance, quantified
  NFRs, banned vague words, 1:1 acceptance-to-requirement mapping, front-matter/status validity, and
  up/down traceability — then write sequence-numbered structured findings to spec.review-NN.md. Use
  when the user asks to review, critique, check, audit, or QA a spec / spec.md / feature requirements.
metadata:
  layer: sdd-skills
  role: reviewer
  artifact: spec
---

# spec-reviewer

Audit a `spec.md` and emit a structured, sequence-numbered findings file. You judge; you do not edit
the spec (that is `spec-rewriter`'s job).

## Read first
- `../_shared/spec/checklist.md` — the rubric (sections, smell test, checklist, severities).
- `../_shared/conventions.md`, `../_shared/ears.md`, `../_shared/banned-words.md`, `../_shared/gherkin.md`.
- `../_shared/spec/example.md` — the known-good reference (do not false-positive on things it does well).
- `../_shared/review-format.md` — the exact findings-file format, naming, and verdict rule.

## Procedure
1. **Locate the spec** and any existing `spec.review-NN.md` next to it; the file you write is
   `spec.review-<highest+1>.md` (first review → `spec.review-01.md`).
2. **Run every check** in `../_shared/spec/checklist.md`, in particular:
   - front-matter complete + `status` legal + `updated` current + `need` resolves;
   - each FR is one of five EARS patterns with **shall/shall not** and is singular & testable;
   - NFRs quantified; no banned words in normative lines;
   - acceptance criteria map 1:1 to `FR`/`NFR` IDs;
   - goals **and** non-goals present; happy/boundary/error covered;
   - §10 empty when `status` is `ready`/`done`;
   - **internal consistency & latent open questions:** no two normative lines fix the same value
     differently (e.g. an FR hard-coding `300 s` while an NFR makes it configurable); no FR joins two
     distinct behaviors with "and"/"or"; and **no hedge lurks outside §10** — a `(design; flagged)`,
     `TBD`, or unresolved "A **or** B" choice anywhere in §4/§7/§8/§11 while §10 says none and
     `status` is `ready`/`done` is a `[BLOCKER]` (an open question in disguise);
   - IDs stable, singular, no drift vs anything that references them.
3. **Record each violation as a finding** — severity (`BLOCKER`/`MAJOR`/`MINOR`), the id/section,
   a one-line defect, `file:line`, the `rule:` (with its `../_shared/…` source), a concrete
   `fix:`, and `resolved: [ ]`.
4. **Set the verdict** — `approved` iff no unresolved BLOCKER/MAJOR; else `changes-requested`.

## Output contract
- Write `spec.review-NN.md` next to the spec, exactly per `../_shared/review-format.md`.
- Do not modify `spec.md`. Report the review path, the verdict, and the finding counts by severity.
