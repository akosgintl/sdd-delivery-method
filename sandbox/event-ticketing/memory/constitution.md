# Project Constitution — Tessera (event-ticketing platform)

> Non-negotiable rules every spec and implementation honors. Short, stable, enforceable.

```yaml
---
type: constitution
version: 1.0.0
ratified: 2026-07-03
last_amended: 2026-07-03
owner: architecture-guild
---
```

## 1. Engineering principles
- **P-1:** Every feature ships with automated tests that verify its spec; each `FR-*`/`NFR-*` has ≥1
  test named for the requirement it verifies (`__FR<n>` / `@FR-<n>`). *(Checked: CI coverage gate + traceability matrix.)*
- **P-2:** Money and inventory operations are **idempotent** (see glossary): a retried request with the
  same idempotency key produces at most one charge and one seat allocation. *(Checked: integration test per money/inventory endpoint.)*
- **P-3:** Specs are reviewed and `ready` (pass Definition of Ready) before construction begins. *(Checked: DoR gate in PR.)*

## 2. Architectural constraints
- **A-1:** Seat inventory is mutated only through the inventory service; no other service writes seat
  state directly. *(Checked: architecture review + DB grants.)*
- **A-2:** The presentation layer never accesses the database directly; it calls the API tier. *(Checked: dependency-graph lint in CI.)*
- **A-3:** All payment provider calls go through the billing adapter; no feature calls a PSP SDK
  directly. *(Checked: import-boundary lint in CI.)*

## 3. Quality bars
- **Q-1:** Test coverage ≥ 80% on changed lines; every `FR-*`/`NFR-*` has a verifying test. *(Checked: CI coverage gate.)*
- **Q-2:** Accessibility: WCAG 2.2 AA for all attendee-facing surfaces. *(Checked: axe CI scan + manual audit per release.)*
- **Q-3:** Performance budget: p95 API latency < 300 ms at 500 RPS; seat-hold and checkout endpoints
  p99 < 800 ms. *(Checked: load test in CI perf stage.)*
- **Q-4:** Security & privacy: dependency, secrets, and SAST scans pass with no high/critical
  findings; the system stores **no** payment-card primary account numbers (PAN) at rest. *(Checked: CI security stage.)*

## 4. Technology constraints
- **T-1:** Approved backend languages: Go, TypeScript. Approved frontend: TypeScript/React. *(Checked: code review.)*
- **T-2:** Banned: GPL-licensed dependencies in shipped code. *(Checked: license scanner in CI.)*
- **T-3:** Attendee PII stays in the EU region; retention ≤ 24 months after the event date unless a
  spec cites a longer legal basis. *(Checked: data-catalog review + retention job.)*

## 5. Process rules
- **R-1:** Each behavior change updates its `spec.md` in the **same** pull request. *(Checked: DoD gate.)*
- **R-2:** Significant or hard-to-reverse decisions are recorded as ADRs. *(Checked: design review.)*
- **R-3:** Work passes the Definition of Ready before build and the Definition of Done before it is
  called complete. *(Checked: DoR/DoD gates.)*

## 6. Amendments & exceptions
- Amending this constitution requires review by the architecture guild + 1 engineering lead, and a
  version bump.
- Exceptions to any clause are recorded as an ADR stating the clause, the reason, the scope, and an
  expiry/review date.

---
*Every per-feature spec inherits these rules; specs note only explicit exceptions (with an ADR).*
