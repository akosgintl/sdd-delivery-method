---
id: 0008-belepteto-beolvasas
title: Beléptetés és QR-beolvasás
status: ready            # draft | in-review | ready | in-progress | done | superseded
owner: helyszini-csapat
created: 2026-07-04
updated: 2026-07-04
need: ../../discovery/prd-event-ticketing.md#6-feature-bontás--specifikációk
supersedes: null
---

# Specifikáció: Beléptetés és QR-beolvasás

> A szándékolt viselkedés pontos, tesztelhető leírása — a szerződés.

## 1. Összefoglaló és kontextus
A beléptető a helyszínen beolvassa a Kibocsátott jegy QR-kódját, és a rendszer egyszer érvényesíti:
ugyanazzal a jeggyel másodszor nem lehet belépni. A helyszínen gyenge lehet a térerő, ezért a QR
hitelessége offline is ellenőrizhető, az egyszeri felhasználás pedig a hálózat visszatértekor
szinkronizálódik. Származó szükséglet: `../../discovery/prd-event-ticketing.md`.

## 2. Célok
- Minden jegyet a helyszínen pontosan egyszer érvényesíteni.
- A QR hitelességét gyenge térerőnél is ellenőrizni.

## 3. Nem-célok (hatókörön kívül)
- A jegy generálása és aláírása (lásd `0006-jegykibocsatas`).
- Forgókapu-hardver vezérlése (integrációs feature később).

## 4. Funkcionális követelmények
- **FR-1:** Amikor a beléptető beolvas egy QR-kódot, amelynek aláírása érvényes és a jegye `valid`
  állapotú, a rendszernek `used` állapotba kell állítania a jegyet, és „belépés engedélyezve”
  eredményt kell adnia.
- **FR-2:** Ha a beolvasott jegy már `used` állapotban van, akkor a rendszernek „már felhasznált”
  eredményt kell adnia, és nem szabad újabb belépést engedélyeznie.
- **FR-3:** Ha a beolvasott QR aláírása érvénytelen, vagy a jegy `void` állapotú, akkor a
  rendszernek „érvénytelen jegy” eredményt kell adnia, és nem szabad belépést engedélyeznie.
- **FR-4:** Amíg a beléptető eszköz offline üzemmódban van, a rendszernek a jegy hitelességét az
  aláírás és a kulcsazonosító alapján kell eldöntenie.
- **FR-7:** Amíg a beléptető eszköz offline üzemmódban van, a rendszernek a `used` jelölést
  lokálisan kell rögzítenie, és a hálózat visszatértekor szinkronizálnia kell.
- **FR-5:** Ha a szinkronizáláskor kiderül, hogy ugyanazt a jegyet két különböző offline eszköz is
  `used`-ként rögzítette, akkor a rendszernek az első időbélyegűt kell érvényesnek tekintenie, és a
  többit ütközésként kell naplóznia a szervező riportja felé (`0009`).
- **FR-6:** A rendszernek minden beolvasási kísérletet naplóznia kell a jegyazonosítóval, az
  eredménnyel, az eszköz azonosítójával és az időbélyeggel.

## 5. Nem funkcionális követelmények
- **NFR-1:** Egy online beolvasás érvényesítésének p95 válaszideje < 300 ms 100 beolvasás/másodperc
  mellett.
- **NFR-2:** Offline üzemmódban a hitelesség-ellenőrzés hálózat nélkül, az eszközön tárolt nyilvános
  kulccsal < 100 ms alatt megtörténik.
- **NFR-3:** Az offline `used` jelölések a hálózat visszatértétől számított 60 másodpercen belül
  szinkronizálódnak.

## 6. Elfogadási kritériumok / forgatókönyvek
```gherkin
Forgatókönyv: Érvényes jegy első beolvasása                # igazolja FR-1
  Adott egy `valid` jegy érvényes aláírással
  Amikor a beléptető beolvassa
  Akkor a jegy `used` lesz, és a rendszer belépést engedélyez

Forgatókönyv: Ugyanaz a jegy másodszor                     # igazolja FR-2
  Adott egy már `used` jegy
  Amikor a beléptető újra beolvassa
  Akkor a rendszer „már felhasznált" eredményt ad, és nem enged be
```
- [ ] Érvénytelen aláírású vagy `void` jegy beolvasása „érvénytelen jegy” eredményt ad (FR-3).
- [ ] Offline üzemmódban a hitelesség hálózat nélkül eldől az aláírás alapján (FR-4), a `used`
  jelölés lokálisan rögzül és a hálózat visszatértekor szinkronizálódik (FR-7, NFR-2, NFR-3).
- [ ] Két offline eszköz ütköző `used` jelölésénél az első időbélyegű nyer, a többi naplózódik
  (FR-5).
- [ ] Minden beolvasási kísérlet megjelenik a naplóban a jegyazonosítóval, az eredménnyel, az eszköz
  azonosítójával és az időbélyeggel (FR-6).
- [ ] Egy online beolvasás érvényesítésének p95 válaszideje < 300 ms 100 beolvasás/másodperc
  terhelési teszt alatt (NFR-1).

## 7. Élhelyzetek és hibaviselkedés
- **Óracsúszás az eszközök között:** az ütközésfeloldáshoz (FR-5) a szerver a beolvasáskori,
  eszköz által küldött monoton időbélyeget használja, és óracsúszás esetén az elsőként
  szinkronizált beolvasást tekinti mérvadónak (FR-5).
- **Ismételt szinkron:** ugyanannak az offline beolvasásnak a kétszeri feltöltése nem hoz létre két
  `used` átmenetet (FR-4, FR-6).

## 8. Adatok és interfészek
- `ScanEvent { id, issued_ticket_id, device_id, result, scanned_at, synced_at }`.
- `POST /scan` (online), `POST /scan/sync` (offline köteg).
- Függőség: a jegy és az aláírás a `0006`; az ütközés-riport a `0009`.

## 9. Függőségek és feltételezések
- Függőségek: `0006-jegykibocsatas` (aláírt QR, kulcsazonosító).
- Feltételezés: a beléptető eszközök a műszak előtt szinkronizálják az érvényes nyilvános kulcsot
  és a jegylistát.

## 10. Nyitott kérdések
*(nincs)*

## 11. Indoklás / döntések
- Az „első időbélyeg nyer” ütközésfeloldás (FR-5) determinisztikus és auditálható; a ritka offline
  dupla-belépést utólag a szervező látja a riportban (`0009`).
