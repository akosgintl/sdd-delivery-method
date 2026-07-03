# Project Constitution — ShopCo Web

> Non-negotiable rules every spec and implementation honors. Short, stable, enforceable.

```yaml
---
type: constitution
version: 1.2.0
ratified: 2026-01-15
last_amended: 2026-05-02
owner: architecture-guild
---
```

## 1. Engineering principles
- **P-1:** Every feature ships with automated tests that verify its spec; each `FR-*`/`NFR-*` has ≥1 test.
- **P-2:** Library-first: capabilities are built as standalone, independently testable modules before UI wiring.
- **P-3:** Specs are reviewed and `ready` before construction begins.

## 2. Architectural constraints
- **A-1:** All inter-service communication goes through the API gateway.
- **A-2:** The presentation layer never accesses the database directly.

## 3. Quality bars
- **Q-1:** Test coverage ≥ 80% on changed lines; every `FR-*`/`NFR-*` has a verifying test.
- **Q-2:** Accessibility: WCAG 2.2 AA for all user-facing surfaces.
- **Q-3:** Performance budget: p95 API latency < 300 ms at 1000 RPS.
- **Q-4:** Security & privacy: dependency, secrets, and SAST scans pass with no high/critical; no
  payment-card data (PAN) stored at rest.

## 4. Technology constraints
- **T-1:** Approved backend languages: Go, TypeScript. Approved frontend: TypeScript/React.
- **T-2:** Banned: GPL-licensed dependencies in shipped code.
- **T-3:** PII stays in the EU region; retention ≤ 90 days unless a spec states a longer legal basis.

## 5. Process rules
- **R-1:** Each behavior change updates its `spec.md` in the **same** pull request.
- **R-2:** Significant or hard-to-reverse decisions are recorded as ADRs.
- **R-3:** Work passes the Definition of Ready before build and the Definition of Done before done.

## 6. Amendments & exceptions
- Amending this constitution requires review by the architecture guild + 1 engineering lead, and a
  version bump.
- Exceptions to any clause are recorded as an ADR stating the clause, the reason, the scope, and an
  expiry/review date.

---
*Every per-feature spec inherits these rules; specs note only explicit exceptions (with an ADR).*
