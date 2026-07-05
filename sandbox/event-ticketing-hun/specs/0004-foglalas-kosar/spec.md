---
id: 0004-foglalas-kosar
title: Foglalás és kosár lejárati idővel
status: ready            # draft | in-review | ready | in-progress | done | superseded
owner: vasarloi-csapat
created: 2026-07-04
updated: 2026-07-04
need: ../../discovery/prd-event-ticketing.md#6-feature-bontás--specifikációk
supersedes: null
---

# Specifikáció: Foglalás és kosár lejárati idővel

> A szándékolt viselkedés pontos, tesztelhető leírása — a szerződés.

## 1. Összefoglaló és kontextus
A vásárló a fizetés előtt Foglalással (Hold) köti le a jegyeket vagy ülőhelyeket. A Foglalás egy
lejárati időn belül fizetéssé alakítható; utána automatikusan felszabadul, hogy más is
vásárolhasson. Ez a feature szünteti meg a szellemfoglalásokat (a kontingens 18%-a ragadt be a
nyitás első órájában). Származó szükséglet: `../../discovery/prd-event-ticketing.md`.

## 2. Célok
- A vásárló jegyeit a fizetésig kizárólagosan lekötni.
- A kifizetetlen Foglalásokat automatikusan felszabadítani a lejáratkor.
- A kontingens-számlálót egyidejű foglalások mellett is pontosan tartani.

## 3. Nem-célok (hatókörön kívül)
- A fizetés lebonyolítása (lásd `0005-penztar-fizetes`).
- A jegy fizikai kibocsátása (lásd `0006-jegykibocsatas`).
- Az ülőhely-kizárás felülete (lásd `0003-helyvalasztas`; ez a spec a mögöttes Foglalást adja).

## 4. Funkcionális követelmények
- **FR-1:** Amikor a vásárló jegyet vagy ülőhelyet foglal, és az érintett Jegytípus elérhető
  kontingense elegendő, a rendszernek Foglalást kell létrehoznia a vásárló nevére `active`
  állapotban, lejárati időbélyeggel.
- **FR-2:** A rendszernek a Foglalás lejárati idejét a létrehozástól számított 10 percre kell
  állítania (a fix vs. jegytípusonként állítható lejárat döntése: `ADR-0001`).
- **FR-3:** Amikor egy `active` Foglalás lejárati ideje fizetés nélkül letelik, a rendszernek
  `expired` állapotba kell állítania a Foglalást.
- **FR-4:** Ha a vásárló olyan Jegytípusból foglal, amelynek elérhető kontingense kisebb a kért
  darabszámnál, akkor a rendszernek el kell utasítania a foglalást, és nem szabad Foglalást
  létrehoznia.
- **FR-5:** Amíg egy Foglalás `active` állapotban van, a rendszernek meg kell akadályoznia, hogy a
  benne lekötött jegyeket vagy ülőhelyeket bármely másik vásárló lefoglalja vagy megvegye.
- **FR-6:** Amikor a vásárló a lejárat előtt elveti a Foglalást, a rendszernek `cancelled`
  állapotba kell állítania a Foglalást.
- **FR-7:** Ha egy új foglalás egy vásárló aktív jegyeinek számát egy eseményhez 10 fölé emelné,
  akkor a rendszernek el kell utasítania a foglalást.
- **FR-8:** Amikor a Foglaláshoz tartozó fizetés sikeresen lezárul (`0005/FR-1`), a rendszernek
  `converted` állapotba kell állítania a Foglalást, és nem szabad a lekötött kontingenst
  visszanövelnie.
- **FR-9:** Ha egy Foglalásra érkező ismételt fizetési vagy foglalási kérés ugyanazt az
  idempotencia-kulcsot hordozza, akkor a rendszernek legfeljebb egy állapotváltozást kell
  végrehajtania (alkotmány P-4).
- **FR-10:** Amikor egy Foglalás létrejön, a rendszernek csökkentenie kell az érintett Jegytípus
  elérhető kontingensét a foglalt darabszámmal.
- **FR-11:** Amikor egy Foglalás `expired` vagy `cancelled` állapotba kerül, a rendszernek vissza
  kell növelnie az érintett Jegytípus elérhető kontingensét a felszabadított darabszámmal.

## 5. Nem funkcionális követelmények
- **NFR-1:** A foglalási művelet p95 válaszideje < 500 ms 200 foglalási kérés/másodperc mellett.
- **NFR-2:** A lejárt Foglalások felszabadítása a lejárati időbélyeg után legfeljebb 5 másodperccel
  megtörténik.
