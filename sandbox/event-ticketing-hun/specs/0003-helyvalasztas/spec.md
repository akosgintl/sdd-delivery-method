---
id: 0003-helyvalasztas
title: Ülőhely-választás és helytérkép
status: ready            # draft | in-review | ready | in-progress | done | superseded
owner: vasarloi-csapat
created: 2026-07-04
updated: 2026-07-04
need: ../../discovery/prd-event-ticketing.md#6-feature-bontás--specifikációk
supersedes: null
---

# Specifikáció: Ülőhely-választás és helytérkép

> A szándékolt viselkedés pontos, tesztelhető leírása — a szerződés.

## 1. Összefoglaló és kontextus
Ülőhelyes eseményeknél a vásárló egy térképen látja a szabad és foglalt Ülőhelyeket, és konkrét
helyet választ. A választás pillanatában a helyet ki kell zárni mások elől, hogy két vásárló ne
válassza ugyanazt. Származó szükséglet: `../../discovery/prd-event-ticketing.md`.

## 2. Célok
- A vásárló egy térképen valós idejű szabad/foglalt állapotot lát.
- Egy Ülőhelyet egyszerre legfeljebb egy vásárló tud kiválasztani.

## 3. Nem-célok (hatókörön kívül)
- A Foglalás lejárati logikája (lásd `0004-foglalas-kosar`).
- A helytérkép grafikus szerkesztője a szervezőnek (későbbi feature; az MVP feltölthető
  ülőhelylistát használ).
- Állóhelyes (nem számozott) események (azoknál a `0002` kontingens elég).

## 4. Funkcionális követelmények
- **FR-1:** Amikor a vásárló megnyitja egy ülőhelyes esemény térképét, a rendszernek minden
  Ülőhelyet a valós állapotával (`szabad`, `kivalasztott`, `eladott`) legfeljebb 2 másodperces
  késleltetéssel kell megjelenítenie.
- **FR-2:** Amikor a vásárló kiválaszt egy `szabad` Ülőhelyet, a rendszernek `kivalasztott`
  állapotba kell állítania az Ülőhelyet kizárólag az adott vásárló számára.
- **FR-3:** Ha a vásárló olyan Ülőhelyet próbál kiválasztani, amely már nem `szabad`, akkor a
  rendszernek el kell utasítania a választást, és frissített állapotot kell mutatnia.
- **FR-4:** Amíg egy Ülőhely `kivalasztott` állapotban van egy vásárlónál, a rendszernek meg kell
  akadályoznia, hogy bármely másik vásárló kiválassza.
- **FR-5:** Amikor egy Ülőhelyhez tartozó Foglalás lejár vagy törlődik, a rendszernek vissza kell
  állítania az Ülőhelyet `szabad` állapotba.
- **FR-6:** Amikor egy Ülőhely `kivalasztott` állapotba kerül, a rendszernek Foglalást kell
  kezdeményeznie az adott vásárló nevére a `0004/FR-1` szerint.

## 5. Nem funkcionális követelmények
- **NFR-1:** A térkép állapot-lekérdezésének p95 válaszideje < 300 ms legfeljebb 2000 ülőhelyet
  tartalmazó térképnél.
- **NFR-2:** Egy Ülőhely kiválasztásának megerősítése p95 < 500 ms 200 párhuzamos kiválasztás/
  másodperc mellett.
- **NFR-3:** A térkép felülete megfelel a WCAG 2.2 AA szintnek, beleértve a nem kizárólag színnel
  jelzett állapotot.

## 6. Elfogadási kritériumok / forgatókönyvek
```gherkin
Forgatókönyv: Szabad hely kiválasztása                     # igazolja FR-2, FR-6
  Adott egy ülőhelyes esemény szabad „B-12” hellyel
  Amikor a vásárló kiválasztja a „B-12” helyet
  Akkor a hely `kivalasztott` lesz kizárólag a vásárlónál, és a nevére Foglalás indul

Forgatókönyv: Ütköző kiválasztás elutasítása                # igazolja FR-3, FR-4
  Adott a „B-12” hely, amelyet egy másik vásárló épp `kivalasztott` állapotba tett
  Amikor a vásárló megpróbálja kiválasztani a „B-12” helyet
  Akkor a rendszer elutasítja a választást, és a helyet foglaltként mutatja
```
- [ ] A térkép a helyek állapotát legfeljebb 2 másodperces késleltetéssel tükrözi (FR-1).
- [ ] A Foglalás lejárta után a hely ismét `szabad` állapotban jelenik meg (FR-5).
- [ ] 2000 ülőhelyes térkép állapot-lekérdezésének p95 válaszideje < 300 ms (NFR-1).
- [ ] A kiválasztás megerősítésének p95 válaszideje < 500 ms 200 párhuzamos kiválasztás/másodperc
  terhelésen (NFR-2).
- [ ] Az állapotjelzés nem kizárólag színnel történik, és a térkép átmegy a WCAG 2.2 AA
  ellenőrzésen (NFR-3).

## 7. Élhelyzetek és hibaviselkedés
- Két vásárló pontosan egyszerre választja ugyanazt a helyet: a rendszernek sorosított
  kizárással pontosan egyiküknek kell megadnia, a másikat elutasítania (FR-4).
- A vásárló elnavigál a kiválasztás után fizetés nélkül: a hely a Foglalás lejáratakor felszabadul
  (FR-5, `0004`).

## 8. Adatok és interfészek
- `Seat { id, event_id, section, row, number, status, held_by_hold_id }`.
- `GET /events/{id}/seatmap`, `POST /events/{id}/seats/{seatId}/select`.
- Függőség: a kiválasztás Foglalást hoz létre a `0004` szerint.

## 9. Függőségek és feltételezések
- Függőségek: `0001-esemeny-kezeles`, `0004-foglalas-kosar`.
- Feltételezés: az ülőhelylista az esemény létrehozásakor feltöltésre kerül; egy esemény vagy
  ülőhelyes, vagy állóhelyes.

## 10. Nyitott kérdések
*(nincs)*

## 11. Indoklás / döntések
- A kiválasztás azonnal Foglalást indít, így az ülőhely-kizárás és a kontingens-kezelés ugyanazt a
  lejárati mechanizmust használja (`0004`), elkerülve a két külön időzítőt.
