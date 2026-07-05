---
artifact: problem-statement
target: ./problem-event-ticketing.md
round: 1
reviewed: 2026-07-04
verdict: approved
---

# Review of ./problem-event-ticketing.md — round 1

Independent review. A problem statement — problem-not-solution, named who, evidenced impact,
quantified success metric, explicit out-of-scope, and user stories with a benefit clause pointing
to specs. Judged against `../../.claude/skills/_shared/problem-statement/checklist.md` and
`banned-words.md`. No BLOCKER/MAJOR found; three MINOR nits below.

## Findings

- [MINOR] §Mi a probléma · a "megoldás-mechanizmus" (sorosítás egyetlen készlet-számláló ellen)
  beszivárog a probléma leírásába · problem-event-ticketing.md:25
  rule: problem-statement checklist — "States a problem, not a solution"
  fix: a probléma a túlértékesítés maga; a gyökérok megnevezhető ok-okozatként, de a
  "sorosítva egyetlen készlet-számláló ellen" megoldási irányt sugall — fogalmazd
  következmény-oldalról (pl. "mert a foglalás és a fizetés nem egyetlen készletállapot ellen
  dől el"), a mechanizmus döntését hagyd a specifikációkra.
  resolved: [x]

- [MINOR] §User story-k · tiltott szó "biztonságos" ("biztonságos fizetést akarok"), nem
  kvantifikált · problem-event-ticketing.md:60
  rule: banned-words (../../.claude/skills/_shared/banned-words.md) — „biztonságos" (secure)
  fix: user story benefit-záradékban áll (nem normatív FR), ezért enyhe; de érdemes az igényt
  mérhető haszonra cserélni (pl. "kártyaadat-tárolás nélküli fizetést"), a normatív pontosságot a
  `specs/0005-penztar-fizetes` viszi.
  resolved: [x]

- [MINOR] §Kinek a problémája · a "Beléptető" szereplő csak a user story-knál jelenik meg, a
  szereplő-felsorolásban nem · problem-event-ticketing.md:13-17 (vö. :64)
  rule: problem-statement checklist — "Names who has the problem, specifically"
  fix: vedd fel a Beléptetőt (helyszíni személyzet) harmadik szereplőként a "Kinek a problémája"
  szakaszba, hogy a :64 story szereplője a felsorolásból eredjen.
  resolved: [x]

## Notes (not findings)

- Impact evidenced with numbers (3,2% ütközés, 41 panasz/esemény, 18% szellemfoglalás, 2,5 óra).
- Success metrics quantified and 1:1 mappable to the impacts (< 0,1%, < 5%, < 30 perc).
- Out-of-scope explicit; assumptions/constraints written down (PCI-DSS, ≤ 200 req/s, ≤ 10 jegy).
- All seven user stories carry a benefit clause and a `→ feloldja: specs/…` pointer.
