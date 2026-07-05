# ADR-0001: Foglalás-lejárat stratégia

> Architektúra-döntési feljegyzés. Egy döntés fájlonként. **Elfogadás után változtathatatlan** — a
> döntés megváltoztatásához új, ezt felülíró ADR-t kell írni.

- **Státusz:** accepted
- **Dátum:** 2026-07-04
- **Döntéshozók:** architecture-guild, vasarloi-csapat
- **Kapcsolódó:** `specs/0004-foglalas-kosar` (FR-2), `discovery/prd-event-ticketing.md` §7 nyitott kérdés

## Kontextus
A Foglalásnak (Hold) lejárati ideje van, amely után a lekötött jegyek felszabadulnak
(`0004/FR-2`, `FR-3`, `FR-11`). A PRD nyitott kérdése volt, hogy a lejárat **fix** legyen-e minden
jegytípusra, vagy **jegytípusonként állítható**. A döntés érinti a kontingens forgási sebességét
(a szellemfoglalások elleni fő mérték: a kontingens < 5%-a álljon kifizetetlen foglalásban a nyitás
első órájában) és a rendszer bonyolultságát.

## Döntés
Az MVP-ben **fix, 10 perces** lejárati időt alkalmazunk minden jegytípusra. Az értéket
konfigurációként tároljuk (nem beégetett konstansként), hogy egy jövőbeli, ezt felülíró ADR
jegytípusonként állíthatóvá tehesse anélkül, hogy a kódot át kellene írni.

## Mérlegelt alternatívák
- **Jegytípusonként állítható lejárat** — elvetve: az MVP-ben nincs bizonyított igény rá; növeli a
  szervezői beállítás bonyolultságát és a tesztfelületet, miközben a fix érték is teljesíti a
  kontingens-forgási célt.
- **Nincs lejárat** — elvetve: pontosan ez okozza a szellemfoglalásokat, amelyeket a rendszernek
  fel kell számolnia (a kontingens 18%-a ragadt be a jelenlegi folyamatban).
- **Rövidebb, 2 perces lejárat** — elvetve: a fizetési űrlap kitöltése és a 3D-Secure gyakran
  meghaladja a 2 percet, ami indokolatlan foglalás-vesztést okozna.

## Következmények
- **Pozitív:** kiszámítható, egyszerűen tesztelhető viselkedés; megszünteti a szellemfoglalásokat;
  a 10 perc elég a fizetés befejezéséhez.
- **Negatív / kompromisszum:** a nagyon nagy keresletű eseményeknél a 10 perc lassíthatja a
  kontingens forgását; egyes VIP-folyamatok hosszabb időt szeretnének — ezt az MVP nem támogatja.
- **Követő lépések:** a nyitás utáni első órában mért kifizetetlen-kontingens arány figyelése; ha
  tartósan > 5%, a lejárat rövidítése vagy jegytípusonkénti állíthatóvá tétele új ADR-ben.
