---
id: 0009-szervezoi-riportok
title: Szervezői riportok és értékesítési kimutatás
status: ready            # draft | in-review | ready | in-progress | done | superseded
owner: szervezoi-csapat
created: 2026-07-04
updated: 2026-07-04
need: ../../discovery/prd-event-ticketing.md#6-feature-bontás--specifikációk
supersedes: null
---

# Specifikáció: Szervezői riportok és értékesítési kimutatás

> A szándékolt viselkedés pontos, tesztelhető leírása — a szerződés.

## 1. Összefoglaló és kontextus
A szervezőnek valós idejű képre van szüksége az értékesítésről: mennyi fogyott jegytípusonként,
mennyi a bevétel, hány a foglalás, a visszaváltás és a beléptetett vendég. Ez a feature integrálja
a többi feature adatait egy szervezői kimutatássá. Származó szükséglet:
`../../discovery/prd-event-ticketing.md`.

## 2. Célok
- A szervező valós idejű értékesítési és beléptetési kimutatást lát eseményenként.
- A kimutatás exportálható elszámoláshoz.

## 3. Nem-célok (hatókörön kívül)
- Előrejelzés és keresletanalitika (későbbi feature).
- Több esemény összevont, portfólió-szintű kimutatása (MVP: eseményenként).

## 4. Funkcionális követelmények
- **FR-1:** Amikor a szervező megnyitja egy eseménye kimutatását, a rendszernek jegytípusonként meg
  kell jelenítenie az eladott, a foglalt és az elérhető darabszámot, valamint a bevételt forintban.
- **FR-2:** A rendszernek a kimutatás értékesítési adatait az eseményt érintő utolsó Rend,
  Foglalás, visszaváltás vagy beléptetés után legfeljebb 10 másodperccel frissítenie kell.
- **FR-3:** Amikor a szervező exportot kér, a rendszernek CSV formátumban le kell töltenie a
  kimutatást a jegytípusonkénti bontással és a Rendek tételes listájával.
- **FR-4:** A rendszernek a beléptetési kimutatásban meg kell jelenítenie a beléptetett, a még be
  nem lépett és az offline ütközésként naplózott (`0008/FR-5`) jegyek számát.
- **FR-5:** Ha a szervező olyan esemény kimutatását kéri, amely nem hozzá tartozik, akkor a
  rendszernek el kell utasítania a hozzáférést, és nem szabad adatot megjelenítenie.
- **FR-6:** A rendszernek a bevételi összegeket a visszaváltott összegek levonásával, nettó
  értékként kell megjelenítenie.

## 5. Nem funkcionális követelmények
- **NFR-1:** A kimutatás betöltésének p95 válaszideje < 1000 ms egy legfeljebb 100000 jegyet
  tartalmazó eseményre.
- **NFR-2:** A CSV-export legfeljebb 100000 sorra 30 másodpercen belül elkészül.
- **NFR-3:** A kimutatás felülete megfelel a WCAG 2.2 AA szintnek.

## 6. Elfogadási kritériumok / forgatókönyvek
```gherkin
Forgatókönyv: Értékesítési kimutatás jegytípusonként       # igazolja FR-1
  Adott egy esemény két jegytípussal és néhány eladott jeggyel
  Amikor a szervező megnyitja a kimutatást
  Akkor jegytípusonként látja az eladott, foglalt és elérhető darabszámot és a bevételt

Forgatókönyv: Idegen esemény adatai rejtve                 # igazolja FR-5
  Adott egy másik szervezőhöz tartozó esemény
  Amikor a szervező annak kimutatását kéri
  Akkor a rendszer elutasítja a hozzáférést, és nem jelenít meg adatot
```
- [ ] Egy új eladás után a kimutatás 10 másodpercen belül frissül (FR-2).
- [ ] A CSV-export tartalmazza a jegytípus-bontást és a Rendek tételes listáját (FR-3).
- [ ] A beléptetési kimutatás mutatja a beléptetett, a be nem lépett és az ütköző jegyek számát
  (FR-4).
- [ ] A bevétel a visszaváltások levonásával, nettó értékként jelenik meg (FR-6).
- [ ] A kimutatás betöltésének p95 válaszideje < 1000 ms egy legfeljebb 100000 jegyet tartalmazó
  eseményre (NFR-1).
- [ ] A CSV-export legfeljebb 100000 sorra 30 másodpercen belül elkészül (NFR-2).
- [ ] A kimutatás felületének automatizált WCAG 2.2 AA ellenőrzése hiba nélkül fut le (NFR-3).

## 7. Élhelyzetek és hibaviselkedés
- **Nulla eladás:** a kimutatásnak minden jegytípusra 0 eladást és teljes elérhető kontingenst kell
  mutatnia hiba nélkül (FR-1).
- **Folyamatban lévő visszaváltás:** a `refund_pending` állapotú összeg a nettó bevételből már
  levonásra kerül, és külön sorban jelenik meg (FR-6, `0007`).

## 8. Adatok és interfészek
- Olvasási nézet (read model) a `Order`, `Hold`, `Refund`, `ScanEvent` adatokból.
- `GET /events/{id}/report`, `GET /events/{id}/report.csv`.
- Függőség: minden korábbi feature adata; a hozzáférés a szervezői jogosultsághoz kötött.

## 9. Függőségek és feltételezések
- Függőségek: `0002`, `0004`, `0005`, `0007`, `0008` adatai.
- Feltételezés: a kimutatás közel valós idejű (≤ 10 s késleltetés) elfogadható; nem pénzügyi
  zárási bizonylat.

## 10. Nyitott kérdések
*(nincs)*

## 11. Indoklás / döntések
- A dedikált olvasási nézet (read model) tartja a kimutatás betöltését a teljesítménybüdzsén belül
  (NFR-1), anélkül, hogy az értékesítési tranzakciókat lassítaná.