- **NFR-3:** 200 párhuzamos foglalásnál ugyanazon Jegytípus utolsó darabjaira a túlfoglalt és az
  alulfoglalt jegyek száma egyaránt 0 (pontos kontingens-számlálás).

## 6. Elfogadási kritériumok / forgatókönyvek
```gherkin
Forgatókönyv: Foglalás csökkenti az elérhető kontingenst    # igazolja FR-1, FR-10
  Adott egy „Állóhely” jegytípus 100 elérhető kontingenssel
  Amikor a vásárló lefoglal 2 jegyet
  Akkor létrejön egy `active` Foglalás lejárati idővel, és az elérhető kontingens 98

Forgatókönyv: Lejárat felszabadítja a jegyeket             # igazolja FR-3, FR-11
  Adott egy `active` Foglalás 2 jeggyel, amely 10 perce jött létre
  Amikor eltelik a lejárati idő fizetés nélkül
  Akkor a Foglalás `expired` lesz, és az elérhető kontingens visszanő 2-vel

Forgatókönyv: Utolsó jegy versenyhelyzetben                # igazolja FR-4, FR-5, NFR-3
  Adott egy jegytípus 1 elérhető kontingenssel
  Amikor két vásárló pontosan egyszerre próbálja lefoglalni
  Akkor pontosan egy Foglalás jön létre, a másik kérés elutasításra kerül, az elérhető kontingens 0
```
- [ ] A lejárat előtti elvetés `cancelled` állapotba állítja a Foglalást (FR-6), és visszanöveli az
  elérhető kontingenst (FR-11).
- [ ] Egy vásárló 10 jegye mellett a 11. jegy foglalása ugyanahhoz az eseményhez elutasításra kerül
  (FR-7).
- [ ] Sikeres fizetés után a Foglalás `converted`, és a kontingens nem nő vissza (FR-8).
- [ ] Ugyanazzal az idempotencia-kulccsal küldött ismételt foglalás nem hoz létre második
  Foglalást (FR-9).
- [ ] Egy frissen létrehozott Foglalás lejárati időbélyege pontosan a létrehozás + 10 perc (FR-2).
- [ ] A foglalási művelet p95 válaszideje < 500 ms 200 foglalási kérés/másodperc terhelési teszt
  alatt (NFR-1).
- [ ] A lejárt Foglalás felszabadítása a lejárati időbélyeg után legfeljebb 5 másodperccel megtörténik
  (NFR-2).

## 7. Élhelyzetek és hibaviselkedés
- **Lejárat és fizetés versenye:** ha a fizetés a lejárattal egyszerre ér be, a rendszernek
  sorosított döntéssel vagy a fizetést kell érvényesítenie (a hely megmarad), vagy — ha a
  felszabadítás már megtörtént és a helyet más elvitte — a fizetést vissza kell utasítania
  terhelés nélkül. A pontos atomicitás a `design.md` és az `ADR` döntése (FR-3, FR-8).
- **Ismételt lejárat-feldolgozás:** egy már `expired`/`converted` Foglalás újbóli lejárat-eseménye
  nem okoz kontingens-változást (FR-3, FR-9).
- **0 kontingensű foglalás:** elutasítás Foglalás létrehozása nélkül (FR-4).

## 8. Adatok és interfészek
- `Hold { id, buyer_id, event_id, ticket_type_id, seat_ids[], quantity, status, created_at, expires_at, idempotency_key }`.
  Állapotok: `active → converted | expired | cancelled`.
- `POST /holds` (idempotencia-kulccsal), `DELETE /holds/{id}`, `GET /holds/{id}`.
- Függőség: az elérhető kontingens forrása a `0002`; a fizetés a `0005`.

## 9. Függőségek és feltételezések
- Függőségek: `0002-jegytipusok-arazas` (elérhető kontingens), `0005-penztar-fizetes` (konverzió).
- Feltételezés: a szerver órája pontos, szinkronizált időforráshoz igazított; a lejáratot a szerver
  dönti el.

## 10. Nyitott kérdések
*(nincs)*

## 11. Indoklás / döntések
- A fix 10 perces lejárat egyensúlyt tart a vásárlói kényelem és a kontingens forgása között; a
  fix vs. állítható döntés indoklása: `ADR-0001`.
- A lejárat–fizetés atomicitás a túlértékesítés elleni védelem magja; részletes döntés a designban.
