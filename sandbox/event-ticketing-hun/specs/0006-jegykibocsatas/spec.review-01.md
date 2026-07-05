---
artifact: spec
target: ./spec.md
round: 1
reviewed: 2026-07-04
verdict: changes-requested
---

# Review of ./spec.md — round 01

## Findings

- [MAJOR] FR-4 ↔ NFR-3 · belső ellentmondás ugyanarról az értékről: FR-4 abszolút "60 másodpercen belül el kell küldenie" (implikált 100%), NFR-3 "kézbesítési kísérlete 60 másodpercen belül az esetek 99%-ában"; §7 szerint az e-mail meg is hiúsulhat és újrapróbál — a 100%-os 60 s nem tartható · spec.md:38, spec.md:49
  rule: internal consistency — two normative lines fix the same value differently (../_shared/spec/checklist.md)
  fix: az FR-4 a viselkedést mondja ki időzítés nélkül ("... el kell küldenie a jegyeket tartalmazó visszaigazolást"), az időzítést/százalékot pedig kizárólag az NFR-3 hordozza. Igazítsd a §6 kritériumot is (99% / kísérlet, nem 100% / kézbesítés).
  resolved: [x]

- [MAJOR] FR-5 · compound + hatókör-szivárgás: `void` állapotra állítás (IssuedTicket) + "a QR-kódját érvénytelenként kell kezelnie a beléptetésnél" — a beléptetés a 0008 hatóköre (§3 nem-cél), és "kezelni" banned word · spec.md:40
  rule: EARS / one behavior per FR + no behavior owned by another feature (../_shared/ears.md, ../_shared/spec/checklist.md)
  fix: tartsd meg csak a saját aggregátumon vett viselkedést: "Amikor egy jegy érvénytelenné válik (`0001/FR-6` vagy `0007/FR-2`), a rendszernek `void` állapotba kell állítania a Kibocsátott jegyet." A `void` QR beléptetéskori elutasítását a `0008` spec mondja ki, ide legfeljebb hivatkozásként.
  resolved: [x]

- [MAJOR] NFR-1 · nincs elfogadási kritérium a "jegygenerálás ≤ 10 s, max 10 jegyig" követelményre · spec.md:45
  rule: acceptance 1:1 to requirements (../_shared/gherkin.md)
  fix: adj hozzá NFR-1-et idéző kritériumot (pl. 10 jegyes `paid` Rend esetén az összes Kibocsátott jegy a `paid` eseménytől ≤ 10 s alatt elkészül).
  resolved: [x]

- [MINOR] FR-5 · a trigger "vagy"-gal két különböző eseményt fog össze (esemény törlése VAGY visszaváltás) · spec.md:40
  rule: EARS trigger specificity (../_shared/ears.md)
  fix: elfogadható egy "jegy érvénytelenné válik" érvénytelenítési eseményosztályként; ha külön pass/fail-t akarsz mindkét forrásra, bontsd két FR-re (`0001/FR-6` és `0007/FR-2` triggerekkel).
  resolved: [x]
