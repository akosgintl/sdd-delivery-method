# F — SDD Maturity Model

A ladder for locating where a team (or a whole org) actually sits, and what the *next* rung is. It
maps the incremental adoption stages from
[`11 §1`](../11-adoption-and-antipatterns.md#1-how-to-adopt-sdd-incrementally) to observable signals,
so "are we doing SDD?" becomes a question you can answer by looking rather than by opinion. It is the
measurement companion to the enterprise rollout in
[`C — Scaling to the Enterprise`](C-scaling-to-the-enterprise.md).

> Rule of thumb: **levels are earned by behavior, not by artifacts.** A team with twelve templates
> and no enforced gates is Level 1, not Level 5. Climb only when the current rung is paying for
> itself ([value before ceremony](../11-adoption-and-antipatterns.md#1-how-to-adopt-sdd-incrementally)).

## The levels

| Level | Name | Defining behavior | You can observe… | To advance |
|-------|------|-------------------|------------------|------------|
| **0** | Ad-hoc | Intent lives in tickets, chats, and heads | Frequent "what did we mean here?" mid-build; rework from misunderstanding | Write one real `spec.md` with testable requirements; review it before building (Stage 1) |
| **1** | Specs exist | Some features get a spec, but build can start without one | A few good specs; others ignored after writing (gate theatre) | Adopt a lightweight **DoR/DoD** and actually enforce them (Stage 2) |
| **2** | Gates enforced | Work doesn't enter build until Ready, or ship until Done | Work sent back at DoR; DoD blocks "works on my machine" | Standardize storage — numbered `specs/`, front-matter, **stable IDs** (Stage 3) |
| **3** | Conventions & IDs | Specs are discoverable and consistent across the team | Anyone finds the spec for a feature; `FR-3` means one thing | Add the **requirement-coverage CI check** + spec-lint (Stage 4) |
| **4** | Traceability & CI | Quality is self-enforcing in the pipeline | Every `FR-*` has a test; CI flags spec/code drift | Distill the recurring review rules into a **constitution** (Stage 5) |
| **5** | Constitution & agents | Standing law is codified and in the agent's context | New joiners *and* agents build from the spec unaided; the constitution is CI-checked | Hold the line; scale across squads via a paved road (see [`C`](C-scaling-to-the-enterprise.md)) |

## Reading your level honestly

Use the health signals from [`11 §4`](../11-adoption-and-antipatterns.md#4-is-it-working-signals),
not the artifact count:

- **Healthy climb:** falling clarification/rework rates; fewer "what did we mean?" moments; specs
  that newcomers and agents can build from; post-mortems can cite the spec that was (or wasn't) right.
- **False height:** specs written then ignored (you're really at Level 1, [anti-pattern E](../11-adoption-and-antipatterns.md#e-artifacts-without-gates-theater));
  specs consistently stale ([rot, B](../11-adoption-and-antipatterns.md#b-spec-rot-the-silent-killer));
  ballooning specifying time with no fall in rework ([over-ceremony, F](../11-adoption-and-antipatterns.md#f-uniform-ceremony-for-all-work)).
  If you see these, **drop a level and fix the gate** before adding anything.

> The irreducible core that defines "real" SDD (Level 2 and up): **a testable spec per feature, gated
> by Ready and Done, living in the repo.** Everything above that is amplification.

## At org scale

Levels apply per team, but the **platform/enabling team** ([`C §3`](C-scaling-to-the-enterprise.md))
is what lets a second and third squad reach Level 4–5 cheaply: the CI checks, constitution, and agent
harness are built once and consumed everywhere. Track maturity per squad; expect a pilot squad to
reach Level 4–5 before you roll the paved road out.

---

Continue to [`G — Agent Prompt Library`](G-agent-prompt-library.md), or return to the
[README](../../README.md) for the full table of contents.
