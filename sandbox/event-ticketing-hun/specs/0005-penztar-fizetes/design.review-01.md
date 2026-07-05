---
artifact: design
target: ./design.md
round: 1
reviewed: 2026-07-04
verdict: approved
---

# Review of ./design.md — round 1

Independent design review against `./spec.md` (FR-1..FR-8, NFR-1..NFR-3) and
`../../adr/0002-fizetes-foglalas-atomicitas.md`.

## Összefoglaló

- Minden spec-követelmény build- ÉS verify-úttal rendelkezik. A §7 teszt-stratégia mind a 8 FR-t
  (FR-1..FR-8) és mind a 3 NFR-t (NFR-1..NFR-3) néven nevezi. Hiánytalan lefedettség.
- §2 megtartja az elvetett alternatívákat indokkal (pesszimista zár, 2PC). ✓
- §3 komponensek felelősségei FR/NFR-ID-kat idéznek (CheckoutService, PaymentGatewayAdapter,
  RefundInitiator, OrderStore, Outbox). ✓
- §4 interfészek és §5 adatmodell FR/NFR-ID-kra hivatkoznak (POST /checkout, webhook; PAN-tilalom NFR-2). ✓
- §9 kivezetés, visszaállítás (`checkout_v2` flag), megfigyelhetőség (konzisztencia-őr NFR-3) megtervezve. ✓
- `ADR-0002` linkelve, a fájl létezik. ✓
- Nincs a specifikációban nem szereplő új viselkedés; a webhook a spec §7/§8-ban szerepel. ✓
- Front-matter teljes; `spec: ./spec.md` felold; `status: draft` legális. ✓

## Findings

- [MINOR] §2 · „a szolgáltató nem támogatja" — a „támogat" tiltott-lista szó · design.md:26
  rule: banned-words (../_shared/banned-words.md)
  fix: nem-normatív alternatíva-indoklás, ezért csak nit; a design-próza tolerálja. Pontosíthatod: „a szolgáltató nem kínál kétfázisú commit protokollt". Elhagyható.
  resolved: [x]
