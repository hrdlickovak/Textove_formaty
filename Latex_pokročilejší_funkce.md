# Pokročilá sazba v LaTeXu: Balíčky a citace (BibLaTeX)

Zatímco základní LaTeX řeší strukturu textu a jednoduché vzorce, skutečnou sílu tohoto systému odemknete až pomocí **balíčků (packages)** a automatizované správy literatury. Tento průvodce představuje nejpoužívanější nástroje pro psaní bakalářských, diplomových a odborných prací.

---

## 1. Správa literatury a citací pomocí BibLaTeXu

Ruční psaní seznamu literatury a hlídání formátu citací v textu (např. podle normy ČSN ISO 690) je u delších prací velmi náchylné k chybám. **BibLaTeX** (ve spolupráci s procesorem **Biber**) tento proces plně automatizuje.

### Jak systém funguje?
1. **Databáze zdrojů (`.bib`):** Všechny knihy, články a weby uložíte do samostatného textového souboru (např. `literatura.bib`). Každý záznam má svůj unikátní **citační klíč**.
2. **Citování v textu (`.tex`):** Místo vypisování jmen a roků vložíte do textu příkaz `\cite{klic}`.
3. **Automatický seznam:** Na konci dokumentu příkaz `\printbibliography` sám vygeneruje seznam pouze těch zdrojů, které jste v textu skutečně použili, a správně je seřadí.

### Praktická ukázka: Zápis vs. výsledek

#### 1. Soubor s databází (`literatura.bib`)
```bibtex
@book{knuth1984,
  author    = {Donald E. Knuth},
  title     = {The TeXbook},
  year      = {1984},
  publisher = {Addison-Wesley},
  address   = {Reading, Massachusetts}
}

@online{github_docs,
  author    = {{GitHub, Inc.}},
  title     = {GitHub Docs: Writing on GitHub},
  year      = {2024},
  url       = {https://github.com},
  urldate   = {2024-11-15}
}
```

#### 2. Použití v hlavním dokumentu (`prace.tex`)
```latex
\documentclass[a4paper,12pt]{article}
\usepackage[utf8]{inputenc}
\usepackage[czech]{babel}
\(\usepackage{csquotes} \%\) Doporučeno pro správné české uvozovky v citacích

% Načtení balíčku biblatex s českou normou ISO 690
\usepackage[backend=biber, style=iso-numeric]{biblatex}
\(\addbibresource{literatura.bib} \%\) Připojení databáze

\(\begin{document}\)

Základy počítačové sazby položil Donald Knuth ve své slavné příručce \(\cite{knuth1984}.\) 
Pro moderní verzování dokumentů dnes často využíváme platformu GitHub \(\cite{github_docs}.\)

% Automatické vygenerování seznamu použité literatury
\printbibliography

\(\end{document} \%\%\)
```

#### 3. Vizualizace po překladu (styl `iso-numeric`)
> Základy počítačové sazby položil Donald Knuth ve své slavné příručce. Pro moderní verzování dokumentů dnes často využíváme platformu GitHub.
>
> **Literatura**
> 
> [1] KNUTH, Donald E. *The TeXbook*. Reading, Massachusetts: Addison-Wesley, 1984.
> 
> [2] GITHUB, INC. *GitHub Docs: Writing on GitHub* [online]. 2024 [cit. 2024-11-15]. Dostupné z: https://github.com

*(Poznámka: Změnou parametru `style=iso-numeric` na `style=iso-authoryear` se citace v celém dokumentu jedním kliknutím přepnou na formát `(Knuth, 1984)`).*

---

## 2. Nejpoužívanější balíčky pro odborné práce

Balíčky (knihovny) rozšiřují základní možnosti LaTeXu. Načítají se v preambuli dokumentu (před `\begin{document}`) pomocí příkazu `\usepackage[volitelné_parametry]{nazev_balicku}`. Níže je přehled těch nejdůležitějších rozdělených podle jejich účelu.

---

### 🎨 Vzhled stránky, typografie a navigace

#### 1. `geometry` – Nastavení okrajů a formátu papíru
* **K čemu slouží:** Výchozí okraje v LaTeXu jsou poměrně široké (optimalizované pro čitelnost úzkých sloupců v knihách). Pro bakalářské a diplomové práce však školy vyžadují přesné rozměry (např. širší levý okraj kvůli knižní vazbě).
* **Ukázka použití:**
  ```latex
  % Nastavení okrajů pro jednostranný tisk s prostorem pro vazbu vlevo
  \usepackage[a4paper, top=2.5cm, bottom=2.5cm, left=3.5cm, right=2cm]{geometry}
  ```

