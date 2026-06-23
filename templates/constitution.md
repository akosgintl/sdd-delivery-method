# Project Constitution

> The non-negotiable principles every specification and implementation in this project must
> honor. Keep this short and stable. Amendments are themselves reviewed (see §6).
> See [docs/01-principles.md](../docs/01-principles.md) for the rationale.

```yaml
---
type: constitution
version: 1.0.0
ratified: YYYY-MM-DD
last_amended: YYYY-MM-DD
owner: <eng-leadership / architecture>
---
```

## 1. Engineering principles
- **P-1:** <e.g. Every feature ships with automated tests that verify its spec.>
- **P-2:** <e.g. Library-first: capabilities are built as standalone, independently testable
  modules before being wired into the app.>
- **P-3:** <e.g. Specs are reviewed and Ready before construction begins.>

## 2. Architectural constraints
- **A-1:** <e.g. All inter-service communication goes through the API gateway.>
- **A-2:** <e.g. The presentation layer never accesses the database directly.>

## 3. Quality bars
- **Q-1:** Test coverage ≥ <N>% on changed code; every `FR-*`/`NFR-*` has a verifying test.
- **Q-2:** Accessibility: <e.g. WCAG 2.2 AA> for all user-facing surfaces.
- **Q-3:** Performance budget: <e.g. p95 API latency < 300 ms at <N> RPS>.
- **Q-4:** Security baseline: <e.g. dependency + secrets + SAST scans pass; no high/critical>.

## 4. Technology constraints
- **T-1:** Approved languages/frameworks: <list>.
- **T-2:** Banned dependencies / licenses: <list>.
- **T-3:** Data residency / privacy: <e.g. PII stays in region X; retention ≤ N days>.

## 5. Process rules
- **R-1:** Each behavior change updates its `spec.md` in the **same** pull request.
- **R-2:** Significant or hard-to-reverse decisions are recorded as ADRs.
- **R-3:** Work passes the Definition of Ready before build and the Definition of Done before
  it is called complete.

## 6. Amendments & exceptions
- Amending this constitution requires <e.g. review by the architecture guild + 1 eng lead>.
- Exceptions to any clause must be recorded as an ADR stating the clause, the reason, the scope,
  and the expiry/review date.

---
*Every per-feature spec inherits these rules; specs need not restate them — only note explicit
exceptions (with an ADR).*
