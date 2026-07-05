---
artifact: spec
target: ./spec.md
round: 1
reviewed: 2026-07-04
verdict: changes-requested
---

# Review of ./spec.md — round 01

## Findings

- [MAJOR] FR-1 · compound: three distinct "kell" behaviours joined by "és" (Foglalás létrehozása + Jegytípus-kontingens csökkentése + lejárati időbélyeg rendelése) mutating two aggregates (Hold és a 0002 kontingens-számláló) · spec.md:33
  rule: EARS / one behavior per FR (../_shared/ears.md, ../_shared/spec/checklist.md — "compound joining distinct behaviors")
  fix: bontsd külön FR-re: (a) `active` Foglalás létrehozása lejárati időbélyeggel, (b) az elérhető kontingens csökkentése a foglalt darabszámmal. Az atomicitást az NFR-3 invariáns hordozza, így a szétbontás nem gyengíti a szerződést.
  resolved: [x]

- [MAJOR] FR-3 · compound: állapotváltás (`expired`) + kontingens visszanövelése egyetlen FR-ben (két aggregátum) · spec.md:38
  rule: EARS / one behavior per FR (../_shared/ears.md)
  fix: bontsd külön: "Amikor egy `active` Foglalás fizetés nélkül lejár, a rendszernek `expired` állapotba kell állítania" és külön FR a kontingens visszanövelésére.
  resolved: [x]

- [MAJOR] FR-6 · compound: `cancelled` állapotra állítás + kontingens visszanövelése egy FR-ben (két aggregátum) · spec.md:46
  rule: EARS / one behavior per FR (../_shared/ears.md)
  fix: bontsd külön az állapotváltást és a kontingens-visszaírást; mindegyik önálló pass/fail ellenőrzést kap.
  resolved: [x]

- [MAJOR] FR-7 ↔ §6 · belső ellentmondás: FR-7 szerint "ha elérné a 10-et, akkor utasítsd el a további foglalást" (max 9), az elfogadási kritérium szerint a 11. jegy kerül elutasításra (max 10) · spec.md:48, spec.md:82
  rule: internal consistency — two normative lines fix the same value differently (../_shared/spec/checklist.md)
  fix: egyeztesd a küszöböt. Ha a szándék 10 engedélyezett jegy, írd: "Ha ... száma meghaladná a 10-et, akkor a rendszernek el kell utasítania a további foglalást", és tartsd meg a 11. jegyes forgatókönyvet.
  resolved: [x]

- [MAJOR] FR-2 · nincs elfogadási kritérium a 10 perces lejáratra (megsérti az 1:1 leképezést) · spec.md:36
  rule: acceptance 1:1 to requirements (../_shared/gherkin.md)
  fix: adj hozzá forgatókönyvet/checklist-sort, amely FR-2-t idézi és a 10 perces `expires_at`-ot ellenőrzi (pl. létrehozáskor `expires_at = created_at + 10 perc`).
  resolved: [x]

- [MAJOR] NFR-1 · nincs elfogadási kritérium a p95 < 500 ms követelményre · spec.md:58
  rule: acceptance 1:1 to requirements (../_shared/gherkin.md)
  fix: adj hozzá NFR-1-et idéző kritériumot (pl. terheléses forgatókönyv: 200 kérés/s mellett p95 < 500 ms).
  resolved: [x]

- [MAJOR] NFR-2 · nincs elfogadási kritérium a "lejárat után ≤ 5 s felszabadítás" követelményre · spec.md:59
  rule: acceptance 1:1 to requirements (../_shared/gherkin.md)
  fix: adj hozzá NFR-2-t idéző kritériumot, amely a lejárati időbélyeg és a kontingens-visszaírás közti késleltetést ≤ 5 s-ban méri.
  resolved: [x]

- [MINOR] FR-9 · "legfeljebb egy állapotváltozást szabad végrehajtania" — permission-jellegű megfogalmazás EARS obligáció (kell/nem szabad) helyett · spec.md:53
  rule: EARS obligation keywords (../_shared/ears.md)
  fix: fogalmazd prohibícióként: "... akkor a rendszernek nem szabad egynél több állapotváltozást végrehajtania". A számszerű korlát tesztelhető, csak a kulcsszó lágy.
  resolved: [x]

- [MINOR] §9 · "megbízható forrásból szinkronizált" — banned word "megbízható" (reliable) egy feltételezésben (nem normatív) · spec.md:104
  rule: banned-words (../_shared/banned-words.md)
  fix: konkretizáld (pl. "NTP-vel szinkronizált óra, ≤ 1 s eltéréssel"), vagy hagyd, ha nem normatív — de a szó kerülendő.
  resolved: [x]
