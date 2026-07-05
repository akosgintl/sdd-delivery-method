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

- [MAJOR→resolved] FR-4 ↔ NFR-3 belső ellentmondás · spec.md:38-39, 48-49
  A round-01 finding megoldva: FR-4 már időzítés nélkül mondja ki a viselkedést ("el kell
  küldenie ... a jegyeket tartalmazó visszaigazolást; a kézbesítés célidejét az NFR-3 rögzíti"),
  nincs abszolút 60 s / 100%. Az időzítést és a 99%-ot kizárólag NFR-3 hordozza; a §6 kritérium
  helyesen "kézbesítési kísérlet ... 99%-ában (FR-4, NFR-3)". Nincs többé két ellentmondó
  normatív sor.
  resolved: [x]

- [MAJOR→resolved] FR-5 compound + hatókör-szivárgás · spec.md:40-41
  Megoldva: FR-5 már csak a saját aggregátumon vett egyetlen viselkedést mondja ki ("`void`
  állapotba kell állítania a Kibocsátott jegyet"); a beléptetéskori QR-elutasítás ("kezelni")
  eltűnt, az a `0008` hatóköre. A trigger `0001/FR-6` vagy `0007/FR-1` — mindkét cross-ref
  létező FR (0007/FR-1 a `void`-ra állítás), a hivatkozás konzisztens.
  resolved: [x]

- [MAJOR→resolved] NFR-1 hiányzó elfogadási kritérium · spec.md:44-45, 67-68
  Megoldva: §6 tartalmaz NFR-1-et idéző kritériumot ("A jegygenerálás a `paid` esemény után
  legfeljebb 10 másodperccel elkészül a Rend legfeljebb 10 jegyére (NFR-1)").
  resolved: [x]

- [MINOR→resolved] FR-5 trigger "vagy"-gal két eseményt fog össze · spec.md:40
  A round-01 az érvénytelenítési eseményosztályt elfogadhatónak minősítette; a válasz azonos.
  resolved: [x]

## Round-2 checks (no new defect)

- EARS: FR-1 (Amikor/When), FR-2 (ubiquitous), FR-3 (Ha…akkor/If-Then, shall+shall not egy
  viselkedésről), FR-4 (Amikor/When), FR-5 (Amikor/When) — mind helyes kulcsszóval és
  "kell"/"nem szabad". Nincs soft "kellene/lehet".
- 1:1 lefedettség: FR-1..FR-5 és NFR-1..NFR-3 mind kap ≥1 kritériumot (§6 forgatókönyv +
  checklist), egyetlen kritérium sem hivatkozik nemlétező FR-re.
- §10 Nyitott kérdések üres; front-matter teljes és érintetlen (status `draft`); nincs új
  tiltott vágó szó normatív sorban.

Nincs feloldatlan BLOCKER vagy MAJOR → verdict: approved.
