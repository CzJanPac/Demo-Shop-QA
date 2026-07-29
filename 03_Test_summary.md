# 03_Test_summary.md

## Test Summary (shrnutí výsledků testování)

**Autor:** automaticky vygenerováno
**Repo:** Demo-Shop-QA
**Datum:** 29. 07. 2026

Krátké shrnutí: Testy pokryly prioritní oblasti e-shopu podle dokumentu `01_Testovací_analýza.md`. Během vykonání scénářů (viz `02_Test_report.md`) bylo nalezeno celkem 24 defectů popsaných v `Bug report/`.

### Agregované výsledky
- Celkový počet bug reportů: 24
- Rozdělení podle závažnosti (Severity):
  - Critical: 8
  - High: 5
  - Medium: 4
  - Low: 7

(Všechny bug reporty jsou uloženy v `Bug report/` jako jednotlivé soubory `BUG-###.md`.)

### Seznam bugů se Severity = Critical
- Bug report/BUG-003.md
- Bug report/BUG-007.md
- Bug report/BUG-008.md
- Bug report/BUG-010.md
- Bug report/BUG-011.md
- Bug report/BUG-014.md
- Bug report/BUG-017.md
- Bug report/BUG-021.md

Tyto chyby přímo ovlivňují kritické funkce: přesměrování z názvu e-shopu do Admin, odesílání objednávek bez povinných polí, neplatná validace emailu, nesprávná validace formátu telefonu, zásadní chyby v aplikaci slev (přepočet), chybné ukládání ceny v admin panelu a nemožnost dokončit objednávku pro položky skladem.

### Přehled výsledků podle testovacích scénářů (viz `02_Test_report.md`)
- TS-01: Přihlášení do e-shopu a správa relace — PASSED (0 chyb)
- TS-02: Práce s katalogem produktů a košíkem — FAILED (6 chyb)
- TS-03: Objednávkový formulář a validace prvků — FAILED (5 chyb)
- TS-04: Výpočet celkové ceny a vizuální promítnutí slev — FAILED (4 chyby)
- TS-05: Odeslání objednávky a reakce rozhraní — FAILED (3 chyby)
- TS-06: Administrace produktů v UI (CRUD operace) — FAILED (2 chyby)
- TS-07: Responzivita, layout a UI/UX — FAILED (4 chyby)

Součet nalezených chyb napříč scénáři: 24 (shoduje se s počtem souborů v `Bug report/`).

### Shrnutí rizik a dopadů
- Nejkritičtější problémy (Critical) blokují buď samotné dokončení objednávky nebo vedou k finančním nesrovnalostem (špatné slevy / nesprávné ceny). Dopad: vysoký finanční a reputační risk. Tyto chyby by měly mít prioritu opravy **P1 – CRITICAL**.
- High a Medium chyby (5 + 4) ovlivňují správnost obchodních funkcí (validace polí, export/CRUD v adminu) a mohou vést k chybným objednávkám či provozním potížím.
- Low chyby jsou převážně vizuální/UX a měly by být plánovány na opravu dle kapacit (kosmetika, překlepy, drobné layouty).

### Doporučení (krátce a konkrétně)
1. Okamžitě řešit všechny chyby se Severity = Critical (BUG-003,007,008,010,011,014,017,021). Koordinovat s vývojáři a nasadit hotfixy nebo plán oprav s opravdovým označením P1.
2. Po opravách Critical provést regresní běh pro TS-01…TS-05, s důrazem na scenáře Checkout a Výpočet ceny.
3. Opravit High chyby (5) do sprintu po Critical; zaměřit se na validace a export/CRUD v admin panelu (BUG-009,012,013,016,018).
4. Připravit E2E smoke test (automatizace) pro "Happy path" (dle `01_Testovací_analýza.md`) a spouštět ho v CI po každém nasazení; to zabrání návratu kritických regresů.
5. Aktualizovat `02_Test_report.md` o verifikaci po opravách a přidat sloupec "Verified (retest)" pro každé BUG-###.
6. U kritických chyb přiložit logy / API requesty / DB snapshoty (pokud jsou k dispozici) pro rychlejší lokalizaci příčiny.

---

Pokud chcete, mohu tento souhrn upravit (rozšířit) takto:
- doplnit krátký výpis každého critical bugu s jedním řádkem "co přesně se stane" (převzato z jednotlivých BUG-###.md),
- vygenerovat CSV/JSON s přehledem všech bugů (ID, Severity, Priority, Test scenario),
- vytvořit nový commit a přidat tento soubor do repozitáře jako `03_Test_summary.md`.

