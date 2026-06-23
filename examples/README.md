# Worked Example

A single feature — **shopping cart survives a browser crash** — taken end-to-end through the SDD
artifacts, so you can see the method populated rather than abstract. It is the example threaded
through [docs/04](../docs/04-from-needs-to-spec.md).

```
examples/
└── specs/
    └── 0001-cart-persistence/
        ├── spec.md            ← the contract (start here)
        ├── design.md          ← how it's built
        ├── tasks.md           ← decomposition
        └── traceability.md    ← need → req → test → code
```

Read in order: **spec → design → tasks → traceability**. Each is a real, filled-in instance of
the matching file in [`../templates/`](../templates/). The constitution that governs it would
live at the repo root (`.specify/constitution.md`); see
[`../templates/constitution.md`](../templates/constitution.md).

> This example shows a *standard-risk* feature — the full bundle. A one-line fix would collapse to
> just a rich PR description with testable acceptance criteria (see
> [docs/03 §7](../docs/03-artifacts.md#7-the-minimum-viable-artifact-set)).
