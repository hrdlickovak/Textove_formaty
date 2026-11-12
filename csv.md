# Formát CSV (Comma-Separated Values)

## 1. Historie a účel formátu

Formát **CSV** (Comma-Separated Values neboli hodnoty oddělené čárkou) patří k nejstarším a nejjednodušším způsobům ukládání tabulkových dat. 

* **Vznik:** Formát nemá jednoho konkrétního autora ani oficiální specifikaci ze 70. let. Vznikl organicky jako praktická potřeba přenášet data mezi různými sálovými počítači a databázovými systémy. První široce rozšířené použití se datuje do éry tabulkového procesoru **VisiCalc** (konec 70. let) a později programů **Microsoft Multiplan** a **Lotus 1-2-3**. První formální snaha o standardizaci proběhla až v roce 2005 (dokument RFC 4180).
* **Využití:** Díky své jednoduchosti a univerzálnosti se používá všude tam, kde je potřeba rychle a bez zbytečné režie přenést tabulková data. Typicky jde o exporty a importy z e-shopů, migrace databází, zálohy kontaktů, bankovní výpisy nebo sdílení datasetů ve vědě a datové analýze (např. na portálech otevřených dat).

---

## 2. Základní struktura souboru

CSV je obyčejný **textový soubor** (obvykle s příponou `.csv`). Každý řádek v souboru odpovídá jednomu řádku v tabulce (záznamu) a jednotlivé sloupce jsou od sebe oddělené **oddělovačem** (nejčastěji čárkou, v českém prostředí kvůli používání desetinné čárky v číslech velmi často středníkem).

### Základní pravidla
1. **Hlavička:** První řádek souboru obvykle obsahuje názvy sloupců.
2. **Oddělovač:** Hodnoty na řádku jsou oddělené zvoleným znakem (čárka `,` nebo středník `;`).
3. **Konec řádku:** Každý řádek tabulky končí novým řádkem (`Enter`).

---

## 3. Práce se speciálními znaky (uvozovkování)

Pokud buňka v tabulce obsahuje text, ve kterém se nachází sám oddělovač (např. čárka v adrese), uvozovky, nebo je rozepsaná na více řádků, použije se pravidlo **uzovkování (quoting)**:

* Pokud hodnota obsahuje speciální znak (oddělovač, nový řádek, uvozovky), celý obsah buňky se uzavře do **dvojitých uvozovek (`"..."`)**.
* Pokud hodnota obsahuje samotné uvozovky jako součást textu, zapisují se zdvojením (`""`).

---

## 4. Praktická ukázka: Zápis v CSV vs. výsledek po importu

Níže je ukázka tabulky obsahující běžná data i složitější texty (čárky v textu, uvozovky, víceřádkový popis a prázdnou buňku).

### Zdrojový kód v CSV (oddělovač čárka)

```csv
ID,Nazev_produktu,Kategorie,Cena_CZK,Skladem,Poznamka
101,Mechanická klávesnice,Příslušenství,2490,Ano,"Podsvícení RGB, české rozložení"
102,"Monitor 27"", 4K",Monitory,8990,Ano,Včetně HDMI kabelu
103,USB-C Hub,Příslušenství,850,Ne,
104,Ergonomická myš,Příslušenství,1290,Ano,"Vhodná pro praváky.
Záruka 36 měsíců."
105,"Podložka pod myš ""XL""",Příslušenství,450,Ano,"Rozměry 90x40 cm, obšité okraje"
```

### Vizualizace po importu do tabulkového procesoru

| ID | Nazev_produktu | Kategorie | Cena_CZK | Skladem | Poznamka |
| :--- | :--- | :--- | :--- | :--- | :--- |
| 101 | Mechanická klávesnice | Příslušenství | 2490 | Ano | Podsvícení RGB, české rozložení |
| 102 | Monitor 27", 4K | Monitory | 8990 | Ano | Včetně HDMI kabelu |
| 103 | USB-C Hub | Příslušenství | 850 | Ne | |
| 104 | Ergonomická myš | Příslušenství | 1290 | Ano | Vhodná pro praváky.<br>Záruka 36 měsíců. |
| 105 | Podložka pod myš "XL" | Příslušenství | 450 | Ano | Rozměry 90x40 cm, obšité okraje |

---

## 5. Omezení formátu: Vzorce, formátování a kódování

CSV je **čistě datový a textový formát**. Nemá žádné výpočetní jádro a neukládá informace o vzhledu.

1. **Žádné vzorce:** Do CSV nelze uložit aktivní vzorec typu `=SUMA(D2:D6)`. Pokud jej do souboru zapíšete, cílový program (např. Excel nebo Calc) ho načte pouze jako obyčejný textový řetězec, nikoliv jako výpočet. Výpočty se provádějí až v aplikaci, která data zpracovává (v tabulkovém procesoru po importu, v SQL databázi nebo v programovacím jazyce, např. v Pythonu pomocí knihovny `pandas`).
2. **Žádné vizuální formátování:** CSV nepodporuje barvy buněk, tučné písmo, ohraničení, šířku sloupců ani slučování buněk.
3. **Absence datových typů:** Všechny hodnoty jsou v souboru uloženy jen jako prostý text. Program, který CSV otevírá, musí sám odhadnout, zda je hodnota `2490` číslo, datum, nebo textový řetězec.
4. **Kódování znaků (Encoding):** Častým problémem při otevírání CSV v českém prostředí je poškozená diakritika (tzv. *mojibake* – místo háčků a čárek se zobrazí nesmyslné znaky). Aby čeština fungovala správně napříč systémy (zejména v MS Excel), doporučuje se ukládat CSV v kódování **UTF-8 s BOM** (Byte Order Mark).
