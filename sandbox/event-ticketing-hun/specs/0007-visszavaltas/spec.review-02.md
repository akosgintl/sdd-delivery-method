---
artifact: spec
target: ./spec.md
round: 2
reviewed: 2026-07-04
verdict: approved
---

# Review of ./spec.md — round 02

Re-review after rewrite. Fresh read of spec.md plus verification of the round-01 findings.

## Verification of round-01 findings

- [MAJOR→resolved] FR-1 compound (három viselkedés egy ID alatt) · spec.md:33, 50-53
  Megoldva: a hármas felbontva három egy-viselkedésű FR-re — FR-1 (`void`-ra állítás), FR-8
  (a vásárlónak járó, FR-2 szerint számított összeg visszatérítése), FR-9 (a Jegytípus
  kontingensének +1). Mindegyik önálló Amikor/When trigger + "kell". Az atomicitást NFR-2
  invariánsként őrzi, így a bontás nem veszít garanciát.
  resolved: [x]

- [MINOR→resolved] NFR-1/NFR-2/NFR-3 hiányzó §6 kritérium · spec.md:56-61, 80-85
  Megoldva: §6 három új checklist-sort tartalmaz, egyet-egyet NFR-1 (p95 < 500 ms 50
  visszaváltás/mp), NFR-2 (atomicitás megfigyelhetősége) és NFR-3 (5 percen belüli indulás)
  idézésével.
  resolved: [x]

- [MINOR→resolved] FR-6 összevont hibakezelő FR · spec.md:44-46
  A round-01 tolerálhatónak minősítette; változatlanul egy koherens hibakezelő átmenet, saját
  §6 kritériummal (FR-6). Elfogadható.
  resolved: [x]

- [MINOR→resolved] §9 "támogatja" tiltott szó · spec.md:102
  Megoldva: átfogalmazva "a szolgáltató biztosít részleges és teljes visszatérítést
  idempotencia-kulccsal" — a "biztosít" nem tiltott.
  resolved: [x]

## Round-2 checks (no new defect)

- EARS: FR-1/FR-5/FR-8/FR-9 (Amikor/When), FR-2 (ubiquitous számítási szabály), FR-3/FR-4/
  FR-6/FR-7 (Ha…akkor/If-Then). FR-3, FR-5 (P-... nincs), FR-7 a "shall + shall not" ill.
  egyetlen viselkedést mondják; a szétbontott FR-1/FR-8/FR-9 mind egy-viselkedésűek. Minden
  sor "kell"/"nem szabad", nincs soft ige.
- 1:1 lefedettség: FR-1..FR-9 és NFR-1..NFR-3 mind kap ≥1 kritériumot (§6 forgatókönyv FR-1/
  FR-2/FR-8/FR-9 és FR-3; checklist FR-4..FR-7, NFR-1..NFR-3). Nincs nemlétező FR-re mutató
  kritérium; a §6 forgatókönyv fejlécei (FR-8, FR-9) most már létező ID-kre hivatkoznak.
- §10 üres; front-matter teljes és érintetlen (status `draft`); "idempotencia-kulcs" említése
  helyes (nem defekt); az atomicitás ADR-be halasztása (§11) helyes.

Nincs feloldatlan BLOCKER vagy MAJOR → verdict: approved.
