---
artifact: prd
target: ./prd-event-ticketing.md
round: 1
reviewed: 2026-07-04
verdict: changes-requested
---

# Review of ./prd-event-ticketing.md — round 1

Independent review. A PRD — evidenced problem (not solution), goals with metric + target date,
named users, bounded scope with non-goals, a feature-breakdown table where each row maps to a
`spec.md`, and no requirement-level detail leaking down. Judged against
`../../.claude/skills/_shared/prd/checklist.md` and `banned-words.md`. One MAJOR (design leaks into
§1) blocks approval; MINORs follow.

## Findings

- [MAJOR] §1 Probléma és lehetőség · a megoldás tervezési mechanizmusa a probléma-szakaszban
  jelenik meg ("foglalás-lejárattal és sorosított készletkezeléssel bíró rendszer") ·
  prd-event-ticketing.md:19-20
  rule: prd checklist smell — "A solution presented as the problem in §1" / anatomy: "the solution
  design stays out"
  fix: tartsd meg az eredmény-állítást (egy egységes rendszer megszünteti a két hibaosztályt és a
  beállítást 2,5 óráról 30 perc alá viszi), de töröld a HOGYAN-t megnevező mechanizmusokat
  ("foglalás-lejárat", "sorosított készletkezelés") — ezek a `specs/0004` és a design szintjére
  tartoznak.
  resolved: [x]

- [MINOR] §2 Célok · tiltott szó "Gyorsítani" (gyors-tő) a cél megfogalmazásában ·
  prd-event-ticketing.md:28
  rule: banned-words — „gyors"
  fix: a metrika kvantifikál (medián beállítási idő < 30 perc), ezért enyhe; fogalmazd
  eredmény-oldalról, pl. "Csökkenteni a szervezői beállítási időt", a szám marad.
  resolved: [x]

- [MINOR] §2 Célok · tiltott szó "Megbízható" (reliable) a cél megfogalmazásában ·
  prd-event-ticketing.md:30
  rule: banned-words — „megbízható"
  fix: a metrika kvantifikál (0 dupla beléptetés), ezért enyhe; cseréld mérhető állításra, pl.
  "Egyszeri jegyérvényesítés a helyszínen (0 dupla beléptetés)".
  resolved: [x]

- [MINOR] §3 Célfelhasználók · tiltott szó "gyorsan" a felhasználói igény prózájában ·
  prd-event-ticketing.md:37
  rule: banned-words — „gyors" (nem-normatív próza)
  fix: cseréld konkrétabb igényre vagy hagyd el; a mérhető cél a §2-ben (< 30 perc / lejárat) él.
  resolved: [x]

- [MINOR] §7 Kockázatok · követelmény-/tervezési részlet szivárog le ("idempotencia-kulccsal") ·
  prd-event-ticketing.md:71
  rule: prd checklist smell — "Requirements-level / design detail — push down into specs"
  fix: a kockázat és mérséklés maradjon kezdeményezés-szinten ("a fizetés újrapróbálható a hely
  elvesztése nélkül"); az idempotencia-kulcs a `specs/0005` / design döntése.
  resolved: [x]

- [MINOR] §6 Feature-bontás · a feature-sorok közti függőségek / build-sorrend nincs jelölve,
  pedig a features nem függetlenek (pl. 0004 foglalás → 0005 pénztár → 0006 jegykibocsátás;
  0003 helyválasztás ⟵ 0001 esemény) · prd-event-ticketing.md:57-67
  rule: prd checklist — "Cross-feature dependencies (and build-order risk) are noted where
  features are not independent"
  fix: adj a táblához egy "Függ" oszlopot vagy egy rövid build-sorrend megjegyzést, hogy az
  integrációs kockázat ne maradjon rejtett.
  resolved: [x]

## Notes (not findings)

- Feature-breakdown table complete: all 9 named features present, each mapped to a
  `specs/NNNN-…/spec.md` path (0001–0009). Matches the 9 features referenced in the PRD.
- All 4 goals carry a metric + target date (2026-12-31); scope states in-scope and explicit
  non-goals; front-matter (`type, title, status, owner, updated`) complete, `status: draft` legal.
- Constitution references (Q-4, A-3) are citations, not restatements — acceptable.
