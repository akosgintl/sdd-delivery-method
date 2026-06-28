# How to write good non-functional requirements

Non-functional requirements (NFRs) specify the system's **qualities and constraints** — performance,
security, availability, accessibility, privacy, observability, cost — as opposed to *what it does*
([functional requirements](write-ears-requirements.md)). They are the most under-specified and most
production-dangerous part of a spec: nobody forgets that checkout must work, but everybody forgets to
say it must work in under 300 ms for 1,000 concurrent users. An NFR is only worth writing if it has a
**number and a condition**; without those it's a wish that no one can pass or fail. This is the
*doing* companion to [`docs/04 §3.1`](../docs/04-from-needs-to-spec.md#31-functional-vs-non-functional).

> Every NFR must survive one question: **"what's the number, and under what condition?"** "Fast"
> fails it; "p95 < 300 ms at 1,000 RPS" passes. If you can't attach a measurement, you don't yet
> have a requirement.

## When you write it

**Phase 1**, alongside the functional requirements, in §5 of the [spec](write-a-spec.md). Some NFRs
are inherited wholesale from the [constitution](write-a-constitution.md) (the project-wide quality
floor) — don't restate those; reference them, and in the spec write only the NFRs *specific to this
feature* or where this feature must exceed the floor.

## Anatomy: what goes in (and what doesn't)

Walk the "-ilities" checklist and keep the ones that apply to *this* feature:

| Category | Quantify with… |
|----------|----------------|
| Performance | latency percentile + load (p95 < 300 ms at 1,000 RPS) |
| Scalability | a target volume and growth headroom |
| Availability / reliability | an SLO (99.9% monthly), an error-rate budget |
| Security | a named baseline + "no high/critical findings" |
| Privacy / compliance | data classes, retention windows, residency, a named regime |
| Accessibility | a named standard (WCAG 2.2 AA) |
| Usability | a task-completion or time-on-task target |
| Observability | the signals required (logs/metrics/alerts) for the new behavior |
| Maintainability / portability / cost | the explicit budget or constraint |

Each NFR carries a stable ID (`NFR-1`, `NFR-2`, …) and, like an FR, must be **singular** and
**verifiable**. What stays out: behaviors (those are FRs), and project-wide bars already fixed by the
constitution (reference, don't copy).

## The recipe

1. **Walk the checklist, not your memory.** Go category by category and ask "does this feature have a
   constraint here?" The valuable NFRs are the ones you'd otherwise forget.
2. **Attach a number and a condition to every one.** A metric (p95 latency), a threshold (< 300 ms),
   and the condition it holds under (at 1,000 RPS). No condition = not measurable.
3. **Cite the standard instead of inventing words.** "Meets WCAG 2.2 AA", "OWASP ASVS L2", "99.9%
   monthly availability" — named standards are both precise and checkable.
4. **Inherit from the constitution; don't duplicate.** If the project floor already says "p95 < 300
   ms", only write an NFR here when this feature is stricter, or when it adds a constraint the floor
   doesn't cover.
5. **Say how it's verified.** Each NFR should imply a measurement — a load test, a scan, an audit, a
   synthetic check. If you can't name the measurement, the NFR isn't done. (The
   [design](write-a-technical-design.md)'s test strategy ties each NFR to its measurement.)
6. **Scope the condition realistically.** "Within 1 s for carts up to 100 items" is testable;
   "always instant" is not — and is also a lie.

## Before → After

**Adjective → measurable NFR**

> ✗ *Before:* "The system should be fast and able to handle lots of users."
> *("Fast", "handle", "lots" are all banned — there's no value at which a reviewer could fail this.)*
>
> ✓ *After:*
> - **NFR-1:** Cart restoration shall complete within 1 s at p95 for carts of up to 100 items.
> - **NFR-2:** The service shall sustain 1,000 requests/second with an error rate < 0.1%.

**"Secure" → a verifiable security NFR**

> ✗ *Before:* "Cart data must be stored securely." *(Unverifiable — there's no test for "securely".)*
>
> ✓ *After:* "**NFR-3:** Cart data shall be encrypted at rest (AES-256) and in transit (TLS 1.2+);
> PII retention ≤ 90 days; dependency, secrets, and SAST scans pass with no high/critical findings."

## Smell test — rewrite or remove if you see…

- **A bare adjective.** "fast, scalable, robust, secure, user-friendly" with no number — see the
  [banned words](../docs/04-from-needs-to-spec.md#33-words-to-ban-or-define).
- **A number with no condition.** "< 300 ms" — at what load? for which operation? at which
  percentile?
- **No named standard** where one exists (accessibility, security, availability).
- **An NFR that's really an FR.** If it describes a behavior, move it to the functional section.
- **A constitution clause copied in.** Reference the floor; only write feature-specific NFRs here.
- **No way to measure it.** If you can't state the load test / scan / audit that proves it, it's not
  yet a requirement.

## Checklist

- [ ] Every relevant "-ility" was considered, not just performance.
- [ ] Each NFR has a metric, a threshold, and the condition it holds under.
- [ ] Named standards are cited where they exist (WCAG, OWASP, an SLO).
- [ ] Each NFR has a stable `NFR-n` ID and is singular.
- [ ] Each NFR implies a concrete measurement (load test, scan, audit, synthetic check).
- [ ] Project-wide bars are referenced from the constitution, not duplicated.
- [ ] No banned vague words remain.

## Links

- **Template:** [`templates/specification.md`](../templates/specification.md) §5.
- **Concept:** [`docs/04 §3.1` — Functional vs non-functional](../docs/04-from-needs-to-spec.md#31-functional-vs-non-functional)
  and the [qualities of a good requirement](../docs/04-from-needs-to-spec.md#32-the-qualities-of-a-good-requirement).
- **Related:** the project quality floor lives in the [constitution](write-a-constitution.md); NFRs
  are verified at [DoD](write-dor-dod-gates.md) against their stated numbers; behaviors go in
  [EARS requirements](write-ears-requirements.md).
