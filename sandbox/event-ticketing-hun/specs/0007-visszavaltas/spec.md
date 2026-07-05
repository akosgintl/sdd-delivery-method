---
id: 0007-visszavaltas
title: Visszaváltás és visszatérítés
status: ready            # draft | in-review | ready | in-progress | done | superseded
owner: fizetesi-csapat
created: 2026-07-04
updated: 2026-07-04
need: ../../discovery/prd-event-ticketing.md#6-feature-bontás--specifikációk
supersedes: null
---

# Specifikáció: Visszaváltás és visszatérítés

> A szándékolt viselkedés pontos, tesztelhető leírása — a szerződés.

## 1. Összefoglaló és kontextus
A vásárló vagy a szervező visszaválthat egy kifizetett jegyet: a jegy érvénytelenné válik, a
hozzá tartozó összeg visszatérül, és a felszabaduló kontingens újra eladhatóvá válik. A jegy
érvénytelenítésének és a kontingens felszabadításának atomi módon kell történnie, hogy ne
keletkezzen sem érvényes-de-visszatérített jegy, sem elveszett kontingens. Származó szükséglet:
`../../discovery/prd-event-ticketing.md`.

## 2. Célok
- Egy kifizetett jegyet érvényteleníteni és visszatéríteni.
- A felszabaduló kontingenst atomi módon újra eladhatóvá tenni.

## 3. Nem-célok (hatókörön kívül)
- A jegy átruházása másik vásárlóra (későbbi feature).
- Részleges (arányos) visszatérítési szabályzat összeállítása; az MVP a szervező által megadott
  visszatérítési arányt alkalmazza.

## 4. Funkcionális követelmények
- **FR-1:** Amikor a vásárló vagy a szervező visszaváltási kérést indít egy `valid` Kibocsátott
  jegyre az esemény kezdete előtt, a rendszernek `void` állapotba kell állítania a jegyet.
- **FR-2:** A rendszernek a visszatérítendő összeget a jegy eredeti fizetett ára és a szervező által
  az eseményre beállított visszatérítési arány (0–100%) szorzataként kell kiszámítania.
- **FR-3:** Ha a jegy már `used` (beléptetett) vagy `void` állapotban van, akkor a rendszernek el
  kell utasítania a visszaváltást, és nem szabad visszatérítést indítania.
- **FR-4:** Ha a visszaváltási kérés az esemény kezdete utánra esik, akkor a rendszernek el kell
  utasítania a visszaváltást, kivéve, ha a szervező az eseményt `cancelled` állapotba állította.
- **FR-5:** Amikor a szervező egy eseményt `cancelled` állapotba állít (`0001/FR-6`), a rendszernek
  minden `valid` jegyre teljes (100%) visszatérítést kell indítania a szervező visszatérítési
  arányától függetlenül.
- **FR-6:** Ha egy visszatérítés a fizetési szolgáltatónál sikertelen, akkor a rendszernek meg kell
  tartania a jegyet `void` állapotban, a visszatérítést `refund_pending` állapotúként kell
  nyilvántartania, és legalább 24 órán át újra kell próbálnia.
- **FR-7:** Ha ugyanarra a jegyre ismételt visszaváltási kérés érkezik ugyanazzal az
  idempotencia-kulccsal, akkor a rendszernek nem szabad egynél több visszatérítést végrehajtania
  (alkotmány P-4).
- **FR-8:** Amikor egy `valid` jegyre visszaváltás indul, a rendszernek vissza kell térítenie a
  jegyre eső, az FR-2 szerint számított összeget a vásárlónak.
- **FR-9:** Amikor egy jegy visszaváltás miatt `void` állapotba kerül, a rendszernek vissza kell
  növelnie az érintett Jegytípus elérhető kontingensét eggyel.

## 5. Nem funkcionális követelmények
- **NFR-1:** A visszaváltás-kezdeményezés (jegy-érvénytelenítés + kontingens-felszabadítás)
  p95 válaszideje < 500 ms 50 visszaváltás/másodperc mellett.
