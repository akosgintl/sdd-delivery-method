---
artifact: spec
target: ./spec.md
round: 2
reviewed: 2026-07-04
verdict: approved
---

# Review of ./spec.md — round 2

Független round-2 re-review friss szemmel. Magyar EARS-adaptáció. A round-1 hét MAJOR és két MINOR
findingjének ellenőrzése + új-defekt keresés a `checklist.md`, `ears.md`, `gherkin.md`,
`banned-words.md`, `conventions.md` alapján. Nincs feloldatlan BLOCKER/MAJOR.

## Findings

- [MAJOR] FR-1 · round-1: háromszoros compound (Foglalás létrehozása + kontingens csökkentése +
  lejárati időbélyeg) · spec.md:33-35, 55-56
  rule: ears.md — compound joining distinct behaviors
  fix: FR-1 most csak az `active` Foglalás létrehozása lejárati időbélyeggel; a kontingens-csökkentés
  külön FR-10-be került (line 55-56). A gherkin FR-1-et és FR-10-et együtt idézi (line 69).
  Ellenőrizve: feloldva.
  resolved: [x]

- [MAJOR] FR-3 · round-1: compound (`expired` állapot + kontingens visszanövelése) · spec.md:38-39,
  57-58
  rule: ears.md — compound
  fix: FR-3 most csak az `expired` állapotváltás; a kontingens-visszaírás FR-11-be került (line 57-58).
  Gherkin FR-3-at és FR-11-et együtt idézi (line 74). Ellenőrizve: feloldva.
  resolved: [x]

- [MAJOR] FR-6 · round-1: compound (`cancelled` állapot + kontingens visszanövelése) · spec.md:45-46,
  57-58
  rule: ears.md — compound
  fix: FR-6 most csak a `cancelled` állapotváltás; a visszaírást FR-11 (`expired` VAGY `cancelled`)
  fedi. A checklist-sor FR-6-ot és FR-11-et együtt idézi (line 84-85). Ellenőrizve: feloldva.
  resolved: [x]

- [MAJOR] FR-7 ↔ §6 · round-1: belső ellentmondás (max 9 vs. 11. elutasítva) · spec.md:47-48, 86-87
  rule: internal consistency (../../../.claude/skills/_shared/spec/checklist.md)
  fix: FR-7 most "10 fölé emelné → elutasítás" (azaz max 10 engedett), összhangban a "11. jegy
  elutasítva" kritériummal (line 86-87). Ellenőrizve: feloldva.
  resolved: [x]

- [MAJOR] FR-2 · round-1: nem volt kritérium a 10 perces lejáratra · spec.md:36-37, 91
  rule: gherkin 1:1
  fix: bekerült FR-2-t idéző kritérium ("lejárati időbélyeg pontosan létrehozás + 10 perc", line 91).
  Ellenőrizve: feloldva.
  resolved: [x]

- [MAJOR] NFR-1 · round-1: nem volt kritérium a p95 < 500 ms-ra · spec.md:61, 92-93
  rule: gherkin 1:1
  fix: bekerült NFR-1-et idéző terheléses kritérium (line 92-93). Ellenőrizve: feloldva.
  resolved: [x]

- [MAJOR] NFR-2 · round-1: nem volt kritérium a "≤5 s felszabadítás"-ra · spec.md:62-63, 94-95
  rule: gherkin 1:1
  fix: bekerült NFR-2-t idéző kritérium (line 94-95). Ellenőrizve: feloldva. (NFR-3-at a
  versenyhelyzet-gherkin idézi, line 79.)
  resolved: [x]

- [MINOR] FR-9 · round-1: permission-jellegű "szabad" EARS-kötelezés helyett · spec.md:52-54
  rule: ears.md obligation keywords
  fix: átfogalmazva "kell" kötelezéssel ("legfeljebb egy állapotváltozást kell végrehajtania"); a
  soft "szabad" eltűnt, a számszerű korlát tesztelhető. Ellenőrizve: feloldva.
  resolved: [x]

- [MINOR] §9 · round-1: banned word "megbízható" a feltételezésben · spec.md:114-115
  rule: banned-words
  fix: átfogalmazva "pontos, szinkronizált időforráshoz igazított"; a "megbízható" eltűnt.
  Ellenőrizve: feloldva.
  resolved: [x]

## Notes (not findings)

- Új-defekt keresés: FR-1..FR-11 mind valid EARS helyes magyar kulcsszóval; FR-4 és FR-8 a
  szankcionált "shall X and shall not Y" egy-egy viselkedésről (elutasítás; konverzió) — rendben;
  FR-5 és FR-11 többes trigger egy válaszra — elfogadható. FR-2 az `ADR-0001`-re defer a fix-vs-
  állítható döntéssel, miközben maga 10 percet rögzít — ez helyes ADR-hivatkozás, nem rejtett nyitott
  kérdés (és a status `draft`).
- Minden FR (1-11) ÉS minden NFR (1-3) rendelkezik ≥1 ID-t idéző kritériummal; egyetlen kritérium sem
  hivatkozik nemlétező FR-re.
- §10 üres; front-matter ép; nincs új tiltott szó normatív sorban; `0002`/`0005`/`ADR-0001` cross-refek
  a helyükön.
- Nincs bevezetett új BLOCKER/MAJOR. Verdict: approved.
