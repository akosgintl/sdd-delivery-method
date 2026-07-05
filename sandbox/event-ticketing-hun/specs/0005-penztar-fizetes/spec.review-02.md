---
artifact: spec
target: ./spec.md
round: 2
reviewed: 2026-07-04
verdict: approved
---

# Review of ./spec.md — round 2

Független round-2 re-review friss szemmel. Magyar EARS-adaptáció. A round-1 négy MAJOR és négy MINOR
findingjének ellenőrzése + új-defekt keresés a `checklist.md`, `ears.md`, `gherkin.md`,
`banned-words.md`, `conventions.md` alapján. Nincs feloldatlan BLOCKER/MAJOR.

## Findings

- [MAJOR] FR-1 · round-1: compound (Foglalások `converted` + `paid` Rend létrehozása, két aggregátum) ·
  spec.md:34-36, 49-50
  rule: ears.md — compound joining distinct behaviors
  fix: FR-1 most csak a Foglalások `converted`-re állítása (`0004/FR-8`); a `paid` Rend létrehozása
  külön FR-7-be került (line 49-50). A gherkin FR-1/FR-7/FR-5 együtt idézi (line 64). Ellenőrizve:
  feloldva.
  resolved: [x]

- [MAJOR] FR-4 · round-1: két különálló Ha/akkor szabály egy FR-ben (elutasítás + automatikus
  visszatérítés) · spec.md:42-44, 51-52
  rule: ears.md — unwanted-behavior patterns split
  fix: FR-4 most csak a véglegesítés elutasítása terhelés nélkül; a "mégis terhelt → auto visszatérítés"
  külön FR-8-ba került (line 51-52, `0007/FR-1`). Külön kritériumaik: FR-4 line 79, FR-8 line 80.
  Ellenőrizve: feloldva.
  resolved: [x]

- [MAJOR] NFR-1 · round-1: nem volt kritérium a p95 < 2000 ms-ra · spec.md:55-56, 83-84
  rule: gherkin 1:1
  fix: bekerült NFR-1-et idéző terheléses kritérium (line 83-84). Ellenőrizve: feloldva.
  resolved: [x]

- [MAJOR] NFR-2 · round-1: nem volt kritérium a "teljes PAN nem tárolódik"-ra · spec.md:57-58, 85-86
  rule: gherkin 1:1
  fix: bekerült NFR-2-t idéző kritérium (line 85-86). Ellenőrizve: feloldva. (NFR-3-at a line 82
  kritérium idézi.)
  resolved: [x]

- [MINOR] FR-2 · round-1: compound (eredmény + Foglalások megtartása + Rend nem-létrehozása) ·
  spec.md:37-39
  rule: ears.md — one behavior per FR
  fix: egyben tartva mint egy sikertelen-fizetés válasz (a `payment_failed` + a "nem szabad Rendet
  létrehoznia" a szankcionált prohibíció-pár, a "Foglalások active-ban a lejáratig" a nem-konverzió
  megfigyelhető következménye). A rewriter diszkréciója szerint egyben — MINOR, nem blokkol.
  Ellenőrizve: feloldva.
  resolved: [x]

- [MINOR] FR-3 · round-1: permission-jellegű "szabad" EARS-kötelezés helyett · spec.md:40-41
  rule: ears.md obligation keywords
  fix: átfogalmazva prohibícióként ("nem szabad egynél több terhelést és egynél több Rendet
  létrehoznia"); a soft "szabad" eltűnt. Ellenőrizve: feloldva.
  resolved: [x]

- [MINOR] §7 · round-1: banned word "kezel" normatív élhelyzet-sorban · spec.md:89-91
  rule: banned-words
  fix: átfogalmazva "függőben lévőként kell nyilvántartania … a webhook alapján kell véglegesítenie";
  a "kezel" eltűnt. Ellenőrizve: feloldva.
  resolved: [x]

- [MINOR] §9 · round-1: banned word "támogat" a feltételezésben · spec.md:103
  rule: banned-words
  fix: átfogalmazva "a szolgáltató biztosít idempotencia-kulcsot és fizetési webhookot"; a "támogat"
  eltűnt. Ellenőrizve: feloldva.
  resolved: [x]

## Notes (not findings)

- Új-defekt keresés: FR-1..FR-8 mind valid EARS helyes magyar kulcsszóval; FR-3 és FR-4 a szankcionált
  "shall X and shall not Y" egy-egy viselkedésről — rendben; FR-6 ubikvitusz auditnapló — rendben.
- Minden FR (1-8) ÉS minden NFR (1-3) rendelkezik ≥1 ID-t idéző kritériummal; egyetlen kritérium sem
  hivatkozik nemlétező FR-re.
- §10 üres; front-matter ép; nincs új tiltott szó normatív sorban; `0004/FR-8`, `0006/FR-1`,
  `0007/FR-1` cross-refek defer-ként a helyükön (helyes, nem defekt).
- Nincs bevezetett új BLOCKER/MAJOR. Verdict: approved.
