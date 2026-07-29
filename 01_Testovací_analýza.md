# Testovací analýza: demoShop

**Autor:** Jan Páč

---

## 1. Rozdělení aplikace do testovacích oblastí a prioritizace

K prioritizaci jsem využil analýzu rizik. Vzhledem k nízké rozsáhlosti aplikace jsem nevytvořil kompletní matici rizik (*dopad na byznys × pravděpodobnost výskytu chyby = rizikové skóre*), ale rovnou určil priority dle vlivu na uskutečnění hlavního cíle e-shopu (prodej produktů a generování zisku) a podle počtu zasažených uživatelů.

### Přehled prioritizace testovacích oblastí

| Priorita | Testovací oblast | Hlavní důvod zařazení (Byznys dopad) |
| :--- | :--- | :--- |
| **P1 – CRITICAL** | **Přihlášení do e-shopu** | Vstupní brána do aplikace. Selhání blokuje zákazníky i administrátory. |
| **P1 – CRITICAL** | **Katalog produktů & Kategorie** | Hlavní prodejní plocha. Selhání znemožní výběr zboží a vložení do košíku. |
| **P1 – CRITICAL** | **Košík** | Jediná cesta k objednávce. Selhání blokuje přechod k platbě. |
| **P1 – CRITICAL** | **Objednávkový formulář** | Sběr klíčových dat pro doručení a platbu. Selhání znemožní dokončit nákup. |
| **P1 – CRITICAL** | **Výpočet celkové ceny** | Finanční riziko. Chyba ve výpočtu slev/dopravy způsobuje přímou finanční ztrátu. |
| **P1 – CRITICAL** | **Odeslání a potvrzení** | Vytvoření objednávky v DB, odečet ze skladu a prevence duplicity (double-click). |
| **P2 – HIGH** | **Admin panel (CRUD operace & Export)** | Interní nástroj. Selhání omezí správu zboží, ale přímo neblokuje nakupující zákazníky. |
| **P3 – MEDIUM** | **Detail produktu** | Doplňkové informace pro zákazníka. Zboží lze vložit do košíku i přímo z katalogu (existuje workaround). |
| **P4 – LOW** | **Vzhled a použitelnost (UI/UX)** | Kosmetické a vizuální chyby. Málokdy přímo blokují dokončení nákupu. |

---

### Zdůvodnění priorit

#### P1 – CRITICAL
Kritické oblasti tvořící jádro nákupního procesu. Případná chyba zde plně znemožní dokončení objednávky, což má okamžitý dopad na tržby a reputaci e-shopu.

* **Přihlášení do e-shopu:** Vstupní brána do aplikace pro zákazníky i administrátory. Pokud selže autentizace, uživatelé se do e-shopu vůbec nedostanou. *(Pozn.: Doporučuji ověřit se zadavatelem, zda je přihlášení pouze ochranou testovacího prostředí, nebo požadovanou funkcí pro uzavřený okruh uživatelů).*
* **Katalog produktů & Kategorie:** Primární prodejní plocha, kde zákazník vybírá zboží a vkládá ho do košíku. Selhání zamezí začátku nákupního procesu.
* **Košík:** Nedílný mezikrok mezi výběrem zboží a objednávkou. Zobrazuje celkový přehled položek a je to jediná cesta k objednávkovému formuláři.
* **Objednávkový formulář:** Slouží ke sběru povinných údajů pro doručení a fakturaci. Nefunkčnost formuláře zcela blokuje odeslání objednávky.
* **Výpočet celkové ceny:** Matematické operace sčítající položky, slevy, dopravu a platbu. Nesprávně vypočítaná částka představuje vysoké finanční i právní riziko (podhodnocení i nadhodnocení ceny).
* **Odeslání a potvrzení objednávky:** Klíčové pro zápis dat do databáze, rezervaci skladu a vystavení faktury. Potvrzení objednávky je kritické i jako prevence vzniku duplicitních objednávek (když uživatel nebude vědět, že se objednávka odeslala tak zkusí vytvořit další).

#### P2 – HIGH
Interní část e-shopu určená pro správu katalogu ze strany zaměstnanců. Chyby v této sekci ovlivňují operativu obchodu, ale přímo neblokují nakupující zákazníky.

* **Admin panel (CRUD operace & Export):** Výpadek správy produktů dočasně zamezí úpravám cen či skladů. Může být kritický v situaci, kdy je nutné okamžitě stáhnout z prodeje vyprodané zboží. Export do XLS slouží jako doplňkový nástroj.

#### P3 – MEDIUM
Funkcionality důležité pro uživatelský komfort, u kterých však existuje náhradní řešení v případě selhání (*workaround*).

* **Detail produktu:** Poskytuje doplňkové informace o zboží. Pokud detail nefunguje, zákazník může produkt stále vložit do košíku přímo z hlavního katalogu.

