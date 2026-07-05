# TEST-REPORT — SDD skill-réteg magyar nyelvű teszt-futása

**Téma:** jegyértékesítő rendszer (event ticketing) · **Kimenet nyelve:** magyar · **Skillek nyelve:**
angol (változatlan) · **Dátum:** 2026-07-04 – 2026-07-05 · **Hely:** `sandbox/event-ticketing-hun/`

> Cél: bizonyítani, hogy a `.claude/skills/` SDD pipeline (constitution → glossary → gates →
> problem → PRD → spec → design → tasks → traceability) **magyar nyelvű artifaktokat** tud
> előállítani, miközben a skillek **angolul** maradnak. A korábbi angol futást
> (`sandbox/event-ticketing-eng/`) a felhasználó kérésére **nem olvastuk**; a téma és a döntések
> önállóan születtek.

## 1. Mit állítottunk elő

| Fázis | Artifaktum | Darab | Review-körök |
|-------|------------|------|--------------|
| P0 | constitution, glossary, DoR, DoD | 4 | constitution 1 kör (approved); glossary-lint 1 kör (3 fogalom promótálva) |
| P1 | problem statement, PRD | 2 | problem 1 kör (approved); PRD 2 kör (r1 changes-requested → r2 approved) |
| P2 | spec.md × 9 | 9 | mindegyik 2 kör (r1 changes-requested → r2 approved) |
| P3 | design.md × 3 (0004, 0005, 0007) | 3 | mindegyik 1 kör (approved, MINOR-ök alkalmazva) |
| P3 | ADR × 3 | 3 | mindegyik 1 kör (approved – lásd 5. pont) |
| P4 | tasks.md × 3 | 3 | mindegyik 1 kör (approved – lásd 5. pont) |
| P5 | traceability.md × 3 | 3 | generált; DoD-kapu előtti állapot |
| gate | DoR-kapu × 3 | 3 | pass |

Összes írt artifaktum: 27 fő + 30+ review/gate fájl. A 9 feature: esemény-kezelés, jegytípusok,
helyválasztás, foglalás, pénztár, jegykibocsátás, visszaváltás, beléptetés, riportok. A 3 teljes
csomag (foglalás, pénztár, visszaváltás) a leggazdagabb tervezési döntéseik miatt.

## 2. A magyar nyelvi adaptáció — mi működött

**A pipeline nyelvfüggetlen része gond nélkül átment.** A gépi vokabulárium végig angol maradt, és
ez nem okozott súrlódást:
- front-matter kulcsok (`id, status, owner, need, …`), státusz-értékek (`draft/ready/agreed/…`),
  stabil ID-k (`FR-/NFR-/T-/ADR-`), kereszthivatkozások (`0007/FR-1`), teszt-elnevezés (`__FR1`).

**Az adaptált normatív szabályok a megadott leképezéssel jól érvényesültek** (lásd Függelék):
- **EARS magyarul:** a writerek és a reviewerek helyesen kezelték az `Amikor/Amíg/Ahol/Ha…akkor`
  kulcsszavakat és a `kell` / `nem szabad` kötelezést. A reviewerek pontosan azonosították a
  hibás mintát (pl. `0008/FR-4` tévesen `Ahol`=Where volt egy futásidejű állapotra → `Amíg`=While).
- **Tiltott homályos szavak magyarul:** a reviewerek megtalálták a magyar vague szavakat a normatív
  és a próza-sorokban is: `megbízható`, `támogat`, `kezel`, `gyors`(-tő), `biztonságos` — mind
  javításra került vagy számmal helyettesítve.
- **Gherkin magyarul:** az `Adott / Amikor / Akkor` elfogadási forgatókönyvek működtek, a `# igazolja
  FR-n` leképezés érvényes maradt.

## 3. A legjelentősebb tartalmi finding — nyelvtől független

A domináns, ismétlődő hiba **nem** nyelvi volt, hanem valódi specifikálási tendencia: **compound
FR-ek** — egyetlen FR két különböző aggregátumot mutált (pl. „Foglalás létrehozása **és** a
kontingens csökkentése **és** lejárat rendelése”). A független reviewerek összesen **16 MAJOR**
compound-findinget és **hiányzó NFR-elfogadási kritériumot** jeleztek, ami a specek felét érintette.
Egy egységes rewrite-körben minden atomi FR-re bontottuk (FR-1/FR-10/FR-11 stb.), az atomicitás
invariánsát pedig a megfelelő NFR hordozza. A 2. kör mind a 9 specet **approved**-ra hozta.

Ez a legfontosabb tanulság: a magyar nyelv **nem gyengítette** a review-mélységet — a mély
szerkezeti hibák (compound viselkedés, hiányzó lefedettség, belső ellentmondás, pl. `0004/FR-7`
„10-et elérné” vs. a „11. elutasítva” kritérium) magyarul is előkerültek.

## 4. Súrlódási pontok — ahol az angol tokenek beszűrődtek

1. **Nincs nyelvi kapcsoló a skillekben.** Az EARS-kulcsszavak és a tiltott-szó lista angol
   tokenek. A magyar érvényesítéshez a **magyar leképezést minden reviewer-promptba be kellett
   injektálni** (Függelék). E leképezés nélkül egy reviewer a magyar homályos szavakat és a hibás
   EARS-mintát **csendben átengedte volna** — ez a fő kockázat éles használatnál.
