---
artifact: adr
target: ./0001-foglalas-lejarat-strategia.md
round: 1
reviewed: 2026-07-05
verdict: approved
---

# Review of ./0001-foglalas-lejarat-strategia.md — round 1

> Megjegyzés: ez **self-review** (a szerző végezte), mert a független ADR-reviewer sub-agent a
> session-limit miatt nem tudta befejezni. A független újranézés a `TEST-REPORT.md`-ben jelölt
> nyitott elem.

## Findings

Az ADR teljesíti az adr-checklist pontjait:
- Pontosan egy döntés (fix 10 perces lejárat). ✓
- Globálisan egyedi szám (0001; nem ütközik 0002/0003-mal). ✓
- Legális státusz (`accepted`), dátum, döntéshozók, kapcsolódó feature (`0004`). ✓
- A kontextus megindokolja, miért most (a PRD nyitott kérdése + a szellemfoglalás-metrika). ✓
- Valós elvetett alternatívák indokkal (jegytípusonként állítható, nincs lejárat, 2 perc). ✓
- Pozitív **és** negatív következmények is szerepelnek. ✓
- Immutabilitás: újonnan `accepted`, szerkezet rendben. ✓

Nincs BLOCKER/MAJOR/MINOR.
