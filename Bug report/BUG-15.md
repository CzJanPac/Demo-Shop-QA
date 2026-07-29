# [BUG-015] Potvrzení objednávky: Překlep v popisku celkové ceny ("Totl:" místo "Total:")

* **ID chyby:** BUG-015
* **Aplikace / Sekce:** demoShop / Potvrzení objednávky
* **Závažnost (Severity):** Low
* **Priorita (Priority):** Low
* **Prostředí:** Windows 11, Google Chrome v126
* **Datum:** 29. 07. 2026

### Popis chyby
Na stránce s potvrzením a souhrnem objednávky (Order Confirmed!) je u konečné ceny překlep. Místo správného anglického výrazu "Total:" je zobrazen text "Totl:".

### Předpoklady
1. Uživatel je v sekci Checkout, ve kterém má vyplněné všechny povinné položky.

### Kroky k reprodukci
1. Dokončit objednávkový proces kliknutím na **Pay**.
2. Na stránce **Order Confirmed!** v sekci **Order Summary** zkontrolovat popisek u výsledné sumy.

### Očekávané chování
Popisek celkové ceny je zobrazen správně jako **Total:**.

### Skutečné chování
Popisek celkové ceny obsahuje překlep a zobrazuje se jako **Totl:**.

### Přílohy
<img src="https://raw.githubusercontent.com/CzJanPac/Demo-Shop-QA/main/images/BUG-015.png" alt="BUG-015 Screenshot" width="100%" />
