---
artifact: spec
target: ./spec.md
round: 1
reviewed: 2026-07-04
verdict: changes-requested
---

# Review of ./spec.md — round 1

Független review. Magyar EARS-adaptáció (Amikor=When, Ha…akkor=If/Then, Amíg=While; kell=shall,
nem szabad=shall not). Judged against `../../../.claude/skills/_shared/spec/checklist.md`,
`ears.md`, `gherkin.md`, `banned-words.md`, `conventions.md`. Két MAJOR blokkolja az approve-ot.

## Findings

- [MAJOR] §6 Elfogadási kritériumok · az NFR-1, NFR-2, NFR-3 egyikéhez sincs elfogadási kritérium ·
  spec.md:45-66
  rule: gherkin 1:1 (../../../.claude/skills/_shared/gherkin.md) — "Every FR/NFR has ≥ 1 acceptance
  criterion"; spec checklist — "a requirement with no verifying criterion [MAJOR]"
  fix: adj mindhárom NFR-hez legalább egy mérhető kritériumot, pl. "- [ ] 2000 ülőhelyes térkép
  állapot-lekérdezésének p95 < 300 ms (NFR-1)."; "- [ ] Kiválasztás-megerősítés p95 < 500 ms 200
  párhuzamos kiválasztás/mp terhelésen (NFR-2)."; "- [ ] Az állapotjelzés nem kizárólag színnel
  történik és a térkép átmegy a WCAG 2.2 AA ellenőrzésen (NFR-3)."
  resolved: [x]

- [MAJOR] FR-2 · compound: két különböző viselkedést köt össze "és"-sel — (1) az Ülőhely
  `kivalasztott` állapotba állítása kizárólag az adott vásárlónak, (2) Foglalás kezdeményezése a
  `0004/FR-1` szerint · spec.md:35-37
  rule: ears.md — "Joining two distinct behaviors under one ID … each behavior needs its own
  pass/fail check and its own FR [MAJOR]" (vö. "convert the Hold and route to checkout")
  fix: bontsd két FR-re — FR-2a: "Amikor a vásárló kiválaszt egy `szabad` Ülőhelyet, a rendszernek
  `kivalasztott` állapotba kell állítania kizárólag az adott vásárló számára."; FR-2b: "Amikor egy
  Ülőhely `kivalasztott` állapotba kerül, a rendszernek Foglalást kell kezdeményeznie a `0004/FR-1`
  szerint."; adj mindkettőhöz külön elfogadási kritériumot.
  resolved: [x]

- [MINOR] FR-3 · borderline compound: a választás elutasítása + frissített állapot megjelenítése egy
  ID alatt · spec.md:38-39
  rule: ears.md — compound (a frissített állapot megjelenítése vitatottan az elutasítás megfigyelhető
  válasza)
  fix: elfogadható egyben tartani (egy hiba-válasz), de ha külön ellenőrzést akarsz a frissített
  nézetre, bontsd külön FR-re.
  resolved: [x]

## Notes (not findings)

- Front-matter teljes; `status: draft` legális; `updated` friss; `need` felfelé a PRD §6-ra mutat.
- §10 Nyitott kérdések üres.
- FR-1..FR-5 EARS-formában, helyes magyar kulcsszóval; FR-1 határidő (2 mp), FR-4 (Amíg=While) és
  FR-5 helyesek; FR-5 "lejár vagy törlődik" két trigger egy válaszra — elfogadható.
- NFR-1/2/3 számszerűsített; nincs tiltott szó a normatív FR/NFR sorokban (a §2 "valós idejű" cél
  nem-normatív, és az FR-1 a 2 mp-el kvantifikálja); cél/nem-cél megvan; §7 lefedi a konkurrens- és
  hiba-eseteket; a `0004/FR-1` cross-ref valós a PRD feature-táblája szerint.
