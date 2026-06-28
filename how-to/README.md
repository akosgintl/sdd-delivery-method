# How-To Guides — Authoring SDD Artifacts Well

This folder is the **craft layer** of the study. The [`docs/`](../docs/) chapters explain *what*
each artifact is and *why* it exists; the [`templates/`](../templates/) give you a blank skeleton to
fill in; the [`examples/`](../examples/) show one filled-in bundle. These guides answer the question
in between: **"I have to write one of these — how do I write a *good* one?"**

Each guide is a recipe: why getting it right matters, the parts that actually carry weight, a
step-by-step procedure, an annotated **before → after** rewrite, a smell test, and a copy-paste
checklist. They **link to** the concept chapters rather than repeat them — read the doc for the
theory, open the how-to when you sit down to write.

> Rule of thumb: if you're learning the idea, read `docs/`. If you're staring at a blank
> `spec.md` at 4 p.m. with a deadline, read `how-to/`.

## The guides (in lifecycle order)

| Guide | Write a good… | Template | Concept doc |
|-------|---------------|----------|-------------|
| [`write-a-problem-statement.md`](write-a-problem-statement.md) | problem statement & user stories | [`vision-prd.md`](../templates/vision-prd.md) | [`docs/04 §1–2`](../docs/04-from-needs-to-spec.md#1-eliciting-user-needs) |
| [`write-a-prd.md`](write-a-prd.md) | vision / PRD | [`vision-prd.md`](../templates/vision-prd.md) | [`docs/03`](../docs/03-artifacts.md) |
| [`write-a-constitution.md`](write-a-constitution.md) | project constitution | [`constitution.md`](../templates/constitution.md) | [`docs/01 §2`](../docs/01-principles.md#2-the-constitution) |
| [`write-a-glossary.md`](write-a-glossary.md) | glossary / ubiquitous language | [`glossary.md`](../templates/glossary.md) | [`docs/12`](../docs/12-glossary.md) |
| [`write-ears-requirements.md`](write-ears-requirements.md) | EARS requirements | [`specification.md`](../templates/specification.md) | [`docs/04 §4`](../docs/04-from-needs-to-spec.md#4-writing-requirements-ears) |
| [`write-nfrs.md`](write-nfrs.md) | non-functional requirements | [`specification.md`](../templates/specification.md) | [`docs/04 §3.1`](../docs/04-from-needs-to-spec.md#31-functional-vs-non-functional) |
| [`write-acceptance-criteria.md`](write-acceptance-criteria.md) | acceptance criteria / scenarios | [`specification.md`](../templates/specification.md) | [`docs/04 §5`](../docs/04-from-needs-to-spec.md#5-writing-requirements-bdd--gherkin-acceptance-scenarios) |
| [`write-a-spec.md`](write-a-spec.md) | the whole `spec.md` | [`specification.md`](../templates/specification.md) | [`docs/04 §6`](../docs/04-from-needs-to-spec.md#6-assembling-the-specification) |
| [`write-a-technical-design.md`](write-a-technical-design.md) | `design.md` + ADRs | [`technical-design.md`](../templates/technical-design.md) | [`docs/03`](../docs/03-artifacts.md) |
| [`write-tasks.md`](write-tasks.md) | task breakdown | [`tasks.md`](../templates/tasks.md) | [`docs/03`](../docs/03-artifacts.md) |
| [`write-dor-dod-gates.md`](write-dor-dod-gates.md) | the DoR & DoD gates | [`definition-of-ready.md`](../templates/definition-of-ready.md) | [`docs/06`](../docs/06-definition-of-ready.md) |

All eleven guides are ready. Filenames are verb-first and unnumbered — these are a parallel toolkit,
not a linear study.

---

Return to the [README](../README.md) for the full table of contents, or browse
[`templates/`](../templates/) for the blank artifacts these guides teach you to fill in.
