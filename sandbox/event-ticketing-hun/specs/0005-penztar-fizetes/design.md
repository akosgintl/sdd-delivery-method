---
id: 0005-penztar-fizetes
artifact: design
status: agreed            # draft | in-review | agreed | done
owner: fizetesi-csapat
updated: 2026-07-04
spec: ./spec.md
---

# Technikai terv: Pénztár és fizetés

> Hogyan elégítjük ki a `spec.md`-t. Ez a **hogyan**; a viselkedés a specifikációban van.

## 1. Áttekintés
A fizetést egy PCI-DSS-tanúsított külső szolgáltató végzi; a rendszer csak szolgáltatói tokent tárol.
A véglegesítés feltételes (compare-and-set) tranzakcióval konvertálja a Kosár Foglalásait és hozza
létre a `paid` Rendet, kizárólag ha a Foglalások a véglegesítés pillanatában `active`-ok
(`ADR-0002`). Az idempotencia-kulcs a dupla terhelés elleni fő védelem; a jegygenerálás és a
kényszer-visszatérítés tranzakciós outboxon megy.

## 2. Megközelítés és mérlegelt alternatívák
| Opció | Előnyök | Hátrányok | Döntés |
|-------|---------|-----------|--------|
| Feltételes (CAS) véglegesítés + outbox | nincs túlértékesítés és dupla terhelés | ritka „fizettél, de elveszett a hely” út | ✅ választott |
| Pesszimista zár a Foglalásokon a fizetés idejére | egyszerű | a hosszú zár rontja az átbocsátást, holtpont | elvetve |
| Kétfázisú commit a szolgáltatóval | erős garancia | a szolgáltató nem kínál kétfázisú commit (XA) protokollt | elvetve |

A véglegesítés atomicitása és a lejárat–fizetés versenyhelyzet: `../../adr/0002-fizetes-foglalas-atomicitas.md`.

## 3. Komponensek és felelősségek
- **CheckoutService** — a fizetés indítása, idempotencia (FR-3), a CAS-véglegesítés vezénylése
  (FR-1, FR-7), elutasítás lejárt Foglalásnál (FR-4).
- **PaymentGatewayAdapter** — a külső szolgáltató hívása, webhook-fogadás, token-kezelés (NFR-2).
- **RefundInitiator** — kényszer-visszatérítés indítása téves terhelésnél (FR-8) az outboxon át.
- **OrderStore** — `paid` Rend létrehozása (FR-7), audit (FR-6).
- **Outbox** — `order.paid` esemény a `0006` felé (FR-5), `refund.requested` a `0007` felé (FR-8).

## 4. Interfészek és szerződések
- `POST /checkout` (fejléc: `Idempotency-Key`) → véglegesítés — megvalósítja FR-1, FR-2, FR-3, FR-4,
  FR-7.
- `POST /payment-webhook` → szolgáltatói visszahívás — megvalósítja FR-3 (időtúllépés-feloldás), FR-8.
- Kimenő esemény: `order.paid` → `0006/FR-1`; `refund.requested` → `0007/FR-1`.

## 5. Adatmodell
- `Order(id, buyer_id, event_id, hold_ids[], amount_huf, status, provider_token, created_at)`,
  állapotok: `paid → refunded | partially_refunded`.
- `IdempotencyRecord(key, request_hash, order_id, result)`.
- `OutboxMessage(id, type, payload, status, created_at)`.
- Nyugalmi állapotban PAN nem tárolt (NFR-2); csak `provider_token`.

## 6. Integrációs pontok és függőségek
- `0004-foglalas-kosar`: a Foglalás-konverzió (FR-1) a QuotaStore CAS-ára épül (`ADR-0002`).
- `0006-jegykibocsatas`: az `order.paid` esemény indítja a jegygenerálást (FR-5).
- `0007-visszavaltas`: a kényszer-visszatérítés (FR-8) a visszaváltási folyamatot használja.
- Külső: PCI-DSS fizetési szolgáltató (idempotencia-kulcs + webhook).

## 7. Teszt-stratégia
- FR-1 → integrációs teszt: jóváhagyott terhelés → minden Hold `converted`.
- FR-2 → integrációs teszt: elutasított terhelés → nincs Rend, Hold `active`.
- FR-3 → teszt: azonos idempotencia-kulcs → egy terhelés, egy Rend.
- FR-4 → konkurrens teszt: lejárt Hold → véglegesítés elutasítva, nincs terhelés.
- FR-5 → szerződéses teszt: `paid` → `order.paid` esemény.
- FR-6 → teszt: minden kísérlet auditban, PAN nélkül.
- FR-7 → integrációs teszt: jóváhagyás → `paid` Rend.
- FR-8 → hibainjektálásos teszt: lejárt Holdra terhelés → auto-visszatérítés.
- NFR-1 → terheléses teszt: p95 < 2000 ms 100 fizetés/s.
- NFR-2 → biztonsági szkennelés + adatbázis-audit: nincs PAN nyugalmi állapotban.
- NFR-3 → konzisztencia-teszt: nincs `paid` Rend `converted` Hold nélkül és fordítva.

## 8. Kockázatok és mérséklések
| Kockázat | Valószínűség | Hatás | Mérséklés |
|----------|--------------|-------|-----------|
| Szolgáltatói időtúllépés | közepes | függő fizetés | webhook-alapú véglegesítés, függő állapot |
| Kettős webhook | közepes | dupla feldolgozás | idempotencia-kulcs, outbox dedup |
| „Fizettél, de elveszett a hely” | alacsony | rossz élmény | azonnali auto-visszatérítés (FR-8), tájékoztatás |

## 9. Kivezetés és üzemeltethetőség
- Feature flag: `checkout_v2`.
- Migráció: `Order`, `IdempotencyRecord`, `OutboxMessage` táblák.
- Megfigyelhetőség: metrika a fizetési sikerarányra, az auto-visszatérítésekre, a webhook-késésre,
  a konzisztencia-őr (NFR-3) sértésére; riasztás. Visszaállítás: `checkout_v2` kikapcsolása a régi
  szinkron útra vált vissza.
