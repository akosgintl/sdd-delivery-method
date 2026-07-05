# Projekt Alkotmány — Jegyértékesítő rendszer

> A nem alkudható elvek, amelyeket a projekt minden specifikációja és megvalósítása köteles
> tiszteletben tartani. Legyen rövid és stabil. A módosításokat is felül kell vizsgálni (lásd §6).

```yaml
---
type: constitution
version: 1.0.0
ratified: 2026-07-04
last_amended: 2026-07-04
owner: architecture-guild
---
```

## 1. Mérnöki elvek
- **P-1:** Minden feature automatizált tesztekkel jelenik meg, amelyek a specifikációját igazolják;
  minden `FR-*`/`NFR-*` mögött legalább 1 teszt áll.
- **P-2:** Könyvtár-először: a képességeket önálló, függetlenül tesztelhető modulként építjük meg,
  mielőtt a felhasználói felülethez kötnénk őket.
- **P-3:** A specifikációt felülvizsgálják, és `ready` állapotba kerül, mielőtt a megvalósítás
  elkezdődik.
- **P-4:** A pénzügyi és készlet-műveletek (foglalás, fizetés, visszatérítés) idempotensek: azonos
  idempotencia-kulccsal érkező ismételt kérés legfeljebb 1 állapotváltozást okoz.

## 2. Architekturális megkötések
- **A-1:** A szolgáltatások közötti minden kommunikáció az API-átjárón keresztül történik.
- **A-2:** A megjelenítési réteg soha nem éri el közvetlenül az adatbázist.
- **A-3:** A jegykészlet (kontingens) egyetlen forrásból, tranzakciós írási művelettel változhat;
  a foglalás és az eladás ugyanazon készlet-számláló ellen, sorosított módon dolgozik.

## 3. Minőségi korlátok
- **Q-1:** Tesztlefedettség a módosított sorokon ≥ 80%; minden `FR-*`/`NFR-*` mögött van igazoló
  teszt.
- **Q-2:** Akadálymentesség: WCAG 2.2 AA szint minden vásárló felé néző felületen.
- **Q-3:** Teljesítménybüdzsé: az olvasási API-végpontok p95 válaszideje < 300 ms 1000 RPS mellett;
  a foglalási (hold) végpont p95 válaszideje < 500 ms 200 RPS mellett.
- **Q-4:** Biztonság és adatvédelem: a függőség-, titok- és SAST-vizsgálatok high/critical találat
  nélkül futnak le; kártyaadat (PAN) nem kerül tárolásra nyugalmi állapotban; a fizetés kizárólag
  PCI-DSS-tanúsított szolgáltatón keresztül történik.

## 4. Technológiai megkötések
- **T-1:** Engedélyezett backend nyelvek: Go, TypeScript. Engedélyezett frontend: TypeScript/React.
- **T-2:** Tiltott: GPL-licencű függőség a szállított kódban.
- **T-3:** A személyes adat (PII) az EU-régióban marad; megőrzési idő ≤ 90 nap, kivéve, ha egy
  specifikáció hosszabb jogalapot rögzít (pl. számviteli bizonylat 8 év).

## 5. Folyamati szabályok
- **R-1:** Minden viselkedésváltozás **ugyanabban** a pull requestben frissíti a `spec.md`-jét.
- **R-2:** A jelentős vagy nehezen visszafordítható döntéseket ADR-ként rögzítjük.
- **R-3:** A munka a Definition of Ready kaput a megvalósítás előtt, a Definition of Done kaput a
  késznek nyilvánítás előtt teljesíti.

## 6. Módosítások és kivételek
- Az alkotmány módosításához az architektúra-céh és 1 mérnöki vezető felülvizsgálata, valamint
  verzióemelés szükséges.
- Bármely pont alóli kivételt ADR-ként kell rögzíteni, megnevezve a pontot, az indokot, a hatókört
  és a lejárati/felülvizsgálati dátumot.

---
*Minden feature-szintű specifikáció örökli ezeket a szabályokat; a specifikációk csak a kifejezett
kivételeket jelzik (ADR-rel).*
