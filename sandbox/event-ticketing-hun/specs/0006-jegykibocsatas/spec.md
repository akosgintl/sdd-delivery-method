---
id: 0006-jegykibocsatas
title: Jegykibocsátás és QR-kód
status: ready            # draft | in-review | ready | in-progress | done | superseded
owner: fizetesi-csapat
created: 2026-07-04
updated: 2026-07-04
need: ../../discovery/prd-event-ticketing.md#6-feature-bontás--specifikációk
supersedes: null
---

# Specifikáció: Jegykibocsátás és QR-kód

> A szándékolt viselkedés pontos, tesztelhető leírása — a szerződés.

## 1. Összefoglaló és kontextus
Sikeres fizetés után a rendszernek minden megvásárolt jegyhez egyedi, QR-kóddal ellátott
Kibocsátott jegyet kell generálnia, amely a helyszínen beléptetésre érvényes. A QR-kódnak
hamisítás ellen aláírtnak kell lennie. Származó szükséglet: `../../discovery/prd-event-ticketing.md`.

## 2. Célok
- Minden kifizetett jegyhez egyedi, ellenőrizhető Kibocsátott jegyet létrehozni.
- A jegyet a vásárló számára letölthetővé és e-mailben elérhetővé tenni.

## 3. Nem-célok (hatókörön kívül)
- A helyszíni beolvasás és érvényesítés (lásd `0008-belepteto-beolvasas`).
- Nyomtatott jegyek postázása (PRD nem-cél).
- Apple/Google Wallet-integráció (későbbi feature).

## 4. Funkcionális követelmények
- **FR-1:** Amikor egy Rend `paid` állapotba kerül (`0005/FR-5`), a rendszernek a Rend minden
  jegyéhez létre kell hoznia egy Kibocsátott jegyet egyedi jegyazonosítóval és aláírt QR-kód
  tartalommal.
- **FR-2:** A rendszernek a QR-kód tartalmát kriptográfiai aláírással kell ellátnia, hogy a
  helyszínen ellenőrizhető legyen a hitelessége a szerver elérése nélkül is.
- **FR-3:** Ha egy Rend jegyeire ismételten érkezik kibocsátási kérés, akkor a rendszernek a már
  létező Kibocsátott jegyeket kell visszaadnia, és nem szabad újakat generálnia (idempotencia).
- **FR-4:** Amikor a Kibocsátott jegyek elkészültek, a rendszernek e-mailben el kell küldenie a
  vásárlónak a jegyeket tartalmazó visszaigazolást; a kézbesítés célidejét az NFR-3 rögzíti.
- **FR-5:** Amikor egy jegy érvénytelenné válik (esemény törlése `0001/FR-6` vagy visszaváltás
  `0007/FR-1`), a rendszernek `void` állapotba kell állítania a Kibocsátott jegyet.

## 5. Nem funkcionális követelmények
- **NFR-1:** A jegygenerálás a `paid` esemény után legfeljebb 10 másodperccel elkészül a Rend összes
  jegyére, legfeljebb 10 jegyig.
- **NFR-2:** A QR-kód aláírása HMAC-SHA256 vagy erősebb algoritmussal készül, legalább 256 bites
  kulccsal.
- **NFR-3:** A jegy-visszaigazoló e-mail kézbesítési kísérlete a fizetéstől számított 60 másodpercen
  belül megtörténik az esetek 99%-ában.

## 6. Elfogadási kritériumok / forgatókönyvek
```gherkin
Forgatókönyv: Fizetés után jegy generálódik                # igazolja FR-1, FR-2
  Adott egy 2 jegyet tartalmazó `paid` Rend
  Amikor a jegygenerálás lefut
  Akkor 2 Kibocsátott jegy jön létre egyedi azonosítóval és aláírt QR-kóddal

Forgatókönyv: Ismételt kibocsátás nem duplikál            # igazolja FR-3
  Adott egy Rend, amelynek jegyei már kibocsátásra kerültek
  Amikor újabb kibocsátási kérés érkezik ugyanarra a Rendre
  Akkor a rendszer a meglévő jegyeket adja vissza, új jegy nem jön létre
```
- [ ] A vásárló e-mailt kap a jegyekkel; a kézbesítési kísérlet a fizetéstől számított 60 s-en belül
  megtörténik az esetek 99%-ában (FR-4, NFR-3).
- [ ] Visszaváltott vagy törölt esemény jegye `void` állapotba kerül (FR-5).
- [ ] A QR-kód aláírása szerver nélkül is ellenőrizhető a helyszíni kulccsal (FR-2, NFR-2).
- [ ] A jegygenerálás a `paid` esemény után legfeljebb 10 másodperccel elkészül a Rend legfeljebb 10
  jegyére (NFR-1).

## 7. Élhelyzetek és hibaviselkedés
- **E-mail kézbesítési hiba:** a rendszernek legalább 3-szor, exponenciálisan növekvő
  várakozással kell újrapróbálnia; a jegy a fiókban akkor is elérhető marad (FR-4).
- **Aláírókulcs rotáció:** a beléptetésnek a kibocsátáskori kulcsazonosító alapján kell
  ellenőriznie, hogy a régi jegyek is érvényesek maradjanak (FR-2, `0008`).

## 8. Adatok és interfészek
- `IssuedTicket { id, order_id, event_id, seat_id?, qr_payload, qr_signature, key_id, status }`.
  Állapotok: `valid → used | void`.
- `POST /orders/{id}/issue` (idempotens), `GET /tickets/{id}`.
- Függőség: a `0005` `paid` eseménye indítja; a `0008` ellenőrzi a QR-t.

## 9. Függőségek és feltételezések
- Függőségek: `0005-penztar-fizetes`, e-mail-kézbesítő szolgáltatás.
- Feltételezés: a helyszíni beléptető eszközök hozzáférnek a nyilvános ellenőrző kulcshoz.

## 10. Nyitott kérdések
*(nincs)*

## 11. Indoklás / döntések
- Az offline ellenőrizhető aláírt QR (FR-2) teszi lehetővé a beléptetést gyenge térerőnél (`0008`).
- Az idempotens kibocsátás (FR-3) védi a webhook-ismétlés ellen.
