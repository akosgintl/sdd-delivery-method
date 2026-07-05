---
artifact: design
target: ./design.md
round: 1
reviewed: 2026-07-04
verdict: approved
---

# Review of ./design.md — round 1

Independent design review against `./spec.md` (FR-1..FR-9, NFR-1..NFR-3) and
`../../adr/0003-visszavaltas-keszletfelszabaditas-atomicitas.md`.

## Összefoglaló

- Minden spec-követelmény build- ÉS verify-úttal rendelkezik. A §7 teszt-stratégia mind a 9 FR-t
  (FR-1..FR-9) és mind a 3 NFR-t (NFR-1..NFR-3) néven nevezi. Hiánytalan lefedettség.
- §2 megtartja az elvetett alternatívákat indokkal (kontingens csak megerősítés után, szinkron
  visszatérítés). ✓
- §3 komponensek felelősségei FR/NFR-ID-kat idéznek (RefundService, QuotaStore, RefundOutbox,
  EventCancellationHandler). ✓
- §4 interfészek és §5 adatmodell FR/NFR-ID-kra hivatkoznak (POST /refund; Event.refund_rate FR-2;
  atomi tranzakció NFR-2). ✓
- §9 kivezetés, visszaállítás (`refunds_enabled` flag), megfigyelhetőség (atomicitás-őr NFR-2) megtervezve. ✓
- `ADR-0003` linkelve, a fájl létezik. ✓
- Nincs a specifikációban nem szereplő új viselkedés; a jegy-void és a tömeges visszatérítés a
  spec FR-1/FR-5-ből ered. ✓
- Front-matter teljes; `spec: ./spec.md` felold; `status: draft` legális. ✓

## Findings

- [MINOR] §4 · külső ID-drift: a design az `event.cancelled` forrását `0001/FR-8`-nak nevezi, a spec FR-5 viszont `0001/FR-6`-ot idéz · design.md:41
  rule: stable IDs — the same ID string must be referenced consistently (../_shared/conventions.md)
  fix: a spec §4 FR-5 kimondja, hogy az esemény `cancelled`-be állítása `0001/FR-6`, a design §4 mégis `0001/FR-8`-ra hivatkozik. Egyeztesd a kettőt: idézd ugyanazt az upstream ID-t (valószínűleg `0001/FR-6`), vagy ha a `0001` külön eseménykibocsátó FR-t definiál, jelöld pontosan. (A `0001` nem volt olvasható e review során, ezért MINOR.)
  resolved: [x]