#### 2. `hyperref` – Aktivní klikací odkazy v PDF
* **K čemu slouží:** Promění vygenerovaný obsah (`\tableofcontents`), křížové odkazy na obrázky a rovnice (`\ref`), citace (`\cite`) i webové adresy (`\url`) v **živé hypertextové odkazy**. Vytvoří také záložky (osnovu) v prohlížeči PDF. *Důležité: V preambuli se načítá téměř vždy až jako poslední.*
* **Ukázka použití:**
  ```latex
  % Zapnutí barevných odkazů (vhodné pro elektronickou verzi práce)
  \usepackage[colorlinks=true, linkcolor=black, urlcolor=blue, citecolor=green]{hyperref}

  % Použití v textu:
  Více informací najdete na webu \(\url{https://ctan.org}\) nebo v \hyperref[sec:zaver]{Závěru}.
  ```

#### 3. `fancyhdr` – Vlastní záhlaví a zápatí stránek
* **K čemu slouží:** Umožňuje plně ovládat, co se zobrazí v horní a dolní části stránky (např. název aktuální kapitoly v záhlaví, číslo stránky v zápatí, oddělovací linky).
* **Ukázka použití:**
  ```latex
  \(\usepackage{fancyhdr} \pagestyle{fancy} \fancyhf{} \%\) Vymaže výchozí nastavení
  \fancyhead[L]{\leftmark} % Vlevo v záhlaví název aktuální kapitoly
  \fancyhead[R]{Bakalářská práce} % Vpravo v záhlaví fixní text
  \fancyfoot[C]{\thepage} % Uprostřed v zápatí číslo stránky
  ```

---

### 🖼️ Obrázky a profesionální tabulky

#### 4. `graphicx` a `float` – Vkládání a přesné umístění obrázků
* **K čemu slouží:** `graphicx` je naprostý základ pro vkládání externích souborů (`.png`, `.jpg`, `.pdf`). LaTeX se ale snaží obrázky automaticky přesouvat na typograficky nejvhodnější místo (tzv. plovoucí objekty). Balíček `float` přidává parametr `[H]` (*Here*), kterým systém donutíte nechat obrázek přesně tam, kde je v kódu.
* **Ukázka použití:**
  ```latex
  \(\usepackage{graphicx} \usepackage{float}\)

  \(\begin{figure}[H] \centering \includegraphics[width=0.75\textwidth]{images/graf-mereni.png} \caption{Závislost napětí na proudu při pokojové teplotě.} \label{fig:graf_mereni} \end{figure} \%\) Odkaz v textu: Průběh je patrný z Obrázku~\ref{fig:graf_mereni}.
  ```

#### 5. `booktabs` – Typograficky dokonalé tabulky
* **K čemu slouží:** Vědecké a odborné tabulky by podle typografických pravidel **neměly obsahovat svislé čáry** ani dvojité vodorovné čáry. Balíček `booktabs` nahrazuje obyčejné `\hline` třemi příkazy s různou tloušťkou čáry a lepším odsazením textu: `\toprule` (horní silná), `\midrule` (střední tenká) a `\bottomrule` (dolní silná).
* **Ukázka použití:**
  ```latex
  \(\usepackage{booktabs}\)

  \(\begin{table}[H] \centering \caption{Porovnání vlastností vybraných materiálů} \begin{tabular}{lcr} \toprule \textbf{Materiál} & \textbf{Hustota} & \textbf{Teplota tání} \\ \midrule\)
          Hliník & 2,70 & 660 \\
          Měď & 8,96 & 1085 \\
          Železo \(& 7,87 & 1538 \\ \bottomrule \end{tabular} \end{table} \%\%\)
  ```

---

### 📐 Matematika, fyzika a programování

#### 6. `amsmath` a `amssymb` – Pokročilá matematika
* **K čemu slouží:** Základní rozšíření od Americké matematické společnosti. Přináší prostředí pro zarovnání víceřádkových soustav rovnic pod sebe (pomocí znaku `&`), matice, determinanty a stovky speciálních symbolů (reálná čísla $\mathbb{R}$, parciální derivace $\partial$ atd.).
* **Ukázka použití:**
  ```latex
  \(\usepackage{amsmath, amssymb}\)

  % Prostředí align* nečísluje rovnice a zarovná je podle znaku \(& \begin{align*} f(x) &= (x + 2)^2 - 4 \\ &= x^2 + 4x + 4 - 4 \\ &= x(x + 4) \quad \forall x \in \mathbb{R} \end{align*} \%\%\)MAGIT_PARSER_PROTECT%%  ```

