# Formát JSON (JavaScript Object Notation)

## 1. Historie a účel formátu

Formát **JSON** (JavaScript Object Notation) představuje celosvětový standard pro ukládání a výměnu strukturovaných dat.

* **Vznik:** Formát definoval a zpopularizoval americký programátor **Douglas Crockford** na začátku 21. století. Původně vycházel ze syntaxe programovacího jazyka JavaScript, ale rychle se vyvinul v univerzální, nezávislý formát. Dnes je oficiálně standardizován normami **RFC 8259** a **ECMA-404**.
* **Využití:** JSON se uplatňuje všude tam, kde je nutné přenášet složitější a vnořené struktury dat. Typicky slouží ke komunikaci mezi webovým serverem a aplikacemi (tzv. **REST API**), pro ukládání konfiguračních souborů (například nastavení v moderních vývojových prostředích nebo balíčků v Node.js) a jako nativní datové úložiště v moderních NoSQL databázích (například v MongoDB).

---

## 2. Základní syntaxe a datové typy

JSON má podobu obyčejného **textového souboru** s příponou `.json`. Je navržen tak, aby byl snadno srozumitelný lidem a zároveň bezproblémově zpracovatelný pro počítačové programy.

### Stavební kameny
1. **Objekt `{ ... }`:** Neuspořádaná kolekce dvojic `"klíč": hodnota` oddělených čárkou. Každý klíč musí být uzavřen v dvojitých uvozovkách.
2. **Pole `[ ... ]`:** Uspořádaný seznam hodnot oddělených čárkou, který umožňuje seskupovat více položek pod jeden název.

### Podporované datové typy
Přímo v syntaxi JSONu se rozlišuje několik základních typů hodnot:
* **Řetězec (`string`):** Text uzavřený výhradně v dvojitých uvozovkách (např. `"Praha"`).
* **Číslo (`number`):** Celé nebo desetinné číslo zapsané s desetinnou tečkou bez uvozovek (např. `42`, `3.14`, `-10`).
* **Logická hodnota (`boolean`):** Výhradně hodnoty `true` nebo `false` psané malými písmeny bez uvozovek.
* **Prázdná hodnota (`null`):** Slouží k vyjádření absence hodnoty nebo neznámého údaje.
* **Objekt (`object`):** Vnořená struktura v závorkách `{ ... }`.
* **Pole (`array`):** Vnořený seznam v závorkách `[ ... ]`.

---

## 3. Striktní pravidla zápisu

Programy, které JSON zpracovávají (tzv. parsery), vyžadují dodržování velmi přísné syntaxe. Jediná drobná chyba v zápisu způsobí, že soubor nepůjde vůbec načíst.

* **Pouze dvojité uvozovky:** Všechny klíče i textové hodnoty musí používat dvojité uvozovky (`"..."`). Jednoduché uvozovky nejsou povoleny.
* **Zákaz koncových čárek:** Za posledním prvkem v objektu nebo poli se nesmí nacházet čárka.
* **Žádné komentáře:** Standardní specifikace JSONu nepodporuje vkládání poznámek či komentářů (nelze používat zápisy typu `//` ani `/* ... */`).
* **Speciální znaky:** Pokud text obsahuje uvozovky, je nutné je uvozovat zpětným lomítkem (`\"`). Nový řádek se zapisuje jako `\n`.

---

## 4. Praktická ukázka: Zápis v JSON vs. stromová struktura

Následující příklad ukazuje záznam o produktu, který využívá vnořené objekty a pole pro uložení detailních parametrů a štítků.

### Zdrojový kód v JSON

```json
{
  "id": 101,
  "nazev": "Mechanická klávesnice \"Pro\"",
  "cena_czk": 2490.50,
  "skladem": true,
  "sleva": null,
  "parametry": {
    "rozlozeni": "CZ/SK",
    "podsviceni": "RGB",
    "hmotnost_g": 850
  },
  "stitek": [
    "herní",
    "drátová",
    "novinka"
  ]
}
```

### Vizualizace stromové hierarchie

* **Kořenový objekt**
  * `id`: `101` *(číslo)*
  * `nazev`: `"Mechanická klávesnice \"Pro\""` *(řetězec)*
  * `cena_czk`: `2490.5` *(číslo)*
  * `skladem`: `true` *(logická hodnota)*
  * `sleva`: `null` *(prázdná hodnota)*
  * `parametry` *(vnořený objekt)*
    * `rozlozeni`: `"CZ/SK"` *(řetězec)*
    * `podsviceni`: `"RGB"` *(řetězec)*
    * `hmotnost_g`: `850` *(číslo)*
  * `stitek` *(pole tří prvků)*
    * `[0]`: `"herní"`
    * `[1]`: `"drátová"`
    * `[2]`: `"novinka"`

---

## 5. Charakteristika a technická omezení

JSON slouží výhradně jako **datový nosič**, nikoliv jako programovací jazyk.

1. **Absence funkcí a logiky:** Soubor neobsahuje žádné výpočetní vzorce, podmínky ani spustitelný kód. Veškeré dodatečné výpočty nebo transformace dat musí zajistit program, který data načítá.
2. **Strukturovaná náročnost:** U jednoduchých rovných tabulek může být zápis zbytečně rozvláčný, protože se u každého záznamu opakují názvy klíčů. Na druhou stranu exceluje při práci s hierarchickými vztahy a vnořenými seznamy.
3. **Kódování znaků:** Veškerá data v JSONu se povinně ukládají v kódování **UTF-8**, což zajišťuje bezproblémovou čitelnost diakritiky napříč různými operačními systémy a platformami.
