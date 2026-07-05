# Definition of Done (DoD) — Kész-kapu

> Az a kapu, amelyet a munkának teljesítenie kell, hogy késznek nyilváníthassuk. Az SDD-t
> meghatározó pontok: **a specifikációhoz mérten igazolt** és **a specifikáció a valósággal
> összehangolt**. A mélységet a kockázathoz igazítjuk; a tételek nem változnak.

A munka **Done (kész)**, ha az alábbiak mind teljesülnek:

## A. A specifikáció teljesítve
- [ ] A specifikáció minden elfogadási kritériuma teljesül és bemutatható.
- [ ] Minden `FR-*` megvalósított, és legalább 1, az ID-jára hivatkozó automatizált teszt fedi.
- [ ] Minden `NFR-*` a megadott számhoz mérten igazolt (nem feltételezett).
- [ ] A specifikált élhelyzetek és hibaviselkedés megvalósított és tesztelt.

## B. A specifikáció összehangolva  *(SDD-specifikus)*
- [ ] A specifikáció azt tükrözi, amit ténylegesen megépítettünk — minden viselkedésváltozás a
      specifikációban van, **ugyanabban a PR-ben**.
- [ ] A tervdokumentum / ADR-ek frissítve a megvalósítás közben hozott megközelítés-döntésekkel.
- [ ] A nyomonkövethetőség teljes: szükséglet → követelmény → teszt mind összekötve; nincs árva
      vagy teszteletlen követelmény.
- [ ] A specifikáció státusza `done`-ra emelve; az `updated` dátum aktuális.

## C. Mérnöki minőség
- [ ] A kódot felülvizsgálták és a célágba mergelték.
- [ ] Az automatizált tesztek zölden futnak a CI-ban (unit, integrációs, elfogadási/szerződéses).
- [ ] A lefedettség eléri az alkotmány korlátját (≥ 80% a módosított sorokon).
- [ ] A statikus elemzés / lint / típusellenőrzés átmegy.
- [ ] A biztonsági ellenőrzések átmennek (függőség, titok, SAST, ahol értelmezhető).
- [ ] Nincs a specifikációnak ellentmondó TODO/FIXME; nincs egyeztetett súlyosság feletti ismert
      hiba.

## D. Üzemeltethetőség és dokumentáció
- [ ] A vásárló felé néző dokumentáció / changelog / gyorsindító frissítve.
- [ ] A megfigyelhetőség (logok/metrikák/riasztások) az alkotmány szerint a helyén van.
- [ ] A feature flag / kivezetés / migráció kezelve; a visszaállítási út ismert.
- [ ] A runbook / on-call jegyzetek frissítve, ha az üzemeltetést érinti.

## E. Elfogadva
- [ ] A terméktulajdonos / érdekelt elfogadja a specifikáció elfogadási kritériumaihoz mérten.
- [ ] Az egyeztetett környezetbe telepítve (vagy mergelve és telepítésre kész, a folyamat szerint).

## AI által megvalósított munkánál még:
- [ ] Egy ember a kimenetet **a specifikációhoz mérten** nézte át (minden elfogadási kritériumot),
      nem csak azt, hogy „lefut”.
- [ ] Az ügynök specifikációtól való eltéréseit összehangolták (a kódot javították, vagy a
      specifikációt frissítették egy döntéssel).
- [ ] A generált tesztek a specifikációt igazolják — nem pusztán a megvalósítást tükrözik.

> Lakmuszpróba: nyisd meg a specifikációt és a futó rendszert egymás mellett — megegyeznek minden
> elfogadási kritériumban, mindegyiket egy teszt bizonyítja?