#### P4 – LOW
Oblasti bez přímého vlivu na úspěšné dokončení nákupního procesu.

* **Vzhled a použitelnost (UI/UX):** Zahrnuje responzivitu na mobilních zařízeních a vizuální nedostatky. Tyto chyby většinou mají pouze estetický dopad a nebrání dokončení objednávky.

---

## 2. Testovací techniky a úrovně testování

| Oblast | Úrovně testování | Testovací techniky & Přístupy | Zdůvodnění volby (Proč právě tyto) |
| :--- | :--- | :--- | :--- |
| **1. Přihlášení do e-shopu** | • Integrační (API)<br>• Systémové (UI) | • Ekvivalenční třídy<br>• Chybové odhadování (*Error Guessing*) | Ekvivalenční třídy pro platné a neplatné kombinace jména a hesla. Error Guessing pro neobvyklé vstupy |
| **2. Hlavní stránka (Katalog)** | • Integrační (API)<br>• Systémové (UI) | • Analýza hraničních hodnot (BVA)<br>• Ekvivalenční třídy | Ověření zobrazení karet produktů a kategorií z DB. BVA na šipky +/- u změny množství (0 ks, 1 ks, max sklad) a tlačítko "Do košíku". |
| **3. Košík** | • Integrační (API)<br>• Systémové (UI) | • Přechod stavů (*State Transition*)<br>• Analýza hraničních hodnot (BVA) | State Transition pro chování košíku (prázdný vs. naplněný košík a dostupnost tlačítka k objednávce). BVA pro úpravu kusů přes +/- a odebrání při 0 ks. |
| **4. Objednávkový formulář** | • Integrační (API)<br>• Systémové (UI) | • Analýza hraničních hodnot (BVA)<br>• Ekvivalenční třídy | BVA pro limity vstupních polí (max 30 znaků, PSČ, telefon). Ekvivalenční třídy pro povinná/nepovinná pole a platné formáty (e-mail, datum narození). |
| **5. Výpočet celkové ceny** | • Jednotkové / API (Backend)<br>• Systémové (UI) | • Rozhodovací tabulky (*Decision Tables*)<br>• Analýza hraničních hodnot (BVA) | **Rozhodovací tabulka** pro kombinace slev (věk ≥ 65 let, student checkbox, slevové kódy, platba kartou). BVA pro věkovou hranici podle data narození (64, 65, 66 let). |
| **6. Odeslání a potvrzení** | • Integrační (API + DB)<br>• Systémové (UI) | • Ekvivalenční třídy<br>• Chybové odhadování (*Error Guessing*) | Ekvivalenční třídy pro úspěšné/neúspěšné odeslání (zápis do DB, vyprázdnění košíku, odečet ze skladu). Error Guessing pro prevenci duplicity (double-click). |
| **7. Admin panel (CRUD)** | • Integrační (API + DB)<br>• Systémové (UI) | • Ekvivalenční třídy<br>• Testování funkcionality (CRUD)<br>• Analýza hraničních hodnot (BVA)<br>• Chybové odhadování (*Error Guessing*) | Ověření kompletní správy produktů (přidání, úprava, smazání) a jejich promítnutí do DB a katalogu. BVA pro limity povinných parametrů a skladové zásoby. |
| **8. Detail produktu** | • Systémové (UI)<br>• Integrační (API) | • Analýza hraničních hodnot (BVA)<br>• Ekvivalenční třídy | Ověření správného načtení dat produktu z DB podle ID, funkčnosti šipek +/- pro množství a tlačítka "Zpět do obchodu". |
| **9. Vzhled a použitelnost (UI/UX)** | • Systémové (UI) | • Testování podle checklistů<br>• Cross-browser / Cross-device | Zaměření čistě na responzivitu, přetékání textů, čitelnost a ovladatelnost na mobilu a desktopu. |

---

## 3. Návrh automatizace

1. **E2E test pro kompletní nákupní proces (Happy path):**
   * *Průběh:* Přihlášení $\rightarrow$ vložení produktu do košíku $\rightarrow$ proklik z košíku do objednávky $\rightarrow$ vyplnění objednávkového formuláře $\rightarrow$ odeslání objednávky $\rightarrow$ kontrola zápisu objednávky v DB.
   * *Důvod:* Je to základ, který pokrývá nejčastější a nejběžnější postup zákazníka při nákupu.

2. **Admin panel, CRUD operace:**
   * *Průběh:* Přidání, upravení a smazání produktu.
   * *Důvod:* Patří mezi další základní a často používané funkce.

