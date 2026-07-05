---
id: 0005-penztar-fizetes
title: Pénztár és fizetés
status: ready            # draft | in-review | ready | in-progress | done | superseded
owner: fizetesi-csapat
created: 2026-07-04
updated: 2026-07-04
need: ../../discovery/prd-event-ticketing.md#6-feature-bontás--specifikációk
supersedes: null
---

# Specifikáció: Pénztár és fizetés

> A szándékolt viselkedés pontos, tesztelhető leírása — a szerződés.

## 1. Összefoglaló és kontextus
A Pénztár a Kosárban lévő `active` Foglalásokat egy fizetéssel kifizetett Renddé alakítja. A
fizetés PCI-DSS-tanúsított külső szolgáltatón keresztül történik; a rendszer nem tárol kártyaadatot
(alkotmány Q-4). A folyamatnak idempotensnek kell lennie, hogy a szolgáltató kimaradása esetén az
újrapróbálás ne okozzon dupla terhelést. Származó szükséglet:
`../../discovery/prd-event-ticketing.md`.

## 2. Célok
- A Kosár Foglalásait egyetlen atomi fizetéssel Renddé alakítani.
- A fizetést idempotenssé tenni az újrapróbálás ellen.
- A hely elvesztése nélkül lehetővé tenni a sikertelen fizetés újrapróbálását a lejáratig.

## 3. Nem-célok (hatókörön kívül)
- A jegy kibocsátása és a QR-kód (lásd `0006-jegykibocsatas`).
- A visszatérítés (lásd `0007-visszavaltas`).
- Több pénznem és részletfizetés (MVP nem-cél).

## 4. Funkcionális követelmények
- **FR-1:** Amikor a vásárló elindítja a fizetést a Kosarára egy idempotencia-kulccsal, és a
  fizetési szolgáltató jóváhagyja a terhelést, a rendszernek `converted` állapotba kell állítania a
  Kosár összes Foglalását (`0004/FR-8`).
- **FR-2:** Ha a fizetési szolgáltató elutasítja a terhelést, akkor a rendszernek `payment_failed`
  eredményt kell adnia, meg kell tartania a Foglalásokat `active` állapotban a lejáratig, és nem
  szabad Rendet létrehoznia.
- **FR-3:** Ha ugyanazzal az idempotencia-kulccsal egynél több fizetési kérés érkezik, akkor a
  rendszernek nem szabad egynél több terhelést és egynél több Rendet létrehoznia (alkotmány P-4).
- **FR-4:** Ha a Kosár bármely Foglalása a fizetés jóváhagyása előtt lejárt vagy már nem `active`,
  akkor a rendszernek el kell utasítania a fizetés véglegesítését, és nem szabad megterhelnie a
  vásárlót.
- **FR-5:** Amikor a Rend létrejön `paid` állapotban, a rendszernek eseményt kell kibocsátania a
  jegygenerálás felé (`0006/FR-1`).
- **FR-6:** A rendszernek minden fizetési kísérletet naplóznia kell egy auditnaplóban a Rend
  azonosítójával, az eredménnyel és az időbélyeggel, kártyaadat tárolása nélkül.
- **FR-7:** Amikor a fizetési szolgáltató jóváhagyja a Kosár terhelését, a rendszernek létre kell
  hoznia egy `paid` állapotú Rendet a Kosár Foglalásaiból.
- **FR-8:** Ha egy már lejárt Foglalású Kosárra a szolgáltató mégis végrehajtott terhelést, akkor a
  rendszernek automatikus visszatérítést kell indítania (`0007/FR-1`).

## 5. Nem funkcionális követelmények
- **NFR-1:** A fizetés-véglegesítő művelet p95 válaszideje < 2000 ms (a külső szolgáltató
  válaszidejét is beleértve) 100 fizetés/másodperc mellett.
