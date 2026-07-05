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

- [MAJOR→resolved] FR-6 hiányzó §6 kritérium · spec.md:45-46, 73-74
  Megoldva: §6 checklist-sor idézi FR-6-ot ("Minden beolvasási kísérlet megjelenik a naplóban
  a jegyazonosítóval, az eredménnyel, az eszköz azonosítójával és az időbélyeggel (FR-6)").
  resolved: [x]

- [MAJOR→resolved] FR-4 compound (három offline viselkedés egy ID alatt) · spec.md:38-41
  Nagyrészt megoldva: a hármasból FR-4 most csak az offline hitelesség-döntést mondja ki
  ("Amíg ... offline ..., a jegy hitelességét az aláírás és a kulcsazonosító alapján kell
  eldöntenie"), a `used`-rögzítés és a szinkron átkerült FR-7-be. Lásd az alábbi round-2
  megjegyzést FR-7 maradék összevontságáról (nem blokkoló).
  resolved: [x]

- [MINOR→resolved] FR-4 "Ahol"/Where helyett futásidejű állapot · spec.md:38
  Megoldva: FR-4 most "Amíg"/While állapot-vezérelt formában van.
  resolved: [x]

- [MINOR→resolved] FR-3 If/Then két érvénytelen feltételt fog össze · spec.md:36
  A round-01 elfogadhatónak minősítette (azonos válasz); változatlan.
  resolved: [x]

- [MINOR→resolved] NFR-1 hiányzó §6 kritérium · spec.md:49, 75-76
  Megoldva: §6 checklist-sor idézi NFR-1-et ("Online beolvasás ... p95 < 300 ms 100
  beolvasás/mp ... (NFR-1)").
  resolved: [x]

## Round-2 findings (new)

- [MINOR] FR-7 · maradék összevontság: "a `used` jelölést lokálisan kell rögzítenie, és a
  hálózat visszatértekor szinkronizálnia kell" — a lokális rögzítés és a késleltetett szinkron
  két megkülönböztethető viselkedés egy ID alatt · spec.md:40-41
  rule: EARS singular — one behavior per FR (../_shared/ears.md)
  fix: opcionálisan bontsd külön FR-be a késleltetett szinkront (saját pass/fail). Nem blokkoló:
  egyetlen offline-jelölési életciklust ír le, a szinkron idő-korlátját NFR-3 hordozza, és a §6
  kritérium (FR-7, NFR-2, NFR-3) mindkét részre ad ellenőrzést — a round-01 3-as bontás fő
  MAJOR-ja fel van oldva, ez maradék finomítás. Alacsony bizonyosság → MINOR.
  resolved: [ ]

- [MINOR] §4 FR-sorrend: FR-7 az FR-4 és FR-5 között jelenik meg · spec.md:40-42
  rule: spec checklist (kozmetikai)
  fix: rendezd az ID-ket növekvő sorrendbe (FR-4, FR-5, FR-6, FR-7). Csak olvashatóság; az ID-k
  stabilak, nem defekt.
  resolved: [ ]

## Round-2 checks (no new blocker/major)

- EARS: FR-1/FR-2/FR-3 (Amikor / Ha…akkor), FR-4/FR-7 (Amíg/While), FR-5 (Ha…akkor), FR-6
  (ubiquitous). Mind "kell"/"nem szabad", helyes kulcsszóval; nincs soft ige.
- 1:1 lefedettség: FR-1..FR-7 és NFR-1..NFR-3 mind kap ≥1 kritériumot; egyetlen kritérium sem
  mutat nemlétező FR-re.
- §10 üres; front-matter teljes és érintetlen (status `draft`); nincs új tiltott vágó szó.

Nincs feloldatlan BLOCKER vagy MAJOR (a két új MINOR nem blokkol) → verdict: approved.