3. **Výpočet celkové ceny se zahrnutím slev, dopravy a platby kartou:**
   * *Průběh:* Testovat přes API mimo FE.
   * *Důvod:* Automatizace je zde ideální pro rychlé a stabilní testování s větším množstvím dat a různými variacemi vstupů. U automatizace nehrozí chyby v zadávání dat jako u manuálního provádění (využití Testovací pyramidy).

Tyto tři části jsou i vhodnými adepty na **Smoke testování** a nasazení do **CI/CD pipeline**. Pro přípravu testovacích dat a reset stavu před spuštěním automatizovaných testů lze využít testovací funkci *Reset application*.

---

## 4. Testovací scénáře pro manuální (explorativní) testování (UI / Systémové)

Vzhledem k omezenému času na provedení tohoto úkolu, nedostatečné dokumentaci a absenci specifikace API endpointů je testování zaměřeno výhradně na **systémové testování skrze uživatelské rozhraní (UI)**. K testování byla zvolena metoda **Explorativního testování řízeného testovacími scénáři**, kde scénáře tvoří rámec pro manuální průchody aplikací.

### TS-01: Přihlášení do e-shopu a správa relace (UI)
* **Cíl:** Ověřit funkčnost přihlašovacího formuláře a správy relace uživatele přímo v rozhraní.
* **Klíčové věci k otestování:** Vložení platných/neplatných údajů, reakce UI na chybějící data (chybové hlášky), chování polí při vložení mezer (whitespaces), viditelnost přihlášeného stavu a zachování relace po obnovení stránky (F5).

### TS-02: Práce s katalogem produktů a košíkem (UI)
* **Cíl:** Ověřit chování uživatelského rozhraní při výběru zboží, práci s množstvím a přechodu do košíku.
* **Klíčové věci k otestování:** Zobrazení kategorií, funkčnost tlačítek pro vložení do košíku, úprava počtu kusů pomocí šipek +/- v katalogu i v košíku, reakce UI na pokus o přidání více kusů než je na skladě, automatické zmizení položky z košíku při 0 ks, vysypání košíku, proklik z košíku k objednávce, proklik z katalogu do detailu, úprava množství v detailu a vložení do košíku, tlačítko "Zpět do obchodu".

### TS-03: Objednávkový formulář a validace prvků (UI)
* **Cíl:** Ověřit vizuální validaci a chování všech formulářových prvků v pokladně.
* **Klíčové věci k otestování:** Délkové limity polí (max 30 znaků), reakce na chybějící povinné údaje (červené hlášky/zvýraznění), správnost formátu e-mailu a PSČ, zadání data narození přes kalendář (Date Picker), dynamická změna předvolby u telefonu při změně země.

### TS-04: Výpočet celkové ceny a vizuální promítnutí slev (UI)
* **Cíl:** Ověřit, že UI správně a okamžitě přepočítává a zobrazuje celkovou cenu po aplikaci slev a voleb v pokladně.
* **Klíčové věci k otestování:**
  * Automatické uplatnění 5% slevy podle zadaného data narození (věk $\ge$ 65 let – hranice 64, 65, 66 let).
  * Zaškrtnutí checkboxu "I am a student" (15% sleva).
  * Zadávání slevových kódů (FREESHIP8, AUDIO20PC, FLAT20) a ověření chybové hlášky při pokusu zadat druhý kód (nekombinovatelnost).
  * Změna ceny po výběru platby kartou (5% sleva) a ověření, že se správně přičte/odečte k ostatním slevám.
  * Změna ceny po výběru dopravy (Pobočka $0, Box $5, Domů $15).

### TS-05: Odeslání objednávky a reakce rozhraní (UI)
* **Cíl:** Dokončit nákupní proces v UI, ověřit zobrazení potvrzovací obrazovky a návazné stavy.
* **Klíčové věci k otestování:** Zobrazení finálního souhrnu/potvrzení po kliknutí na odeslat, vyprázdnění košíku po úspěšném nákupu, blokace tlačítka odeslat proti opakovanému kliknutí (double-click).

### TS-06: Administrace produktů v UI (CRUD operace)
* **Cíl:** Ověřit správy produktového katalogu přes uživatelské rozhraní Admin panelu.
* **Klíčové věci k otestování:** Vyplnění formuláře pro nový produkt, úprava stávajícího produktu a vizuální kontrola, zda se změna ihned projevila v katalogu na e-shopu, smazání produktu a jeho zmizení z nabídky, stažení souboru při kliknutí na export do XLS.

### TS-07: Responzivita, layout a UI/UX
* **Cíl:** Ověřit vizuální správnost a použitelnost rozhraní na různých zařízeních a rozlišeních.
* **Klíčové věci k otestování:** Zobrazení na mobilním rozlišení, čitelnost textů, překrývání nebo přetékání prvků (tlačítka, formuláře, tabulky), ovladatelnost na dotykovém displeji vs. myší.