- **NFR-2:** A jegy-érvénytelenítés és a kontingens-visszanövelés atomi: nincs olyan pillanat,
  amelyben a jegy `void`, de a kontingens nem szabadult fel, vagy fordítva (100% konzisztencia).
- **NFR-3:** A visszatérítés a szolgáltató felé a kezdeményezéstől számított 5 percen belül
  elindul az esetek 99%-ában.

## 6. Elfogadási kritériumok / forgatókönyvek
```gherkin
Forgatókönyv: Érvényes jegy visszaváltása                  # igazolja FR-1, FR-2, FR-8, FR-9
  Adott egy 10000 forintért vett `valid` jegy egy 80% visszatérítési arányú eseményen
  Amikor a vásárló az esemény előtt visszaváltja
  Akkor a jegy `void` lesz, 8000 forint visszatérítés indul, és az elérhető kontingens 1-gyel nő

Forgatókönyv: Beléptetett jegy nem váltható vissza          # igazolja FR-3
  Adott egy `used` (már beléptetett) jegy
  Amikor visszaváltási kérés érkezik rá
  Akkor a rendszer elutasítja, és nem indul visszatérítés
```
- [ ] Esemény kezdete után indított visszaváltás elutasításra kerül, ha az esemény nem `cancelled`
  (FR-4).
- [ ] Esemény törlésekor minden `valid` jegy 100%-os visszatérítést kap (FR-5).
- [ ] Sikertelen szolgáltatói visszatérítés `refund_pending` marad és újrapróbálódik (FR-6).
- [ ] Ugyanazzal az idempotencia-kulccsal ismételt visszaváltás nem térít vissza kétszer (FR-7).
- [ ] A visszaváltás-kezdeményezés (jegy-érvénytelenítés + kontingens-felszabadítás) p95 válaszideje
  < 500 ms 50 visszaváltás/másodperc terhelési teszt alatt (NFR-1).
- [ ] Nincs olyan megfigyelhető pillanat, amelyben a jegy `void`, de a kontingens nem szabadult fel,
  vagy fordítva (NFR-2).
- [ ] A visszatérítés a szolgáltató felé a kezdeményezéstől számított 5 percen belül elindul az
  esetek 99%-ában (NFR-3).

## 7. Élhelyzetek és hibaviselkedés
- **Visszaváltás és beléptetés versenye:** ha a jegyet a visszaváltással egyidejűleg beolvassák, a
  rendszernek sorosított döntéssel pontosan egyet kell érvényesítenie: vagy `void`+visszatérítés,
  vagy `used`+beléptetés, de nem mindkettőt (FR-1, FR-3, `0008`).
- **Részleges rendszerhiba:** ha a jegy már `void`, de a visszatérítés nem indult el, az
  idempotencia-kulcs alapján az újrapróbálás a meglévő visszaváltást folytatja (FR-6, FR-7).

## 8. Adatok és interfészek
- `Refund { id, order_id, issued_ticket_id, amount_huf, status, idempotency_key, created_at }`.
  Állapotok: `refund_pending → refunded | failed`.
- `POST /tickets/{id}/refund` (idempotencia-kulccsal).
- Függőség: a jegy a `0006`; a fizetett Rend a `0005`; a kontingens a `0002`.

## 9. Függőségek és feltételezések
- Függőségek: `0005-penztar-fizetes`, `0006-jegykibocsatas`, külső fizetési szolgáltató.
- Feltételezés: a szolgáltató biztosít részleges és teljes visszatérítést idempotencia-kulccsal.

## 10. Nyitott kérdések
*(nincs)*

## 11. Indoklás / döntések
- A jegy-érvénytelenítés és a kontingens-felszabadítás atomicitása (NFR-2) a `design.md`-ben és egy
  dedikált ADR-ben (visszatérítés–felszabadítás atomicitás) rögzül.
- Az esemény-törlés 100%-os visszatérítése (FR-5) fogyasztóvédelmi elvárás.
