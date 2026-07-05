# ADR-0002: Fizetés–foglalás konverzió atomicitása és a lejárat–fizetés versenyhelyzet

> Architektúra-döntési feljegyzés. Egy döntés fájlonként. **Elfogadás után változtathatatlan.**

- **Státusz:** accepted
- **Dátum:** 2026-07-04
- **Döntéshozók:** architecture-guild, fizetesi-csapat
- **Kapcsolódó:** `specs/0005-penztar-fizetes` (FR-1, FR-4, FR-7, FR-8), `specs/0004-foglalas-kosar` (FR-8)

## Kontextus
A fizetés jóváhagyásakor a Kosár összes Foglalását atomi módon `converted` állapotba kell állítani
(`0005/FR-1`, `0004/FR-8`), és `paid` Rendet kell létrehozni (`0005/FR-7`). A külső fizetési
szolgáltatói jóváhagyás azonban másodpercekig tarthat, és eközben egy Foglalás lejárhat
(`0004/FR-3`), a helyet más elviheti. Egyszerre kell megelőzni a **túlértékesítést** (a szabad
hely másnak is kiadva) és a **dupla terhelést**, valamint kezelni azt az esetet, amikor a
szolgáltató már terhelt, de a Foglalás közben lejárt.

## Döntés
A fizetés véglegesítését **feltételes (compare-and-set) tranzakcióval** végezzük: a Foglalásokat
csak akkor konvertáljuk, ha a véglegesítés pillanatában **mind** `active` állapotúak, a Foglalás
sor-verziója (optimista zárolás) alapján. A lejárat-söprő és a véglegesítés ugyanazon
sor-verzión versenyeznek. Ha a véglegesítés **veszít** (a Foglalás már `expired` és a helyet
elvitték), a véglegesítést elutasítjuk; ha a szolgáltató addigra már terhelt, automatikus
visszatérítést indítunk (`0005/FR-8` → `0007/FR-1`). A Rend-létrehozás és a konverzió egy
adatbázis-tranzakcióban történik; a jegygenerálási és visszatérítési eseményeket **tranzakciós
outboxon** keresztül, legalább-egyszer kézbesítéssel bocsátjuk ki.

## Mérlegelt alternatívák
- **Pesszimista zárolás a Foglalásokon a fizetés teljes ideje alatt** — elvetve: a fizetés
  másodpercekig tart, a hely/kontingens ilyen hosszú zárolása rontja az átbocsátást (a foglalási
  végpont NFR-1 célját), és holtpont-kockázatot hoz.
- **A Foglalás meghosszabbítása a fizetés indulásakor** — elvetve: támadó korlátlanul foglalva
  tarthatná a helyet, és a versenyhelyzetet így is kezelni kellene.
- **Kétfázisú commit (XA) a szolgáltatóval** — elvetve: a fizetési szolgáltató nem támogatja az
  elosztott tranzakciót.

## Következmények
- **Pozitív:** nincs túlértékesítés és nincs csendes dupla terhelés; a hely csak akkor kerül
  eladásra, ha a fizetés pillanatában is szabad; az outbox garantálja a jegygenerálást.
- **Negatív / kompromisszum:** ritka, a vásárló felé látható út marad: „a fizetés sikerült, de a
  hely közben elveszett → automatikus visszatérítés”. Ez rossz élmény, de nem jár pénzvesztéssel.
- **Követő lépések:** az automatikus visszatérítések arányának riasztásos figyelése; ha a versengő
  lejárat gyakori, a lejárati idő vagy a fizetési folyamat felülvizsgálata.
