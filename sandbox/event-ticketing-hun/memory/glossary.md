# Glosszárium — Egységes nyelvezet (Ubiquitous Language)

> A domain-fogalmak elfogadott definíciói, amelyeket a specifikációk, a kód, a tesztek és az
> AI-ügynökök közösen használnak. A fogalmak kétértelműsége a specifikáció kétértelműsége. Rögzített,
> jól ismert útvonalon él.

| Fogalom | Definíció | Megjegyzés / kerülendő szinonimák |
|---------|-----------|-----------------------------------|
| Esemény | Egy adott helyszínen és időpontban megtartott, jegyekkel látogatható rendezvény, amely a jegyértékesítés legfelső szintű egysége. | nem „koncert”, nem „show” |
| Helyszín | Fizikai vagy virtuális hely, ahol az esemény zajlik; egy vagy több szektorra és kapacitásra bontható. | nem „terem”, nem „aréna” önmagában |
| Jegytípus | Egy eseményhez tartozó, névvel, árral és kontingenssel bíró eladható kategória (pl. „Állóhely”, „VIP”). | nem „kategória”, nem „tarifa” |
| Kontingens | Egy jegytípushoz rendelt, eladható darabszám felső korlátja. | nem „készlet” általánosan, nem „limit” |
| Ülőhely | Egy szektoron belül egyedileg azonosított, kiválasztható pozíció (sor + szék). | nem „szék” önmagában |
| Foglalás (Hold) | Egy vásárló nevére ideiglenesen lekötött jegy vagy ülőhely, amely a lejárati időn belül fizetéssé alakítható, utána felszabadul. | nem „rezerváció”, nem „lefoglalt jegy” |
| Lejárati idő (Hold) | Az az időtartam, ameddig egy Foglalás fizetés nélkül fennáll, mielőtt automatikusan felszabadul. | nem „időkorlát” általánosan |
| Kosár | Egy vásárló aktuális, még ki nem fizetett Foglalásainak összessége a pénztár előtt. | nem „bevásárlókosár”, nem „rendelés” |
| Pénztár | A fizetés lezárását végző folyamat, amely a Kosár Foglalásait kifizetett Renddé alakítja. | nem „fizetés” önmagában, nem „checkout” magyar szövegben |
| Rend | Egy sikeres fizetés után létrejött, egy vagy több kifizetett jegyet tartalmazó, visszakereshető vásárlási egység. | nem „megrendelés” váltakozva, nem „tranzakció” |
| Kibocsátott jegy | Egy Rendhez tartozó, egyedi QR-kóddal ellátott, beléptetésre érvényes elektronikus jegy. | nem „belépő”, nem „voucher” |
| Visszaváltás | Egy kifizetett jegy érvénytelenítése és a hozzá tartozó összeg részleges vagy teljes visszatérítése. | nem „lemondás” önmagában, nem „sztornó” |
| Visszatérítés | A Visszaváltás pénzügyi része: a vásárlónak visszautalt összeg. | nem „jóváírás” váltakozva |
| Beléptetés | A Kibocsátott jegy QR-kódjának a helyszínen történő beolvasása és egyszeri érvényesítése. | nem „beolvasás” önmagában |
| Szervező | Az a szerep, amely eseményt hoz létre, jegytípusokat és árakat állít be, és értékesítési riportot lát. | nem „adminisztrátor” általánosan |
| Vásárló | Az a szerep, amely jegyet foglal, fizet és beléptetésre használ. | nem „felhasználó” általánosan |
| Idempotencia-kulcs | A kliens által megadott egyedi azonosító, amely biztosítja, hogy egy pénzügyi vagy készlet-művelet ismétlése ne okozzon többszörös hatást. | nem „tranzakció-azonosító” |
| Elérhető kontingens | Egy Jegytípus pillanatnyilag eladható darabszáma: a Kontingens és a lekötött (eladott + foglalt) jegyek különbsége; a túlértékesítés elleni egyetlen igazságforrás. | nem „szabad készlet”, nem „maradék”; nem azonos a Kontingenssel (az a felső korlát) |
| Auditnapló | Append-only, utólag nem módosítható napló, amely egy állapotváltozáshoz vagy műveleti kísérlethez rögzíti a cselekvő azonosítóját, a régi/új állapotot vagy az eredményt és az időbélyeget. | nem „napló” önmagában, nem „log” — egy szó használandó |
| Kimutatás | A szervezőnek eseményenként megjelenített, közel valós idejű értékesítési és beléptetési összesítés. | a „riport” és „kimutatás” váltakozó használata kerülendő |

*Szabály: ha egy szó egy specifikációban két olvasónak két dolgot jelenthet, itt kell definiálni,
vagy le kell cserélni.*

<!-- A javasolt bejegyzéseket (a glossary-maintainer fűzi hozzá, megerősítésre várva) `status: proposed` jelöli. -->
