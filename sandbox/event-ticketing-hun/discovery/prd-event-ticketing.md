---
type: prd
title: Jegyértékesítő rendszer (MVP)
status: active           # draft | active | shipped | archived
owner: termekmenedzser
updated: 2026-07-04
---

# PRD: Jegyértékesítő rendszer (MVP)

> Termékszintű „miért és mit” egy kezdeményezéshez. A specifikációk felett áll — egy PRD tipikusan
> több feature-specifikációt szül. A miért/mit szinten marad; a viselkedési pontosság a
> specifikációkban él.

## 1. Probléma és lehetőség
A kis- és közepes eseményszervezők ma tabellás táblázatokban és külön fizetési linkeken adnak el
jegyet. Ez két mérhető kárt okoz: **túlértékesítést** (az eladott jegyek 3,2%-a ütközik) és
**foglalás-elakadást** (a kontingens 18%-a kifizetetlen foglalásban ragad a nyitás első órájában).
Egy egységes jegyértékesítő rendszer megszünteti ezt a két hibaosztályt, és a szervezői beállítási
időt 2,5 óráról 30 perc alá viszi. A megoldás felépítése (hogyan) a specifikációk és a
tervdokumentumok szintjén dől el. Részletek: `problem-event-ticketing.md`.

## 2. Célok és sikermutatók
- **Cél:** Megszüntetni a túlértékesítést. **Mutató:** ütköző eladások aránya. **Cél-érték:**
  < 0,1% az eladott jegyekre, 2026. december 31-ig.
- **Cél:** Felszámolni a szellemfoglalásokat. **Mutató:** kifizetetlen foglalásban álló kontingens
  a nyitás utáni első órában. **Cél-érték:** < 5%, 2026. december 31-ig.
- **Cél:** Csökkenteni a szervezői eseménybeállítási időt. **Mutató:** medián szervezői beállítási
  idő. **Cél-érték:** < 30 perc, 2026. december 31-ig.
- **Cél:** Egyszeri jegyérvényesítés a helyszínen. **Mutató:** duplán beléptetett jegyek száma.
  **Cél-érték:** 0 dupla beléptetés, 2026. december 31-ig.

## 3. Célfelhasználók és igényeik
- **Szervező** (kis-/közepes rendező): egy helyen akar eseményt beállítani, árat és kontingenst
  megadni, és valós idejű értékesítési képet látni.
- **Vásárló**: ütközés nélkül akar jegyet foglalni és fizetni, tudni akarja a helyét, és szükség
  esetén visszaváltani.
- **Beléptető** (helyszíni személyzet): egyértelmű, egyszeri érvényesítést akar a kapuban, akár
  gyenge térerőnél is.

## 4. Hatókör
- **Hatókörben:** eseménybeállítás; jegytípusok és árazás; ülőhely-választás; foglalás lejárattal;
  pénztár és fizetés; jegykibocsátás QR-kóddal; visszaváltás; beléptetés; szervezői riportok.
- **Hatókörön kívül / nem-célok:** másodlagos (viszonteladói) piac; dinamikus, keresletalapú
  árazás; nyomtatott jegyek postázása; marketing/hírlevél-küldés; többnyelvű vásárlói felület az
  MVP-ben (csak magyar).

## 5. Megkötések és feltételezések
- Fizetés kizárólag PCI-DSS-tanúsított külső szolgáltatón; kártyaadat nem tárolt (alkotmány Q-4).
- A személyes adat az EU-régióban marad; megőrzés ≤ 90 nap, kivéve a számviteli bizonylatot.
- Csúcsterhelés eseménynyitáskor ≤ 200 párhuzamos foglalási kérés/másodperc.
- A készlet egyetlen forrásból, sorosított írással változik (alkotmány A-3).

## 6. Feature-bontás → specifikációk
> Minden sor egy specifikációvá válik a `specs/` alatt. A „Függ” oszlop a build-sorrendet jelzi;
> az alapfolyamat 0001 → 0002 → 0004 → 0005 → 0006, a helyválasztás (0003) és a beléptetés (0008)
> ráépül, a riport (0009) a végén integrál.

| Feature | Specifikáció | Függ | Státusz |
|---------|--------------|------|---------|
| Esemény létrehozása és kezelése | specs/0001-esemeny-kezeles/spec.md | — | draft |
| Jegytípusok és árazás | specs/0002-jegytipusok-arazas/spec.md | 0001 | draft |
| Ülőhely-választás / helytérkép | specs/0003-helyvalasztas/spec.md | 0001, 0004 | draft |
| Foglalás / kosár lejárattal | specs/0004-foglalas-kosar/spec.md | 0002 | draft |
| Pénztár és fizetés | specs/0005-penztar-fizetes/spec.md | 0004 | draft |
| Jegykibocsátás és QR-kód | specs/0006-jegykibocsatas/spec.md | 0005 | draft |
| Visszaváltás és visszatérítés | specs/0007-visszavaltas/spec.md | 0005, 0006 | draft |
| Beléptetés / QR-beolvasás | specs/0008-belepteto-beolvasas/spec.md | 0006 | draft |
| Szervezői riportok | specs/0009-szervezoi-riportok/spec.md | 0005 | draft |

## 7. Kockázatok és nyitott kérdések
- **Kockázat:** a fizetési szolgáltató kimaradása a nyitási csúcson. **Mérséklés:** a foglalás a
  fizetéstől függetlenül tartja a helyet a lejáratig, és a fizetés a hely elvesztése nélkül
  újrapróbálható; a pontos mechanizmus a `specs/0005` és a design döntése.
- **Kockázat:** a helyszínen gyenge a térerő a beléptetéskor. **Mérséklés:** a beléptetés offline
  módot igényel (a 0008 specifikáció dönti el, hogyan).
- **Nyitott kérdés (a specifikálásig):** a foglalás lejárati ideje fix vagy jegytípusonként
  állítható legyen-e? → a `0004` specifikáció és `ADR-0001` dönti el.
