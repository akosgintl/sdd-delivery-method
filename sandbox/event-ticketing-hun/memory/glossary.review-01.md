---
artifact: glossary
target: ./glossary.md
round: 1
reviewed: 2026-07-04
verdict: approved   # javasolt bejegyzések promótálva a glosszáriumba
---

# Review of ./glossary.md — round 01 (glossary-maintainer, proposals)

A 9 specifikáció (`specs/0001..0009/spec.md`) átvizsgálva a glosszárium ellen: normatívan
használt, de nem definiált fogalmakat, körkörös definíciókat és szinonima-elcsúszást keresve.
A findingek javasolt/definiálatlan fogalmak; minden `fix` egy javasolt magyar definíció. A javasolt
bejegyzések `status: proposed` jelöléssel a glosszáriumhoz fűzve, emberi megerősítésre (a
`glossary-writer` promótálja).

## Findings

- [MAJOR] Elérhető kontingens · normatívan használt, de nem definiált; a `Kontingens` (felső korlát)
  fogalomtól eltérő, származtatott érték · 0002/spec.md:43, 0004/spec.md:40, 0007/spec.md:53
  rule: glossary checklist — undefined contested term used normatively (../_shared/glossary/checklist.md)
  fix: proposed definíció — „Egy Jegytípus pillanatnyilag eladható darabszáma: a Kontingens és a
    lekötött (eladott + foglalt) jegyek különbsége; a túlértékesítés elleni egyetlen igazságforrás."
    Kerülendő szinonima: „szabad készlet", „maradék"; és nem azonos a Kontingenssel (az a felső korlát).
  resolved: [x]

- [MAJOR] Auditnapló · normatívan használt, de nem definiált; ráadásul szinonima-elcsúszás a sima
  „napló"-val (0008 ugyanazt a fogalmat „naplóznia"/„naplóban" alakban írja) · 0001/spec.md:49,
  0005/spec.md:47, 0008/spec.md:45
  rule: glossary checklist — undefined contested term + synonym drift (../_shared/glossary/checklist.md)
  fix: proposed definíció — „Append-only, utólag nem módosítható napló, amely egy állapotváltozáshoz
    vagy műveleti kísérlethez rögzíti a cselekvő azonosítóját, a régi/új állapotot vagy az eredményt
    és az időbélyeget." Kerülendő: a sima „napló" önmagában, „log"; egy szó használandó.
  resolved: [x]

- [MAJOR] Kimutatás (értékesítési riport) · szinonima-elcsúszás: a `0009` ugyanazt a fogalmat
  „kimutatás" és „riport" alakban is használja (a cím „riportok", a törzs „kimutatás"), a `0008`
  „riport" · 0009/spec.md:3, 0009/spec.md:23, 0008/spec.md:44
  rule: glossary checklist — synonym drift, no agreed term (../_shared/glossary/checklist.md)
  fix: proposed definíció — „A szervezőnek eseményenként megjelenített, közel valós idejű
    értékesítési és beléptetési összesítés." Egy megnevezés használandó; a „kimutatás" és „riport"
    váltakozó használata kerülendő.
  resolved: [x]

## Megvizsgált, de NEM javasolt fogalmak (indoklással)

- **QR-kód / aláírt QR** — nem javasolt. A `QR-kód` már lehorgonyzott a meglévő `Kibocsátott jegy`
  és `Beléptetés` bejegyzésekben, és minden specifikáció következetesen használja (nincs drift). Az
  „aláírt" jelző kriptográfiai megvalósítási részlet (HMAC-SHA256, `0006/NFR-2`) → design/kód, nem
  glosszárium.
- **read model / olvasási nézet** — nem javasolt. Megvalósítási/architektúra-fogalom (`0009` §8/§11),
  a checklist szerint az implementációs részlet kimarad (→ design). (Külön kérdés, hogy a spec §8/§11
  design-szivárgást tartalmaz — az a spec-review dolga, nem a glosszáriumé.)
- **webhook** — nem javasolt. Megosztott technikai zsargon / megvalósítási részlet (`0005`), nem
  vitatott domain-fogalom.
- **outbox** — nem javasolt. Egyetlen specifikációban sem fordul elő (elavult jelölt).

## Összegzés

3 javasolt bejegyzés (`Elérhető kontingens`, `Auditnapló`, `Kimutatás`) `status: proposed` jelöléssel
a glosszáriumhoz fűzve. 4 jelölt megvizsgálva és elvetve (nem vitatott / megvalósítási részlet / nem
használt). Prune-jelölt: nincs (minden meglévő bejegyzésre hivatkozik legalább egy specifikáció).
