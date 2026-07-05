---
type: problem-statement
title: Online jegyértékesítés túlértékesítés és foglalás-elakadás nélkül
status: draft            # draft | agreed | superseded
owner: termek-BA
updated: 2026-07-04
---

# Problem statement: Online jegyértékesítés túlértékesítés és foglalás-elakadás nélkül

> Egy probléma, még **azelőtt** megfogalmazva, hogy bárki megoldást választott volna.

## Kinek a problémája
Két, egymástól elváló szereplő:
- **Szervezők** (kis- és közepes koncert- és sportesemény-rendezők), akik jelenleg tabellás
  táblázatokban és külön fizetési linkeken kezelik az értékesítést.
- **Vásárlók**, akik népszerű eseményekre a nyitás első perceiben próbálnak jegyet venni.
- **Beléptetők** (helyszíni személyzet), akiknek a kapuban kell érvényesíteniük a jegyeket.

## Mi a probléma
A jelenlegi, kézi és több eszközön szétszórt folyamat két konkrét kárt okoz:
1. **Túlértékesítés:** ugyanazt a helyet vagy a kontingens utolsó darabjait több vásárló is
   megveszi, mert a foglalás és a fizetés nem egyetlen, közös készletállapot ellen dől el.
2. **Foglalás-elakadás:** a vásárló betesz jegyeket a kosárba, de ha nem fizet, a jegyek nem
   szabadulnak fel automatikusan, így a kontingens „szellemfoglalásokban” ragad, és mások nem
   tudnak venni.

## Miért számít (hatás)
- A túlértékesítés miatt a szervezőknek utólag kell jegyeket visszamondaniuk; a mért adat szerint
  a manuálisan kezelt eseményeken az eladott jegyek **3,2%-a** ütközik (dupla eladás vagy törölt
  hely), ami eseményenként átlagosan **41 vásárlói panasz** és kézi visszatérítés.
- A szellemfoglalások miatt a nyitás utáni első órában a kontingens **18%-a** kifizetetlen
  foglalásban áll, miközben a várólistán valós vásárlók vannak — ez elmaradt bevétel.
- A szétszórt eszközök miatt egy esemény beállítása jelenleg átlagosan **2,5 munkaóra** szervezői
  időt visz el.

## Sikermutató
- A túlértékesítési ütközés aránya < **0,1%** az eladott jegyekre vetítve.
- A nyitás utáni első órában a kifizetetlen foglalásban álló kontingens < **5%**.
- Egy átlagos esemény beállítása < **30 perc** szervezői idő.

## Megkötések és feltételezések
- A fizetés kizárólag PCI-DSS-tanúsított külső szolgáltatón keresztül történik; kártyaadatot nem
  tárolunk (lásd alkotmány Q-4).
- Feltételezzük, hogy a csúcsterhelés eseménynyitáskor ≤ 200 párhuzamos foglalási kérés/másodperc.
- Feltételezzük, hogy egy vásárló egy tranzakcióban legfeljebb 10 jegyet vásárol.

## Hatókörön kívül
- Másodlagos (viszonteladói) jegypiac.
- Dinamikus, keresletalapú árazás.
- Fizikai (nyomtatott) jegyek postázása.

## User story-k
- Szervezőként be akarom állítani egy eseményt (helyszín, időpont, kontingens) egy helyen, hogy ne
  kelljen több eszközt összehangolnom. → feloldja: `specs/0001-esemeny-kezeles`,
  `specs/0002-jegytipusok-arazas`.
- Vásárlóként ki akarom választani a helyemet egy térképen, hogy tudjam, pontosan hova ülök. →
  feloldja: `specs/0003-helyvalasztas`.
- Vásárlóként azt akarom, hogy a kosárba tett jegyeim adott ideig biztosan az enyémek legyenek, de
  utána szabaduljanak fel, hogy más is vásárolhasson. → feloldja: `specs/0004-foglalas-kosar`.
- Vásárlóként kártyaadat-tárolás nélküli fizetést akarok, ami után azonnal megkapom az érvényes
  jegyemet. →
  feloldja: `specs/0005-penztar-fizetes`, `specs/0006-jegykibocsatas`.
- Vásárlóként vissza akarom váltani a jegyemet, ha nem tudok elmenni, hogy visszakapjam a
  pénzemet. → feloldja: `specs/0007-visszavaltas`.
- Beléptetőként a helyszínen egyszer akarom érvényesíteni a jegyet, hogy ne lehessen ugyanazzal
  kétszer belépni. → feloldja: `specs/0008-belepteto-beolvasas`.
- Szervezőként valós idejű értékesítési kimutatást akarok, hogy lássam, mennyi fogyott. →
  feloldja: `specs/0009-szervezoi-riportok`.
