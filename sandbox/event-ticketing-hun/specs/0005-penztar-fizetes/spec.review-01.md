---
artifact: spec
target: ./spec.md
round: 1
reviewed: 2026-07-04
verdict: changes-requested
---

# Review of ./spec.md — round 01

## Findings

- [MAJOR] FR-1 · compound: két külön aggregátumot érintő "és"-joined behaviour (a Kosár összes Foglalásának `converted`-re állítása + `paid` Rend létrehozása) · spec.md:34
  rule: EARS / one behavior per FR (../_shared/ears.md — a "convert the Hold and route to checkout" mintaeset)
  fix: bontsd külön FR-re: (a) a Foglalások `converted`-re állítása, (b) `paid` Rend létrehozása. Az "egyetlen atomi művelet" invariánst már az NFR-3 hordozza, így a szétbontás nem veszít semmit.
  resolved: [x]

- [MAJOR] FR-4 · két különálló Ha/akkor szabály egy FR-ben: (1) lejárt/nem-active Foglalás → fizetés elutasítása terhelés nélkül, (2) ha a szolgáltató mégis terhelt → automatikus visszatérítés · spec.md:43
  rule: EARS / one behavior per FR — unwanted-behavior patterns split (../_shared/ears.md)
  fix: bontsd két FR-re. FR-4a: "Ha a Kosár bármely Foglalása lejárt vagy már nem `active`, akkor ... el kell utasítania a véglegesítést és nem szabad megterhelnie a vásárlót." FR-4b: "Ha a szolgáltató a véglegesítés elutasítása ellenére terhelt, akkor ... automatikus visszatérítést kell indítania (`0007/FR-1`)."
  resolved: [x]

- [MAJOR] NFR-1 · nincs elfogadási kritérium a p95 < 2000 ms követelményre · spec.md:53
  rule: acceptance 1:1 to requirements (../_shared/gherkin.md)
  fix: adj hozzá NFR-1-et idéző kritériumot (100 fizetés/s mellett p95 < 2000 ms, a szolgáltató válaszidejét is beleértve).
  resolved: [x]

- [MAJOR] NFR-2 · nincs elfogadási kritérium a "teljes PAN nem tárolódik" követelményre · spec.md:55
  rule: acceptance 1:1 to requirements (../_shared/gherkin.md)
  fix: adj hozzá NFR-2-t idéző kritériumot (nyugalmi tárolás átvizsgálva; csak a szolgáltató tokenje jelenik meg, teljes kártyaszám sehol).
  resolved: [x]

- [MINOR] FR-2 · compound: eredmény visszaadása + Foglalások `active`-ban tartása + Rend nem-létrehozása egy mondatban · spec.md:38
  rule: EARS / one behavior per FR (../_shared/ears.md)
  fix: fontold meg a szétbontást: a `payment_failed` eredmény és a "nem szabad Rendet létrehoznia" prohibíció egy behaviour (megengedett), de a "Foglalások megtartása a lejáratig" külön FR-ként tisztább.
  resolved: [x]

- [MINOR] FR-3 · "legfeljebb egy terhelést és legfeljebb egy Rendet szabad létrehoznia" — két különálló korlát + permission-jellegű "szabad" EARS obligáció helyett · spec.md:41
  rule: EARS obligation keywords / one behavior per FR (../_shared/ears.md)
  fix: fogalmazd prohibícióként ("nem szabad egynél több terhelést ... egynél több Rendet ..."), vagy bontsd két FR-re (idempotens terhelés; idempotens Rend).
  resolved: [x]

- [MINOR] §7 · "függőben lévőként kell kezelnie" — banned word "kezel" (handle) egy normatív élhelyzet-sorban · spec.md:84
  rule: banned-words (../_shared/banned-words.md)
  fix: konkretizáld (pl. "`pending` állapotban kell tartania a fizetést a webhook beérkeztéig"), a "kezel" ige elhagyásával.
  resolved: [x]

- [MINOR] §9 · "a szolgáltató támogatja az idempotencia-kulcsot" — banned word "támogat" (support) egy feltételezésben (nem normatív) · spec.md:97
  rule: banned-words (../_shared/banned-words.md)
  fix: fogalmazd konkrétan (pl. "a szolgáltató elfogadja az `Idempotency-Key` fejlécet és webhookot küld a végállapotról"), a "támogat" elhagyásával.
  resolved: [x]
