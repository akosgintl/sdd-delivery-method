---
artifact: spec
target: ./spec.md
round: 2
reviewed: 2026-07-04
verdict: approved
---

# Review of ./spec.md — round 2

Független round-2 re-review friss szemmel. Magyar EARS-adaptáció. A round-1 két MAJOR és egy MINOR
findingjének ellenőrzése + új-defekt keresés a `checklist.md`, `ears.md`, `gherkin.md`,
`banned-words.md`, `conventions.md` alapján. Nincs feloldatlan BLOCKER/MAJOR.

## Findings

- [MAJOR] §6 · round-1: NFR-1/2/3 egyikéhez sem volt elfogadási kritérium · spec.md:68-72
  rule: gherkin 1:1 (../../../.claude/skills/_shared/gherkin.md)
  fix: mindhárom NFR kapott mérhető, ID-t idéző kritériumot (NFR-1 line 68, NFR-2 line 69-70, NFR-3
  line 71-72). Ellenőrizve: feloldva.
  resolved: [x]

- [MAJOR] FR-2 · round-1: compound (`kivalasztott` állapot + Foglalás kezdeményezése `0004/FR-1`) ·
  spec.md:35-36, 43-44
  rule: ears.md — compound joining distinct behaviors
  fix: szétbontva FR-2 (`kivalasztott` állapotba állítás kizárólag az adott vásárlónak, line 35-36)
  és FR-6 (Foglalás kezdeményezése `0004/FR-1` szerint, line 43-44); a gherkin forgatókönyv mindkettőt
  idézi (line 56). Ellenőrizve: feloldva.
  resolved: [x]

- [MINOR] FR-3 · round-1: elutasítás + frissített állapot megjelenítése egy ID alatt · spec.md:37-38
  rule: ears.md compound (egy hiba-válasz, egyben tartható)
  fix: egyben tartva mint egy hiba-válasz; elfogadható. Ellenőrizve: feloldva.
  resolved: [x]

## Notes (not findings)

- Új-defekt keresés: FR-1..FR-6 mind valid EARS; FR-1 kvantifikált (≤2 mp), FR-4 (Amíg=While) és FR-5
  ("lejár vagy törlődik" — két trigger egy válaszra) helyesek; FR-2 és FR-6 most külön viselkedések.
- Minden FR (1-6) ÉS minden NFR (1-3) rendelkezik ≥1 ID-t idéző kritériummal (FR-2/FR-6/FR-3/FR-4
  gherkinben, FR-1/FR-5 checklistben); egyetlen kritérium sem hivatkozik nemlétező FR-re.
- §10 üres; front-matter ép; a §2 "valós idejű" cél nem-normatív (az FR-1 kvantifikálja); nincs új
  tiltott szó normatív sorban; `0004/FR-1` cross-ref valós.
- Nincs bevezetett új BLOCKER/MAJOR. Verdict: approved.
