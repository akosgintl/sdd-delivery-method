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
`ears.md`, `gherkin.md`, `banned-words.md`, `conventions.md`. Egy MAJOR blokkolja az approve-ot.

## Findings

- [MAJOR] §6 Elfogadási kritériumok · sem az NFR-1-hez, sem az NFR-2-höz nincs elfogadási kritérium ·
  spec.md:50-67
  rule: gherkin 1:1 (../../../.claude/skills/_shared/gherkin.md) — "Every FR/NFR has ≥ 1 acceptance
  criterion"; spec checklist — "a requirement with no verifying criterion [MAJOR]"
  fix: adj mindkét NFR-hez legalább egy mérhető kritériumot, pl. "- [ ] Az elérhető kontingens
  lekérdezésének p95 válaszideje < 200 ms 1000 RPS terhelési teszt alatt (NFR-1)."; "- [ ] A
  Jegytípus-szerkesztő automatizált WCAG 2.2 AA ellenőrzése hiba nélkül fut le (NFR-2)."
  resolved: [x]

- [MINOR] FR-1 · borderline compound: Jegytípus létrehozása + egyedi azonosító hozzárendelése egy
  ID alatt · spec.md:31-32
  rule: ears.md — compound (az azonosító-hozzárendelés vitatottan a létrehozás része)
  fix: elfogadható egyben tartani (az identitás intrinzik a létrehozáshoz); ha külön ellenőrzést
  akarsz az azonosítóra, bontsd külön FR-re.
  resolved: [x]

- [MINOR] §3 Nem-célok · tiltott szó "kezeli" a nem-cél prózájában ("a `0003` kezeli a
  helyfoglalás–jegytípus párosítást") · spec.md:28
  rule: banned-words (../../../.claude/skills/_shared/banned-words.md) — „kezel" (nem-normatív
  próza → MINOR)
  fix: fogalmazd konkrétabban, pl. "a helyfoglalás–jegytípus párosítást a `0003` definiálja".
  resolved: [x]

## Notes (not findings)

- Front-matter teljes; `status: draft` legális; `updated` friss; `need` felfelé a PRD §6-ra mutat.
- §10 Nyitott kérdések üres.
- FR-1..FR-6 EARS-formában, helyes magyar kulcsszóval; FR-4 (Amíg=While) és FR-6 (ubikvitusz)
  helyesek; FR-5 a szankcionált "shall X and shall not Y" egy viselkedésről (ármódosítás) — rendben;
  FR-2 "új vagy módosított" trigger egy viselkedésre (elutasítás) — nem compound.
- NFR-1/2 számszerűsített; nincs tiltott szó normatív sorban; cél/nem-cél megvan; §7 lefedi a
  konkurrens/határeseteket; a `0001`/`0004` cross-refek valósak a PRD feature-táblája szerint.