2. **Angol baseline-feltevés a glossary-checklistben** („general English” kontraszt): a
   glossary-maintainer helyesen működött, de a „vitatott vs. köznyelvi” határ magyarra nincs
   kalibrálva; itt emberi ítélet pótolta.
3. **Reviewer-kalibrációs szórás (egészséges jel):** ugyanarra a hiányzó-NFR-kritérium hibára az
   egyik független reviewer **MAJOR**-t, a másik (a `_shared/spec/example.md`-hez kalibrálva)
   **MINOR**-t adott. Ez a reviewer-függetlenség valódi jele, nem nyelvi probléma.

## 5. A futás korlátai (őszinte elszámolás)

- A futás vége felé **session-limit** lépett életbe, ezért az **ADR-reviewek (3), a tasks-reviewek
  (3), a DoR-kapuk (3) és a glossary-promóció self-review / self-check** formában készültek, nem
  külön, független sub-agenttel. A self-review a workflow szerint gyengébb bizonyíték; a független
  ADR/tasks újranézés **nyitott elem**. A P0–P3 (constitution, problem, PRD, mind a 9 spec, mind a
  3 design) ezzel szemben **valódi, külön kontextusú független reviewert** kapott.
- A **DoD-kapu nem futott le**: a `build` a skillek hatókörén kívül esik (a `workflow.md` külön
  jelöli: „build (outside the skills)”). Mivel kód nem készült, a DoD (verifikáció a spec ellen,
  spec-összehangolás) definíció szerint nem teljesíthető — a traceability `Verified` oszlopa nyitott.
  Ez a spec/design/terv szintű teszt várt végállapota.
- A `Kimutatás` / `riport` szinonima-elcsúszást a glosszárium rögzíti (a `Kimutatás` a kanonikus),
  de a `0009` címét és a próza egy részét nem írtuk át, hogy elkerüljük a felesleges churn-t.

## 6. Összegzés

A magyar nyelvű SDD-futás **sikeres**: a pipeline minden fázisa magyar artifaktumokat termelt, a
konvenciók (stabil ID-k, front-matter, státusz-gépek, EARS-mintázat, kvantifikált NFR-ek, 1:1
elfogadás, traceability) sértetlenek maradtak, és a független reviewerek a magyar leképezéssel
ugyanolyan mélységben fogták meg a hibákat, mint angolul tennék. A **fő tanulság**: a skillek
portolhatók más nyelvre, **feltéve, hogy a normatív szabálykészletet (EARS-kulcsszavak,
tiltott-szó lista) lefordítjuk és a reviewer kontextusába tesszük** — enélkül az érvényesítés
csendben elnémul. Javaslat: a `_shared/ears.md` és `_shared/banned-words.md` kapjon egy nyelvi
függeléket, vagy a skillek egy `output_language` paramétert, amely a megfelelő leképezést betölti.

---

## Függelék — Magyar adaptációs szabálykészlet (a reproducibilitásért)

Ezt a leképezést kapta minden writer/reviewer/rewriter. Angol forrás: `.claude/skills/_shared/ears.md`,
`.claude/skills/_shared/banned-words.md`.

### EARS öt minta magyarul
Kötelezettség: `kell` = shall, `nem szabad`/`nem lehet` = shall not. Tiltott lágy szavak:
`kellene`, `lehet`, `tudná` = should/may/could.

| # | Minta | Kulcsszó (HU/EN) | Alak |
|---|-------|------------------|------|
| 1 | Univerzális | *(nincs)* | A(z) `<rendszer>`-nek `<válasz>` kell. |
| 2 | Eseményvezérelt | **Amikor** / When | Amikor `<esemény>`, a(z) `<rendszer>`-nek `<válasz>` kell. |
| 3 | Állapotvezérelt | **Amíg** / While | Amíg `<állapot>`, a(z) `<rendszer>`-nek `<válasz>` kell. |
| 4 | Opcionális | **Ahol** / Where | Ahol `<funkció adott>`, a(z) `<rendszer>`-nek `<válasz>` kell. |
| 5 | Nemkívánt | **Ha … akkor** / If…Then | Ha `<esemény>`, akkor a(z) `<rendszer>`-nek `<válasz>` kell. |

Szankcionált összetett: „shall X and shall not Y” egy viselkedésről → „`<X>` kell és `<Y>` nem szabad”.

### Tiltott homályos szavak (magyar)
gyors, lassú, könnyű, egyszerű, intuitív, robusztus, biztonságos, skálázható, rugalmas, támogat,
kezel, megfelelő, „stb.”, „és/vagy”, optimalizál, felhasználóbarát, megbízható, hatékony,
gördülékeny, „szükség szerint”, „ahol szükséges”, TBD. → mindet számmal+feltétellel kiváltani, vagy
a glosszáriumban definiálni.

### Változatlanul angol (gépi vokabulárium)
front-matter kulcsok; státusz-értékek (`draft, in-review, ready, in-progress, done, superseded,
agreed, proposed, accepted`); ID-k (`FR-<n>, NFR-<n>, T-<n>, ADR-NNNN`); teszt-elnevezés (`__FR2`,
`@requirement(...)`, `@FR-3`). Gherkin: `Adott / Amikor / Akkor` (Given/When/Then).
