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
  spec.md:61-81
  rule: gherkin 1:1 (../../../.claude/skills/_shared/gherkin.md) — "Every FR/NFR has ≥ 1 acceptance
  criterion"; spec checklist — "a requirement with no verifying criterion [MAJOR]"
  fix: adj mindhárom NFR-hez legalább egy mérhető kritériumot, pl. "- [ ] Az esemény-létrehozás p95
  válaszideje < 300 ms 1000 RPS olvasás / 50 RPS írás terhelési teszt alatt (NFR-1).";
  "- [ ] A szervezői képernyők automatizált WCAG 2.2 AA ellenőrzése hiba nélkül fut le (NFR-2).";
  "- [ ] Egy 400 napja írt auditbejegyzés még visszakereshető (NFR-3)."
  resolved: [x]

- [MAJOR] FR-6 · compound: két különböző viselkedést köt össze "és"-sel — (1) minden kibocsátott
  jegy érvénytelenné jelölése, (2) a visszatérítési folyamat elindítása · spec.md:47-49
  rule: ears.md — "Joining two distinct behaviors under one ID … each behavior needs its own
  pass/fail check and its own FR [MAJOR]"
  fix: bontsd két FR-re — FR-6a: "Amikor a szervező `cancelled` állapotba állít egy eseményt, a
  rendszernek érvénytelenné kell jelölnie az összes kibocsátott jegyét."; FR-6b: "Amikor egy esemény
  `cancelled` állapotba kerül, a rendszernek el kell indítania a visszatérítési folyamatot (lásd
  `0007/FR-1`)."; adj mindkettőhöz külön elfogadási kritériumot.
  resolved: [x]

- [MINOR] FR-4 · borderline compound: állapotváltás (`published`) + vásárlói láthatóvá tétel egy
  ID alatt; a §6 kritérium csak a láthatóságot ellenőrzi, az állapotot nem · spec.md:43-44, 76
  rule: ears.md — compound; gherkin 1:1 (a rejtett második viselkedésnek nincs saját ellenőrzése)
  fix: ha a láthatóság a `published` állapot definíciós következménye, hagyd meg egyben, de adj
  kritériumot az állapotra is; ha külön viselkedés, bontsd két FR-re.
  resolved: [x]

- [MINOR] FR-1 · borderline compound: esemény létrehozása + egyedi, változatlan azonosító
  hozzárendelése egy ID alatt · spec.md:36-38
  rule: ears.md — compound (az azonosító-hozzárendelés vitatottan a létrehozás része)
  fix: elfogadható egyben tartani (az identitás intrinzik a létrehozáshoz), de fontold meg külön
  FR-t a "változatlan azonosító" invariánsra, hogy külön pass/fail check készülhessen rá.
  resolved: [x]

## Notes (not findings)

- Front-matter teljes; `status: draft` legális; `updated` friss; `need` felfelé a PRD §6-ra mutat.
- §10 Nyitott kérdések üres — jó irány a `ready` felé.
- FR-1..FR-7 mind EARS-formában, helyes magyar kulcsszóval és kell/nem szabad kötelezéssel; FR-2 a
  szankcionált "shall X and shall not Y" (elutasít + nem hoz létre) egy viselkedésről — rendben.
- NFR-1/2/3 számszerűsített; nincs tiltott szó a normatív sorokban; cél/nem-cél mindkettő megvan;
  §7 lefedi a határ- és hibaeseteket; `0007/FR-1` cross-ref valid a PRD feature-táblája szerint.
