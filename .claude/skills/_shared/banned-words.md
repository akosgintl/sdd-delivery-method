# Banned vague words (bundled reference)

In any **normative** statement (a requirement, NFR, acceptance criterion, or constitution
principle), the following words are banned. Each must be either replaced by a **measurable
statement** or given a **precise definition in the glossary**. Self-contained reference.

> **Output language.** This list is English. If the artifact is authored in another natural language,
> also read `../_shared/languages/<lang>.md` and apply its banned-word list **in addition to** this
> one. If no pack exists for that language, flag it as a `[MAJOR]` process risk rather than silently
> passing non-English normative text (a vague word you cannot see is a vague word you cannot catch).

## The list

> fast, slow, easy, intuitive, robust, secure, scalable, flexible, support, handle, manage,
> appropriate, etc., and/or, optimize, user-friendly

Related open-ended offenders to treat the same way: *reliable, performant, seamless, efficient,
lightweight, as needed, where appropriate, and so on, TBD*.

## The rule

- **Quantify NFRs with numbers and conditions.** Not "fast" → "p95 latency < 300 ms at 1000 RPS".
  Not "secure" → "encrypted at rest with AES-256; stores no payment-card data". Not "scalable" →
  "sustains 5 000 concurrent sessions with < 1% error rate".
- **A quality without a number is not testable.** If you cannot write a pass/fail check, the word
  is doing the hiding — replace it.
- If a term is genuinely domain-specific and unavoidable (e.g. "idempotent"), **define it in the
  glossary** and link, rather than leaving it to interpretation.

## How a reviewer applies this

1. Scan every normative line (FRs, NFRs, acceptance criteria, principles) for a banned word.
2. For each hit: is it quantified/defined right there? If not → finding.
   - In an NFR or requirement → **MAJOR** (or BLOCKER if it makes the requirement untestable).
   - In prose/context (non-normative) → **MINOR** or ignore.
3. The `fix` is always concrete: supply the number/condition or point to a glossary definition.

## Before → after

- ✗ "The system should handle payment errors gracefully."
- ✓ "**If** the payment provider does not respond within 5 s, **then** the system **shall** cancel
  the attempt and display a retry prompt without charging the customer."

- ✗ "Data must be stored securely."
- ✓ "Persisted data **shall** be encrypted at rest (AES-256) and **shall** contain no
  payment-card data."
