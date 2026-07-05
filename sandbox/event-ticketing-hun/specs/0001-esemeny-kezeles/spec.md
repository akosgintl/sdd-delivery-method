---
id: 0001-esemeny-kezeles
title: Esemény létrehozása és kezelése
status: ready            # draft | in-review | ready | in-progress | done | superseded
owner: szervezoi-csapat
created: 2026-07-04
updated: 2026-07-04
need: ../../discovery/prd-event-ticketing.md#6-feature-bontás--specifikációk
supersedes: null
---

# Specifikáció: Esemény létrehozása és kezelése

> A szándékolt viselkedés pontos, tesztelhető leírása — a szerződés. A **mit** és **miért**, nem a
> **hogyan**.

## 1. Összefoglaló és kontextus
A szervezőknek ma több eszközben szétszórtan kell eseményt beállítaniuk, ami átlagosan 2,5
munkaóra. Ez a feature egyetlen helyen teszi lehetővé egy Esemény létrehozását, publikálását,
módosítását és törlését, ellenőrzött állapotátmenetekkel. Származó szükséglet:
`../../discovery/prd-event-ticketing.md`.

## 2. Célok
- A szervező egy helyen tud eseményt létrehozni és publikálni.
- Az esemény állapota (`draft → published → cancelled`) ellenőrzött és auditált.
- A kapacitás soha nem csökkenthető a már lekötött (eladott + foglalt) mennyiség alá.

## 3. Nem-célok (hatókörön kívül)
- Jegytípusok és árazás definiálása (lásd `0002-jegytipusok-arazas`).
- Ülőhely-térkép szerkesztése (lásd `0003-helyvalasztas`).
- Ismétlődő (sorozat-) események automatikus generálása.

## 4. Funkcionális követelmények
> Minden követelmény: stabil ID, egyszingáris, egyértelmű, tesztelhető. Magyar EARS.

- **FR-1:** Amikor a szervező elküldi az esemény-létrehozó űrlapot a kötelező mezőkkel (név,
  helyszín, kezdő időpont, teljes kapacitás), a rendszernek létre kell hoznia az eseményt `draft`
  állapotban, és egyedi, változatlan eseményazonosítót kell rendelnie hozzá.
- **FR-2:** Ha a megadott kezdő időpont korábbi a létrehozás pillanatánál, akkor a rendszernek el
  kell utasítania a létrehozást, és nem szabad eseményt létrehoznia.
- **FR-3:** Ha a megadott teljes kapacitás nem egész szám 1 és 100000 között, akkor a rendszernek el
  kell utasítania a létrehozást.
- **FR-4:** Amikor a szervező publikálja a `draft` állapotú eseményt, a rendszernek `published`
  állapotba kell állítania, és láthatóvá kell tennie a vásárlók számára.
- **FR-5:** Amíg egy esemény `published` állapotban van, a rendszernek el kell utasítania a teljes
  kapacitás olyan módosítását, amely a kapacitást a már eladott és foglalt jegyek összege alá vinné.
- **FR-6:** Amikor a szervező `cancelled` állapotba állít egy eseményt, a rendszernek érvénytelenné
  kell jelölnie az esemény összes kibocsátott jegyét.
- **FR-7:** A rendszernek minden esemény-állapotváltozást naplóznia kell egy auditnaplóban, amely
  tartalmazza a szervező azonosítóját, a régi és az új állapotot, valamint az időbélyeget.
- **FR-8:** Amikor egy esemény `cancelled` állapotba kerül, a rendszernek el kell indítania a
  visszatérítési folyamatot az esemény jegyeire (lásd `0007/FR-1`).

## 5. Nem funkcionális követelmények
> Számszerűsített.

- **NFR-1:** Az esemény-létrehozó és -publikáló műveletek p95 válaszideje < 300 ms 1000 RPS
  olvasási és 50 RPS írási terhelés mellett.
- **NFR-2:** A szervezői felület minden képernyője megfelel a WCAG 2.2 AA szintnek.
- **NFR-3:** Az auditnapló bejegyzései legalább 400 napig visszakereshetők maradnak.

## 6. Elfogadási kritériumok / forgatókönyvek
> 1:1 leképezés a követelményekre.

```gherkin
Forgatókönyv: Érvényes esemény létrehozása                 # igazolja FR-1
  Adott egy bejelentkezett szervező
  Amikor kitölti az űrlapot érvényes névvel, helyszínnel, jövőbeli időponttal és 500 kapacitással
  Akkor létrejön egy esemény `draft` állapotban, egyedi azonosítóval

Forgatókönyv: Múltbeli időpont elutasítása                 # igazolja FR-2
  Adott egy bejelentkezett szervező
  Amikor a kezdő időpontot a tegnapi napra állítja
  Akkor a rendszer elutasítja a létrehozást, és nem jön létre esemény
```
- [ ] 0 vagy 100001 kapacitás megadásakor a rendszer elutasítja a létrehozást (FR-3).
- [ ] A `draft` esemény publikálása után az esemény állapota `published`, és megjelenik a vásárlói
  listában (FR-4).
- [ ] `published` eseménynél a kapacitás nem csökkenthető az eladott+foglalt összeg alá; a művelet
  elutasításra kerül (FR-5).
- [ ] Esemény törlésekor minden kibocsátott jegye `void` állapotba kerül (FR-6).
- [ ] Minden állapotváltozás után az auditnaplóban megjelenik egy bejegyzés a régi/új állapottal
  (FR-7).
- [ ] Esemény törlésekor az esemény jegyeire elindul a visszatérítési folyamat (FR-8).
- [ ] Az esemény-létrehozás és -publikálás p95 válaszideje < 300 ms 1000 RPS olvasás / 50 RPS írás
  terhelési teszt alatt (NFR-1).
- [ ] A szervezői képernyők automatizált WCAG 2.2 AA ellenőrzése hiba nélkül fut le (NFR-2).
- [ ] Egy 400 napja írt auditbejegyzés még visszakereshető (NFR-3).

## 7. Élhelyzetek és hibaviselkedés
- Egyidejű publikálás és kapacitás-módosítás: a rendszernek sorosítania kell a két írást, hogy az
  FR-5 invariáns ne sérüljön (FR-5).
- Már `cancelled` esemény ismételt törlése: a rendszernek idempotensen, állapotváltozás nélkül kell
  nyugtáznia (FR-6).
- Hiányzó kötelező mező: a rendszer mezőnkénti hibaüzenetet ad, esemény nem jön létre (FR-1).

## 8. Adatok és interfészek
- `Event { id, name, venue_id, starts_at, total_capacity, status, created_at }`.
- `POST /events`, `POST /events/{id}/publish`, `PATCH /events/{id}`, `POST /events/{id}/cancel`.
- Függőség: Helyszín (venue) referencia; a kontingens-számlálót a `0002` és `0004` használja.

## 9. Függőségek és feltételezések
- Függőségek: hitelesített szervezői munkamenet; a `0007` visszaváltási folyamat a törléshez.
- Feltételezés: egy eseményhez egy helyszín tartozik az MVP-ben.

## 10. Nyitott kérdések
> A `ready` előtt üresnek kell lennie.

*(nincs)*

## 11. Indoklás / döntések
- A `draft → published → cancelled` állapotgép megakadályozza a félkész események értékesítését.
- Az FR-5 invariáns a túlértékesítés elleni védelem első rétege (lásd alkotmány A-3).
