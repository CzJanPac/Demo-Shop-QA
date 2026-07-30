## Test report
Výsledek provedeného testování. Samotné testovací scénáře, dle kterých bylo testování prováděno, jsou součástí testovací analýzy.

- Celkový počet bug reportů: 24
- Rozdělení podle závažnosti (Severity):
  - Critical: 8
  - High: 5
  - Medium: 4
  - Low: 7
 
### Seznam bugů se závažností (Severity) = Critical
- [BUG-003: Záhlaví: Kliknutí na název e-shopu "Demo Shop" přesměruje uživatele do Admin sekce](./Bug%20report/BUG-003.md)
- [BUG-007: Checkout: Objednávkový formulář lze odeslat bez vyplnění povinného pole "City"](./Bug%20report/BUG-007.md)
- [BUG-008: Checkout: Nedostatečná validace pole "Email" (akceptuje neplatnou TLD s jedním znakem)](./Bug%20report/BUG-008.md)
- [BUG-010: Checkout: Nesprávná validace formátu telefonního čísla (Slovensko akceptuje pouze 8 číslic)](./Bug%20report/BUG-010.md)
- [BUG-011: Checkout: Slevový kupón FLAT20 aplikuje nesprávnou výši slevy ($200 místo $20)](./Bug%20report/BUG-011.md)
- [BUG-014: Checkout: Seniorská sleva se aplikuje dříve (v 64 letech) a v chybné výši (10% místo 5%)](./Bug%20report/BUG-014.md)
- [BUG-017: Admin panel: Po úpravě produktu se uloží nesprávná cena (navyšuje se o 10 %)](./Bug%20report/BUG-017.md)
- [BUG-021: Checkout: Nelze dokončit objednávku u produktu skladem ("The following products cannot be bought")](./Bug%20report/BUG-021.md)


| ID Scénáře | Název testovacího scénáře | Výsledek | Počet chyb | Výstup |
| :--- | :--- | :---: | :---: | :--- |
| **TS-01** | Přihlášení do e-shopu a správa relace | 🟢 **PASSED** | 0 | Funkcionalita bez výhrad. Validace polí, reakce na neplatné údaje i zachování relace po F5 fungují správně. |
| **TS-02** | Práce s katalogem produktů a košíkem | 🔴 **FAILED** | 6 | Nalezeny chyby:<br>• [[BUG-001](<./Bug report/BUG-001.md>)] Katalog: Při vyfiltrování kategorie "Toys" se zobrazují produkty z kategorie "Audio"<br>• [[BUG-002](<./Bug report/BUG-002.md>)] Katalog: Řazení produktů "Name (Z-A)" zobrazuje položky nesprávně sežazené<br>• [[BUG-003](<./Bug report/BUG-003.md>)] Záhlaví: Kliknutí na název e-shopu "Demo Shop" přesměruje uživatele do Admin sekce<br>• [[BUG-004](<./Bug report/BUG-004.md>)] Košík: Celková cena v košíku zobrazuje nesprávný symbol měny (Euro místo Dolaru)<br>• [[BUG-005](<./Bug report/BUG-005.md>)] Detail produktu: Překlep v popisku značky ("Barnd" místo "Brand")<br>• [[BUG-006](<./Bug report/BUG-006.md>)] Detail produktu: Český výraz "Barva" v anglické verzi webu (místo "Color") |
| **TS-03** | Objednávkový formulář a validace prvků | 🔴 **FAILED** | 5 | Nalezeny chyby:<br>• [[BUG-007](<./Bug report/BUG-007.md>)] Checkout: Objednávkový formulář lze odeslat bez vyplnění povinného pole "City"<br>• [[BUG-008](<./Bug report/BUG-008.md>)] Checkout: Nedostatečná validace pole "Email" (akceptuje neplatnou TLD s jedním znakem)<br>• [[BUG-009](<./Bug report/BUG-009.md>)] Checkout: Nedostatečná validace maximální délky pole "Last Name" (umožňuje zadat více než 30 znaků)<br>• [[BUG-010](<./Bug report/BUG-010.md>)] Checkout: Nesprávná validace formátu telefonního čísla (Slovensko akceptuje pouze 8 číslic)<br>• [[BUG-020](<./Bug report/BUG-020.md>)] Checkout: Mezinárodní předvolba telefonního čísla je manuálně editovatelná a umožňuje zadat neplatné údaje |
| **TS-04** | Výpočet celkové ceny a vizuální promítnutí slev | 🔴 **FAILED** | 4 | Nalezeny chyby:<br>• [[BUG-011](<./Bug report/BUG-011.md>)] Checkout: Slevový kupón FLAT20 aplikuje nesprávnou výši slevy ($200 místo $20)<br>• [[BUG-012](<./Bug report/BUG-012.md>)] Checkout: Chybné hlášení u speciální nabídky (text uvádí slevu 50% místo reálně aplikovaných 20%)<br>• [[BUG-013](<./Bug report/BUG-013.md>)] Checkout: Nelze kombinovat studentskou/seniorskou slevu se slevovým kódem (v rozporu se specifikací)<br>• [[BUG-014](<./Bug report/BUG-014.md>)] Checkout: Seniorská sleva se aplikuje dříve (v 64 letech) a v chybné výši (10% místo 5%) |
| **TS-05** | Odeslání objednávky a reakce rozhraní | 🔴 **FAILED** | 3 | Nalezeny chyby:<br>• [[BUG-015](<./Bug report/BUG-015.md>)] Potvrzení objednávky: Překlep v popisku celkové ceny ("Totl:" místo "Total:")<br>• [[BUG-016](<./Bug report/BUG-016.md>)] Potvrzení objednávky: Sekce Order Summary nezobrazuje zakoupené produkty a aplikované slevy<br>• [[BUG-021](<./Bug report/BUG-021.md>)] Checkout: Nelze dokončit objednávku u produktu skladem ("The following products cannot be bought") |
| **TS-06** | Administrace produktů v UI (CRUD operace) | 🔴 **FAILED** | 2 | Nalezeny chyby:<br>• [[BUG-017](<./Bug report/BUG-017.md>)] Admin panel: Po úpravě produktu se uloží nesprávná cena (navyšuje se o 10 %)<br>• [[BUG-018](<./Bug report/BUG-018.md>)] Admin panel: Nově přidané produkty se nepropisují do XLSX exportu |
| **TS-07** | Responzivita, layout a UI/UX | 🔴 **FAILED** | 4 | Nalezeny chyby:<br>• [[BUG-019](<./Bug report/BUG-019.md>)] Hlavní stránka: Chybné odsazení (zarovnání) textu "In stock" u produktové karty<br>• [[BUG-022](<./Bug report/BUG-022.md>)] Checkout: Rozpadnutý mobilní layout u pole "Phone Number" (obří mezera a odskočená vlajka)<br>• [[BUG-023](<./Bug report/BUG-023.md>)] Detail produktu: Přetečení textu mimo kontejner karty na mobilním zařízení (Text Overflow)<br>• [[BUG-024](<./Bug report/BUG-024.md>)] Hlavní stránka: Chybějící název e-shopu v záhlaví a nadbytečné prázdné místo na mobilním zařízení |
