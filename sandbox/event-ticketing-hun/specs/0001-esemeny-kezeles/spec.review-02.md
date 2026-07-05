---
artifact: spec
target: ./spec.md
round: 2
reviewed: 2026-07-04
verdict: approved
---

# Review of ./spec.md — round 2

Független round-2 re-review friss szemmel. Magyar EARS-adaptáció (Amikor=When, Ha…akkor=If/Then,
Amíg=While, no-keyword=ubikvitusz; kell=shall, nem szabad=shall not). A round-1 két MAJOR és két
MINOR findingjének ellenőrzése + új-defekt keresés a `checklist.md`, `ears.md`, `gherkin.md`,
`banned-words.md`, `conventions.md` alapján. Nincs feloldatlan BLOCKER/MAJOR.

## Findings

- [MAJOR] §6 · round-1: NFR-1/2/3 egyikéhez sem volt elfogadási kritérium · spec.md:85-88
  rule: gherkin 1:1 (../../../.claude/skills/_shared/gherkin.md)
  fix: mindhárom NFR-hez bekerült mérhető kritérium (NFR-1 line 85-86, NFR-2 line 87, NFR-3 line 88),
  mind az ID-t idézve. Ellenőrizve: feloldva.
  resolved: [x]

- [MAJOR] FR-6 · round-1: compound (érvénytelenné jelölés + visszatérítés indítása) · spec.md:47-52
  rule: ears.md — compound joining distinct behaviors
  fix: szétbontva FR-6 (érvénytelenné jelölés, line 47-48) és FR-8 (visszatérítési folyamat indítása
  `0007/FR-1`, line 51-52); mindkettő önálló EARS `Amikor…kell` és külön kritériuma van (FR-6 line 81,
  FR-8 line 84). Ellenőrizve: feloldva.
  resolved: [x]

- [MINOR] FR-4 · round-1: állapotváltás + láthatóság egy ID alatt, kritérium csak a láthatóságot mérte ·
  spec.md:43-44, 77-78
  rule: ears.md compound / gherkin 1:1
  fix: az FR-4 kritériuma most az állapotot (`published`) ÉS a vásárlói listában való megjelenést is
  ellenőrzi (line 77-78). Ellenőrizve: feloldva.
  resolved: [x]

- [MINOR] FR-1 · round-1: létrehozás + azonosító-hozzárendelés egy ID alatt · spec.md:36-38
  rule: ears.md compound (intrinzik identitás — egyben tartható)
  fix: egyben tartva, elfogadható (az azonosító a létrehozás intrinzik része). Ellenőrizve: feloldva.
  resolved: [x]

## Notes (not findings)

- Új-defekt keresés: FR-1..FR-8 mind valid EARS helyes magyar kulcsszóval és kell/nem szabad
  kötelezéssel; FR-2 a szankcionált "shall X and shall not Y" egy viselkedésről (elutasít + nem hoz
  létre) — rendben; FR-7 ubikvitusz auditnapló — rendben.
- Minden FR (1-8) ÉS minden NFR (1-3) rendelkezik ≥1 kritériummal, mind az ID-t idézve; egyetlen
  kritérium sem hivatkozik nemlétező FR-re.
- §10 Nyitott kérdések üres; front-matter ép (`status: draft`, `updated: 2026-07-04`, `need` a PRD
  §6-ra mutat); nincs új tiltott szó normatív sorban; `0007/FR-1` cross-ref valid.
- Nincs bevezetett új BLOCKER/MAJOR. Verdict: approved.
