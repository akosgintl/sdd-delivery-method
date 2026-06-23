# Templates

Copy-ready artifact templates for Specification-Driven Development. Each maps to a part of the
study in [`../docs/`](../docs/). Copy what the work's risk justifies — the
[minimum viable set](../docs/03-artifacts.md#7-the-minimum-viable-artifact-set) is a need, a
`spec.md`, and the two gates.

| Template | Use for | Study reference |
|----------|---------|-----------------|
| [`constitution.md`](constitution.md) | Project standing rules (once per project) | [docs/01 §2](../docs/01-principles.md#2-the-constitution) |
| [`glossary.md`](glossary.md) | Shared ubiquitous language (once per project) | [docs/03](../docs/03-artifacts.md#glossary--ubiquitous-language) |
| [`vision-prd.md`](vision-prd.md) | Initiative-level why/what | [docs/03](../docs/03-artifacts.md#vision--prd-product-requirements-document) |
| [`specification.md`](specification.md) | **The per-feature spec (central artifact)** | [docs/04](../docs/04-from-needs-to-spec.md) |
| [`technical-design.md`](technical-design.md) | Per-feature technical approach | [docs/03](../docs/03-artifacts.md#designmd--the-technical-design) |
| [`tasks.md`](tasks.md) | Per-feature task breakdown | [docs/03](../docs/03-artifacts.md#tasksmd--the-task-breakdown) |
| [`adr.md`](adr.md) | One significant decision (immutable) | [docs/03](../docs/03-artifacts.md#adr--architecture-decision-record) |
| [`traceability-matrix.md`](traceability-matrix.md) | Explicit trace (audited work) | [docs/09](../docs/09-quality-and-traceability.md) |
| [`definition-of-ready.md`](definition-of-ready.md) | The Ready gate | [docs/06](../docs/06-definition-of-ready.md) |
| [`definition-of-done.md`](definition-of-done.md) | The Done gate | [docs/07](../docs/07-definition-of-done.md) |

## Suggested placement in a real repo
```
.specify/  (or docs/)   constitution.md, definition-of-ready.md, definition-of-done.md, glossary.md
docs/product/           vision-prd.md instances
docs/adr/               NNNN-*.md (ADRs)
specs/NNNN-slug/         spec.md, technical-design.md → design.md, tasks.md, traceability-matrix.md
```
See [docs/05 — Storage & Organization](../docs/05-storage-and-organization.md) for the full layout.
