# Testovací analýza: demoShop

**Autor:** Jan Páč

---

## 1. Rozdělení aplikace do testovacích oblastí a prioritizace

K prioritizaci jsem využil analýzu rizik. Vzhledem k nízké rozsáhlosti aplikace jsem nevytvořil matici rizik (*dopad na byznys × pravděpodobnost výskytu chyby = rizikové skóre*), ale rovnou určil priority dle vlivu na uskutečnění hlavního cíle aplikace (e-shopu), kterým je prodej produktů a generování zisku. Zohlednil jsem také počet uživatelů, které by potenciální chyba mohla ovlivnit.

### P1 – CRITICAL
Jedná se o oblasti, u kterých by chyby znemožnily uskutečnit nákup a odeslat objednávku. Tyto chyby by ovlivnily prakticky všechny uživatele a měly by tak největší dopad jak na finanční, tak na reputační stránku společnosti.

* **Přihlášení do e-shopu:** Vzhledem k tomu, že je přihlášení uvedeno ve Funkční specifikaci, zahrnuji jej do analýzy (určitě bych se zeptal zadavatele zda je to pouze přístup na testovací prostředí nebo zda je to záměr, aby se na eshop dostal jen omezený okruh lidí, např. firemní partneři apod.). Přihlášení je základ pro přístup do e-shopu. Pokud selže přihlašování, do aplikace se nedostanou ani zákazníci, ani zaměstnanci kvůli správě produktů v sekci Admin.
* **Hlavní stránka (katalog produktů + kategorie):** Nepostradatelná oblast, na které si zákazník prohlíží produkty, upravuje jejich množství, vkládá je do košíku a proklikává se do košíku. Hlavní akce zákazníka se provádějí zde.
* **Košík:** Mezikrok mezi hlavní stránkou a samotnou objednávkou. Informuje zákazníka o jeho vybraných produktech a cenách včetně celkové ceny za produkty. Je to jediná cesta, kterou lze pokračovat k objednávce.
* **Objednávkový formulář:** Vyplňují se zde povinné i nepovinné údaje o zákazníkovi, doručení i slevách. Pokud by selhal formulář, nebude možné zboží odeslat zákazníkovi a celý nákupní proces selže.
* **Výpočet celkové ceny (množství zboží, ceny, slevy, doprava):** Matematické operace vypočítávající celkovou cenu z několika vstupů (produkty, slevy, doprava, platební metoda). Je zde riziko nesprávného výpočtu podle zadaných vstupů a chyba v celkové částce by byla velmi kritická, ať už by se jednalo o podhodnocení, nebo nadhodnocení celkové ceny.
* **Odeslání objednávky a potvrzení:** Všechny výše zmíněné kroky vedou k odeslání objednávky, u které je klíčové, aby se úspěšně propsala do databáze se správnými daty. Bez toho by opět nebylo možné odeslat zboží na správnou adresu, vystavit účet (fakturu) a udržovat správné množství zboží na skladě. Přidal jsem zde i potvrzení objednávky, které je důležité, pro informování zákazníka o úspěšně odeslané objednávce. Bez tohoto potvrzení by mohl zákazník objednávku znovu vytvořit a vznikla by nechtěná duplicitní objednávka.

### P2 – HIGH
Správa produktů, tedy interní část aplikace, kterou používají zaměstnanci e-shopu. Chyba, v této části, může ovlivnit cenu zboží, skladové zásoby a omezit dostupnost zboží. Většina chyb ovšem nenaruší samotný nákupní proces zákazníka, ale ovlivní pouze zaměstnance e-shopu, kteří provedou úpravy o něco později po odstranění případné chyby.

* **Admin panel, CRUD operace (přidání, editace, smazání produktu), export:** Pokud nelze přidat nebo upravit produkt v danou chvíli (musí se počkat na opravu), tak to přímo to neovlivní nákupní proces, který lze provést i tak. Mohou nastat situace, kdy to může být kritické – například když je potřeba stáhnout z prodeje zboží, které už dodavatel nemá k dispozici, aby nedocházelo k vytváření objednávek, které nelze odbavit právě z důvodu nedostupnosti a nestažení zboží z prodeje. Export do XLS je drobná funkce, kterou nechávám jako součást této oblasti.

### P3 – MEDIUM
Důležité části, které nejsou nutné pro kompletní nákupní proces (vytvoření a odeslání objednávky), případně je možný *workaround* – tedy provést nákup jinou, i když třeba méně pohodlnou cestou.

