# Definition of Ready (DoR) — Készenléti kapu

> Az a kapu, amelyet egy munkatételnek a megvalósítás **megkezdése előtt** teljesítenie kell. A
> csapat által birtokolt minőségi korlát, nem vámsorompó. A mélységet a kockázathoz igazítjuk; a
> tételek nem változnak.

Egy munkatétel **Ready (kész az építésre)**, ha az alábbiak mind teljesülnek:

## A. A szándék világos
- [ ] A szükséglet / user story rögzített és érthető (ki, mit, miért).
- [ ] Az üzleti érték vagy a sikermutató megfogalmazott.
- [ ] Visszavezethető egy célra/PRD-re (vagy kifejezetten önálló).

## B. A specifikáció megalapozott
- [ ] Létezik `spec.md`.
- [ ] A funkcionális követelményeknek stabil ID-juk van, egyszingárisak, egyértelműek és
      **tesztelhetők**.
- [ ] A nem funkcionális követelmények meghatározottak és **számszerűsítettek**, ahol értelmezhető.
- [ ] Léteznek elfogadási kritériumok, és a követelményekhez rendelhetők.
- [ ] Az élhelyzetek és a hibaviselkedés lefedettek (nem csak a boldog út).
- [ ] A célok **és** a nem-célok is rögzítettek.
- [ ] **Nem maradt nyitott kérdés.**
- [ ] A homályos kifejezéseket eltávolították/definiálták; a fogalmak illeszkednek a glosszáriumhoz.

## C. Megvalósítható és körülhatárolt
- [ ] Elég kicsi ahhoz, hogy becsülhető és a szokásos egységben szállítható legyen (különben
      **fel kell bontani**).
- [ ] A függőségek azonosítottak, elérhetők vagy sorba állítottak.
- [ ] A technikai megvalósíthatóság józansági ellenőrzésen esett át (spike, ha valós kétség volt).
- [ ] Megfelel az alkotmánynak, vagy rögzített ADR-kivétel létezik.

## D. Egyeztetett
- [ ] A specifikációt a mérnökség és az érintett érdekelt(ek)/terméktulajdonos átnézte.
- [ ] A csapat elég jól érti ahhoz, hogy elkezdje.
- [ ] (Ha releváns) a technikai megközelítés egyeztetett; a jelentős döntések rögzítettek.

## AI által megvalósított munkánál még:
- [ ] A specifikáció önhordó (egy törzstudás nélküli ügynök is meg tudná építeni).
- [ ] Az alkotmány / irányító szabályok az ügynök kontextusában vannak.
- [ ] Az elfogadási kritériumok gépiesen ellenőrizhetők.
- [ ] Az ügynök által tiszteletben tartandó interfészek/szerződések explicitek.

> A „Nem Ready” egészséges kimenet: **finomíts, spike-olj, bonts fel vagy halaszd** — soha nem
> „kezdd el mégis, majd menet közben tisztázzuk”.