#### 7. `siunitx` – Typograficky správný zápis jednotek a čísel
* **K čemu slouží:** V českém textu se mezi číslem a jednotkou píše pevná mezera, jednotky se sázejí stojatým písmem (nikoliv kurzívou jako proměnné) a používá se desetinná čárka. Tento balíček vše ohlídá automaticky, a navíc umí zarovnávat čísla v tabulkách podle desetinné čárky.
* **Ukázka použití:**
  ```latex
  \(\usepackage{siunitx} \sisetup\){output-decimal-marker = {,}} % Nastavení české desetinné čárky

  % Použití v textu:
  Tíhové zrychlení je přibližně \(\qty{9.81}{\meter\per\second\squared}\) 
  a rychlost světla činí \(\qty{3e8}{\meter\per\second}. \%\%\)
  ```

#### 8. `listings` – Zdrojové kódy programů
* **K čemu slouží:** Pokud v práci uvádíte ukázky skriptů nebo algoritmů (např. v Pythonu, C++ nebo SQL), tento balíček je naformátuje neproporcionálním písmem, přidá číslování řádků, ohraničení a barevně zvýrazní klíčová slova.
* **Ukázka použití:**
  ```latex
  \(\usepackage{listings} \usepackage{xcolor} \%\) Pro definici barev

  \lstset{
      language=Python,
      basicstyle=\ttfamily\small,
      keywordstyle\(=\color{blue}\bfseries, commentstyle=\color{gray}\itshape,\)
      numbers=left,
      frame=single
  }

  \(\begin{lstlisting}\)[caption={Ukázka jednoduché funkce v Pythonu}]
  def faktorial(n):
      # Rekurzivní výpočet faktoriálu
      return 1 if n <= 1 else n * faktorial(n - 1)
  \(\end{lstlisting} \%\%\)
  ```


---

## 3. Jak vypadá preambule reálné bakalářské práce?

Následující šablona ukazuje, jak se výše uvedené balíčky kombinují v praxi do čisté a funkční preambule:

```latex
\documentclass[a4paper,12pt]{report}

% 1. Kódování a český jazyk
\usepackage[utf8]{inputenc}
\usepackage[T1]{fontenc}
\usepackage[czech]{babel}

% 2. Rozvržení stránky (okraje pro tisk a vazbu)
\usepackage[top=2.5cm, bottom=2.5cm, left=3.5cm, right=2cm]{geometry}

% 3. Matematika a fyzikální jednotky
\(\usepackage{amsmath, amssymb} \usepackage{siunitx} \sisetup\){output-decimal-marker = {,}} % Nastavení české desetinné čárky

% 4. Obrázky a tabulky
\(\usepackage{graphicx} \usepackage{booktabs} \usepackage{float}\)

% 5. Citace podle české normy
\(\usepackage{csquotes} \usepackage[backend=biber, style=iso-authoryear]{biblatex} \addbibresource{zdroje.bib}\)

% 6. Klikací odkazy v PDF (načítá se jako poslední)
\usepackage[colorlinks=true, linkcolor=black, urlcolor=blue, citecolor=black]{hyperref}

\(\begin{document} \%\) Zde začíná samotný text práce\(... \end{document} \%\%\)
```

---

## 4. Na co si dát u pokročilých funkcí pozor

1. **Víceprůchodový překlad:** Pokud používáte `biblatex`, křížové odkazy (`\ref`) nebo obsah (`\tableofcontents`), nestačí dokument přeložit jednou. Překladač musí proběhnout v cyklu (např. `pdflatex` $\rightarrow$ `biber` $\rightarrow$ `pdflatex` $\rightarrow$ `pdflatex`), aby se správně propojila čísla stránek a citací. Moderní editory (jako *Overleaf* nebo *TeXstudio*) to dělají automaticky.
2. **Pořadí balíčků:** Některé balíčky se mohou vzájemně ovlivňovat. Zlatým pravidlem je načítat balíček `hyperref` až na samém konci preambule.
3. **Práce s Overleafem vs. lokální instalace:** Pro týmovou spolupráci nebo rychlý start bez instalace je ideální online editor [Overleaf](https://overleaf.com), který má všechny zmíněné balíčky předinstalované. Pro offline práci se používá distribuce **TeX Live** (Windows/Linux) nebo **MacTeX** (macOS).