* **Detail produktu:** Detail produktu je důležitý hlavně pro zákazníka, který má zájem o bližší informace o konkrétním produktu. Neovlivní tedy všechny zákazníky, ale jen určitou část. Navíc je možné provést vložení produktu do košíku i mimo detail zboží, a to na hlavní stránce s katalogem produktů.

### P4 – LOW
Do této kategorie jsem zařadil oblast, která nemá větší vliv na nákupní proces. Pokud se v ní vyskytnou chyby, ovlivní méně uživatelů a většinou nebrání vytvoření a dokončení objednávky.

* **Vzhled a použitelnost (UI/UX):** Jelikož se jedná o responzivní e-shop, zvolil jsem testování UI a UX jako samostatnou oblast. Při použití na mobilních zařízeních se často vyskytují různé chyby v rozložení prvků i použitelnosti. Tyto chyby obvykle nebývají kritické (tedy blokující používání aplikace), proto jsem zvolil nízkou prioritu testování.

---

## 2. Testovací strategie, techniky a úrovně testování

### Přihlášení do e-shopu
* **Integrační testování:** Validace API endpointů (status, response, doba odezvy)
* **Systémové testování:** Validace vstupních polí, neplatné kombinace
* **Techniky:** Ekvivalenční třídy

### Hlavní stránka (katalog produktů + kategorie)
* **Integrační testování:** Validace API endpointů (status, response, doba odezvy)
* **Systémové testování:** Správnost úpravy množství, vkládání do košíku, proklik na detail zboží, řazení, proklik do kategorie
* **Techniky:** Analýza hraničních hodnot, ekvivalenční třídy

### Košík
* **Integrační testování:** Validace API endpointů (status, response, doba odezvy)
* **Systémové testování:** Testování úprav v košíku, aktualizace okna, přidání zboží v jiném okně nebo záložce prohlížeče
* **Techniky:** Analýza hraničních hodnot, přechod stavů, ekvivalenční třídy

### Objednávkový formulář
* **Integrační testování:** Validace API endpointů (status, response, doba odezvy)
* **Systémové testování:** Validace vstupních polí formuláře
* **Techniky:** Ekvivalenční třídy, analýza hraničních hodnot, rozhodovací tabulky

### Výpočet celkové ceny (počet zboží, slevy, doprava)
* **Integrační testování:** Validace API endpointů (status, response, doba odezvy)
* **Systémové testování:** Testujeme správnost změny celkové ceny a shrnutí objednávky po změnách v dopravě, platbě nebo zadání kódu a také dodržení podmínek slev
* **Techniky:** Ekvivalenční třídy, analýza hraničních hodnot, rozhodovací tabulky

### Odeslání objednávky a potvrzení
* **Integrační testování:** Validace API endpointů (status, response, doba odezvy) a kontrola dat v databázi
* **Systémové testování:** Ověření vyplnění povinných polí, vyprázdnění košíku, nemožnost odeslat objednávku opakovaně po opakovaném kliknutí na odeslat
* **Techniky:** Ekvivalenční třídy, analýza hraničních hodnot, rozhodovací tabulky

### Admin panel, CRUD operace (přidání, editace, smazání produktu)
* **Integrační testování:** Validace API endpointů (status, response, doba odezvy) a kontrola dat v databázi
* **Systémové testování:** Celková správa produktů včetně exportu do XLS
* **Techniky:** Ekvivalenční třídy, analýza hraničních hodnot

### Detail produktu
* **Integrační testování:** Validace API endpointů (status, response, doba odezvy)
* **Systémové testování:** Úpravy množství, přidávání do košíku
* **Techniky:** Analýza hraničních hodnot

### Vzhled a použitelnost (UI)
* **Systémové testování:** Responzivita, kompatibilita napříč prohlížeči/zařízeními, čitelnost
* **Techniky:** Testování na základě checklistů (kontrolní seznam pro UI prvky)

Na všechny oblasti bych vyhradil určitý čas pro **Explorativní testování**, díky kterému je možné odhalit neočekávané chyby, mezery v logice používání aplikace, špatné ovladatelnosti nebo i nedostatky v uživatelské přívětivosti.

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
   * *Důvod:* Automatizace je zde ideální pro rychlé a stabilní testování s větším množstvím dat a různými variacemi vstupů. U automatizace nehrozí chyby v zadávání dat jako u manuálního provádění.

Tyto tři části jsou i vhodnými adepty na **Smoke testování** a nasazení do **CI/CD pipeline**.
