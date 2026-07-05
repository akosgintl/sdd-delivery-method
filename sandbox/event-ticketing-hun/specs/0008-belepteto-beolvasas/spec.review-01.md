---
artifact: spec
target: ./spec.md
round: 1
reviewed: 2026-07-04
verdict: changes-requested
---

# Review of ./spec.md — round 01

## Findings

- [MAJOR] FR-6 · no §6 acceptance criterion cites FR-6; the logging requirement is unverified in the acceptance section · spec.md:44
  rule: gherkin 1:1 — a requirement with no verifying criterion is a defect (../_shared/gherkin.md); spec checklist
  fix: add a §6 checklist line, pl. "Minden beolvasási kísérlet naplózódik a jegyazonosítóval, az eredménnyel, az eszközazonosítóval és az időbélyeggel (FR-6)". (A §7 „Ismételt szinkron" csak említi FR-6-ot, nem elfogadási kritérium.)
  resolved: [x]

- [MAJOR] FR-4 · compound: the offline flow joins three distinct behaviors ("hitelességet ... eldöntenie" + "`used` jelölést lokálisan ... rögzítenie" + "hálózat visszatértekor szinkronizálnia") under one FR · spec.md:38
  rule: EARS singular — one behavior per FR (../_shared/ears.md)
  fix: bontsd külön FR-ekre (offline hitelesség-döntés; lokális `used`-rögzítés; késleltetett szinkron). NFR-2 és NFR-3 már hordozza az idő-korlátokat, így a bontás nem veszít invariánst; a késleltetett szinkron különösen külön pass/fail-t kíván.
  resolved: [x]

- [MINOR] FR-4 · "Ahol" (Where = optional-feature) used for a runtime state (az eszköz offline üzemmódban van) · spec.md:38
  rule: EARS pattern choice — state → "Amíg"/While, not "Ahol"/Where (../_shared/ears.md)
  fix: fogalmazd állapot-vezérelt formában: "Amíg a beléptető eszköz offline üzemmódban van, a rendszernek ...". (Alacsony bizonyosság — az offline képesség opcionális feature-ként is olvasható; ezért MINOR.)
  resolved: [x]

- [MINOR] FR-3 · If/Then trigger ORs two distinct invalid conditions ("aláírása érvénytelen, vagy a jegy `void`") under one ID · spec.md:36
  rule: EARS singular (../_shared/ears.md)
  fix: elfogadható, mert a válasz azonos („érvénytelen jegy"); ha külön pass/fail granularitás kell, bontsd két FR-re (érvénytelen aláírás; `void` jegy).
  resolved: [x]

- [MINOR] NFR-1 · no §6 acceptance criterion cites NFR-1 (online beolvasás p95 < 300 ms) · spec.md:48
  rule: gherkin 1:1 (../_shared/gherkin.md)
  fix: add a §6 checklist line, pl. "Online beolvasás érvényesítése p95 < 300 ms 100 beolvasás/mp mellett (NFR-1)". (NFR-2 és NFR-3 már hivatkozott a §6-ban.)
  resolved: [x]
