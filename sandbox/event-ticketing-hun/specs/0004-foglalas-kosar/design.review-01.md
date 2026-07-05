---
artifact: design
target: ./design.md
round: 1
reviewed: 2026-07-04
verdict: approved
---

# Review of ./design.md — round 1

Independent design review against `./spec.md` (FR-1..FR-11, NFR-1..NFR-3) and
`../../adr/0001-foglalas-lejarat-strategia.md`.

## Összefoglaló

- Minden spec-követelmény build- ÉS verify-úttal rendelkezik. A §7 teszt-stratégia mind a 11 FR-t
  (FR-1..FR-11) és mind a 3 NFR-t (NFR-1..NFR-3) néven nevezi. Hiánytalan lefedettség.
- §2 megtartja az elvetett alternatívákat indokkal (Redis lock, csak ülőhely-soros). ✓
- §3 komponensek felelősségei FR-ID-kat idéznek (HoldService, QuotaStore, ExpirySweeper,
  IdempotencyStore). ✓
- §4 interfészek és §5 adatmodell FR-ID-kra hivatkoznak. ✓
- §9 kivezetés, visszaállítás (feature flag), megfigyelhetőség megtervezve. ✓
- `ADR-0001` linkelve, a fájl létezik. ✓
- Nincs a specifikációban nem szereplő új viselkedés (a design HOGYAN, nem MI). ✓
- Front-matter teljes; `spec: ./spec.md` felold; `status: draft` legális. ✓

## Findings

- [MINOR] FR-5 · nincs explicit build-út: egyetlen komponens/interfész sem idézi az FR-5-öt · design.md:30-44
  rule: design checklist — "Interfaces and data model link back to the requirements they realize" (../_shared/design/checklist.md)
  fix: az FR-5 (aktív Foglalás jegyeinek kizárólagos lekötése) verify-útja megvan (§7 konkurrens teszt), de a build-út csak implicit a QuotaStore CAS-ában. Tedd explicitté: a QuotaStore vagy a HoldService felelősség-sorában idézd az FR-5-öt (pl. „az `active` Holdban lekötött darabszám a CAS-számlálót foglalja — FR-5, FR-10").
  resolved: [x]

- [MINOR] FR-2 · a 10 perces lejárat beállítása implicit, komponens nem idézi · design.md:31-37
  rule: design checklist — components/interfaces cite the requirement they realize (../_shared/design/checklist.md)
  fix: az FR-2 verify-útja megvan (§7 egységteszt), de a §3 nem nevezi meg, ki állítja az `expires_at = created_at + 10 perc` értéket. Add a HoldService felelősségéhez az FR-2-t.
  resolved: [x]
