---
artifact: constitution
target: ./constitution.md
round: 1
reviewed: 2026-07-04
verdict: approved
---

# Review of ./constitution.md — round 01

Független felülvizsgálat. Az alkotmányt friss szemmel, a
`_shared/constitution/checklist.md` rubrika, a `banned-words.md` (magyar megfelelők) és a
`review-format.md` szerint bíráltam el.

## Összegzés

- Minden pont univerzális, nem alkudható és ellenőrizhető (P-1..P-4, A-1..A-3, Q-1..Q-4,
  T-1..T-3, R-1..R-3).
- A minőségi korlátok (§3) számmal kvantifikáltak: Q-1 ≥ 80% lefedettség; Q-2 WCAG 2.2 AA;
  Q-3 p95 < 300 ms @ 1000 RPS és hold p95 < 500 ms @ 200 RPS; Q-4 nincs high/critical
  találat, PAN nem tárolt nyugalomban, PCI-DSS-tanúsított fizetés.
- Q-4 a „Biztonság" fogalmat konkrét, ellenőrizhető feltételekre bontja — nem tiltott vágy-szó,
  hanem kvantifikált korlát. Nem találat.
- Nincs tiltott, homályos szó normatív mondatban (a magyar tiltólistára is szkennelve; a
  „kezel"/handle sehol nem fordul elő).
- §6 tartalmazza a módosítási folyamatot **és** az ADR-alapú kivétel-folyamatot (pont, indok,
  hatókör, lejárat). Nincs BLOCKER.
- Front-matter teljes (`type, version, ratified, last_amended, owner`). Nincs stílus-útmutató
  vagy feature-szintű részlet. Rövid, egy ülésben olvasható.

## Findings

- [MINOR] §1–§5 · a pontok az ellenőrizhetőségük módját (CI / review / tooling) nem nevezik meg
  explicit módon · constitution.md:16-52
  rule: constitution checklist ("Each clause notes how it's enforced")
  fix: Opcionális — a pontok természetüknél fogva ellenőrizhetők; ha kívánatos, egy-egy pont
  végére rövid „(CI: coverage gate)" / „(review)" / „(SAST pipeline)" zárójeles jelölés
  tehető. Nem blokkol.
  resolved: [ ]

## Verdict

approved — nincs feloldatlan BLOCKER vagy MAJOR.