- **NFR-2:** A rendszer nyugalmi állapotban nem tárol teljes kártyaszámot (PAN); a fizetési adat
  kizárólag a szolgáltató tokenjeként jelenik meg.
- **NFR-3:** A fizetés-véglegesítés és a Foglalás-konverzió közötti adatkonzisztencia 100%: nincs
  olyan `paid` Rend, amelyhez ne tartozna `converted` Foglalás, és fordítva.

## 6. Elfogadási kritériumok / forgatókönyvek
```gherkin
Forgatókönyv: Sikeres fizetés Rendet hoz létre             # igazolja FR-1, FR-7, FR-5
  Adott egy Kosár két `active` Foglalással
  Amikor a vásárló fizet, és a szolgáltató jóváhagyja a terhelést
  Akkor a két Foglalás `converted` lesz, létrejön egy `paid` Rend, és indul a jegygenerálás

Forgatókönyv: Elutasított terhelés megtartja a foglalást   # igazolja FR-2
  Adott egy Kosár egy `active` Foglalással
  Amikor a szolgáltató elutasítja a terhelést
  Akkor nem jön létre Rend, és a Foglalás `active` marad a lejáratig

Forgatókönyv: Ismételt kérés nem terhel kétszer            # igazolja FR-3
  Adott egy már jóváhagyott fizetés az „ABC" idempotencia-kulccsal
  Amikor ugyanaz a kérés az „ABC" kulccsal újra megérkezik
  Akkor a rendszer az eredeti Rendet adja vissza, és nem történik második terhelés
```
- [ ] Lejárt Foglalásra érkező fizetés nem terheli a vásárlót (FR-4).
- [ ] Ha lejárt Foglalásra mégis történt terhelés, automatikus visszatérítés indul (FR-8).
- [ ] Minden fizetési kísérlet megjelenik az auditnaplóban kártyaadat nélkül (FR-6).
- [ ] Nincs `paid` Rend `converted` Foglalás nélkül és fordítva (NFR-3).
- [ ] A fizetés-véglegesítő művelet p95 válaszideje < 2000 ms 100 fizetés/másodperc terhelési teszt
  alatt (NFR-1).
- [ ] Nyugalmi állapotban egyetlen adatrekordban sem szerepel teljes kártyaszám (PAN); csak a
  szolgáltatói token (NFR-2).

## 7. Élhelyzetek és hibaviselkedés
- **Szolgáltatói időtúllépés:** ha a szolgáltató nem válaszol 20 másodpercen belül, a rendszernek
  a kérést függőben lévőként kell nyilvántartania, és a szolgáltató visszahívása (webhook) alapján
  kell véglegesítenie vagy elvetnie, dupla terhelés nélkül (FR-3, FR-4).
- **Részleges hiba:** ha a terhelés sikerült, de a Rend-írás nem, az idempotencia-kulcs alapján az
  újrapróbálás a meglévő terhelést használja, nem indít újat (FR-3).

## 8. Adatok és interfészek
- `Order { id, buyer_id, event_id, hold_ids[], amount_huf, status, provider_token, created_at }`.
  Állapotok: `paid → refunded | partially_refunded`.
- `POST /checkout` (idempotencia-kulccsal), `POST /payment-webhook`.
- Függőség: a Foglalások a `0004`; a jegygenerálás a `0006`; a visszatérítés a `0007`.

## 9. Függőségek és feltételezések
- Függőségek: `0004-foglalas-kosar`, külső PCI-DSS fizetési szolgáltató.
- Feltételezés: a szolgáltató biztosít idempotencia-kulcsot és fizetési webhookot.

## 10. Nyitott kérdések
*(nincs)*

## 11. Indoklás / döntések
- A fizetés-konverzió atomicitása és a lejárat–fizetés versenyhelyzet feloldása a `design.md`-ben
  és egy dedikált ADR-ben rögzül (fizetés–foglalás atomicitás).
- Az idempotencia-kulcs kötelező volta a dupla terhelés elleni fő védelem.
