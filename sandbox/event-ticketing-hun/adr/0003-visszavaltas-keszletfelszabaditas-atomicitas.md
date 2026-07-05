# ADR-0003: Visszaváltás–készletfelszabadítás atomicitása

> Architektúra-döntési feljegyzés. Egy döntés fájlonként. **Elfogadás után változtathatatlan.**

- **Státusz:** accepted
- **Dátum:** 2026-07-04
- **Döntéshozók:** architecture-guild, fizetesi-csapat
- **Kapcsolódó:** `specs/0007-visszavaltas` (FR-1, FR-6, FR-8, FR-9, NFR-2)

## Kontextus
A visszaváltáskor három hatásnak kell konzisztensnek lennie: a jegy `void` állapotba állítása
(`0007/FR-1`), a visszatérítés (`FR-8`) és az elérhető kontingens visszanövelése (`FR-9`). Az
NFR-2 megköveteli, hogy ne legyen olyan megfigyelhető pillanat, amelyben a jegy `void`, de a
kontingens nem szabadult fel, vagy fordítva. A szolgáltatói visszatérítés azonban aszinkron, és
sikertelen is lehet (`FR-6`), ezért nem tehető ugyanabba a lokális tranzakcióba.

## Döntés
A **lokális állapotváltozásokat** — jegy `void` + kontingens visszanövelése — **egyetlen
adatbázis-tranzakcióban** hajtjuk végre, így az NFR-2 invariáns teljesül. A **szolgáltatói
visszatérítést** külön, **tranzakciós outboxon** keresztül, idempotencia-kulccsal soroljuk be,
legalább-egyszer kézbesítéssel; a sikertelen visszatérítés `refund_pending` marad, és legalább 24
órán át újrapróbálódik (`FR-6`). A kontingens **azonnal**, a `void` pillanatában felszabadul, nem
várunk a szolgáltatói visszaigazolásra.

## Mérlegelt alternatívák
- **A kontingens felszabadítása csak a szolgáltatói visszatérítés megerősítése után** — elvetve: a
  szolgáltató késése vagy kimaradása addig „bent tartaná” az újra eladható helyet, ami ellentétes a
  feature céljával (a felszabaduló kontingens azonnal legyen eladható).
- **Szinkron szolgáltatói visszatérítés a lokális tranzakción belül** — elvetve: külső hálózati
  hívás egy adatbázis-tranzakcióban hosszú zárolást és részleges hibát kockáztat.

## Következmények
- **Pozitív:** a hely azonnal újra eladhatóvá válik; a visszatérítés végül garantáltan megtörténik;
  az NFR-2 atomicitás lokálisan biztosított.
- **Negatív / kompromisszum:** ritka esetben a sikertelen szolgáltatói visszatérítés úgy marad
  `refund_pending`, hogy a hely már újra eladásra került — a pénz **tartozás**, nem elveszett; a
  `refund_pending` kor alapján követett és riasztott.
- **Követő lépések:** riasztás a 24 óránál régebbi `refund_pending` visszatérítésekre; a
  szolgáltatói visszatérítési hibaarány figyelése.
