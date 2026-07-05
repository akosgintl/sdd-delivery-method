# Hungarian language pack (`hu`)

Localized normative tokens for Hungarian-language artifacts. Apply **in addition to** `ears.md`,
`banned-words.md`, and `gherkin.md` — this pack supplies the parallel Hungarian tokens; it does not
replace the English rules. Validated in the `event-ticketing-hun` test-run.

## 1. EARS — the five patterns in Hungarian

Obligation: `kell` = shall · `nem szabad` / `nem lehet` = shall not.
Banned soft modals (negotiable — reject in a normative line): `kellene` = should, `lehet` = may,
`tudná` = could.

| # | Pattern | Keyword (HU / EN) | Shape |
|---|---------|-------------------|-------|
| 1 | Ubiquitous (Univerzális) | *(none)* | A(z) `<rendszer>`-nek `<válasz>` **kell**. |
| 2 | Event-driven (Eseményvezérelt) | **Amikor** / When | Amikor `<esemény>`, a(z) `<rendszer>`-nek `<válasz>` **kell**. |
| 3 | State-driven (Állapotvezérelt) | **Amíg** / While | Amíg `<állapot>`, a(z) `<rendszer>`-nek `<válasz>` **kell**. |
| 4 | Optional-feature (Opcionális) | **Ahol** / Where | Ahol `<funkció adott>`, a(z) `<rendszer>`-nek `<válasz>` **kell**. |
| 5 | Unwanted (Nemkívánt) | **Ha … akkor** / If…Then | Ha `<esemény>`, akkor a(z) `<rendszer>`-nek `<válasz>` **kell**. |

> **Common reviewer catch:** a runtime *state* condition mis-tagged `Ahol` (Where / optional feature)
> when it should be `Amíg` (While / state-driven). Match the keyword to the *nature* of the condition,
> not to surface phrasing.

## 2. Sanctioned compound

The one allowed compound (a prohibition pairing about the **same** behavior) —
"shall X and shall not Y" reads:

> `<X>` **kell** és `<Y>` **nem szabad**.

Joining two *distinct behaviors* under one `FR` remains a `[MAJOR]` split, exactly as in English.

## 3. Banned vague words (Hungarian)

In any normative line (FR, NFR, acceptance criterion, constitution principle), replace each with a
number + condition, or define it in the glossary:

> gyors, lassú, könnyű, egyszerű, intuitív, robusztus, biztonságos, skálázható, rugalmas, támogat,
> kezel, megfelelő, „stb.", „és/vagy", optimalizál, felhasználóbarát, megbízható, hatékony,
> gördülékeny, „szükség szerint", „ahol szükséges", TBD

(These are the Hungarian equivalents of `banned-words.md`'s English offenders; the same severity rules
apply — MAJOR in a normative line, MINOR/ignore in prose.)

## 4. Acceptance criteria (Gherkin) in Hungarian

| Gherkin (EN) | Hungarian |
|--------------|-----------|
| Given | **Adott** |
| When | **Amikor** |
| Then | **Akkor** |
| And | **És** |

The `# verifies FR-<n>` / `(FR-1)` requirement-ID annotation stays exactly as in English.

## 5. Stays English (machine vocabulary — never localize)

Front-matter keys; status values (`draft, in-review, ready, in-progress, done, superseded, agreed,
proposed, accepted`); IDs (`FR-<n>, NFR-<n>, T-<n>, ADR-NNNN`); cross-references (`0007/FR-1`);
test-naming (`__FR2`, `@requirement(...)`, `@FR-3`).
