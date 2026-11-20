# Základy sazby v LaTeXu

## 1. Historie a účel systému

**LaTeX** je profesionální sázecí systém určený především pro tvorbu dokumentů s matematickými vzorci, technickou dokumentací, vědeckými články, diplomovými pracemi či knihami.

* **Vznik:** Na konci 70. let vytvořil počítačový vědec **Donald Knuth** sázecí systém **TeX** proto, aby dosáhl špičkové typografické kvality při sazbě své knižní série *The Art of Computer Programming*. V první polovině 80. let pak **Leslie Lamport** navrhl nadstavbu **LaTeX**, která usnadňuje strukturování celých dokumentů (kapitoly, seznamy, citace, křížové odkazy) a nechává typografická pravidla na systému.
* **Využití:** LaTeX se používá všude tam, kde záleží na přesné sazbě matematiky, vzorců, tabulek a logické struktury textu. Typicky jde o akademické texty, matematické a fyzikální materiály, technické zprávy, přednáškové poznámky nebo životopisy.

---

## 2. Základní struktura dokumentu a příkazy

LaTeX pracuje na principu **zdrojového textu** (obvykle se soubory `.tex`), který se **přeloží** (zkompiluje) do výsledného formátu, nejčastěji **PDF**. Na rozdíl od klasických textových editorů typu „co vidíš, to máš“ (WYSIWYG) píšete obsah i strukturální značky přímo do textu.

### Kostra dokumentu
Každý dokument má dvě hlavní části: **preambuli** (nastavení před začátkem samotného textu) a **tělo dokumentu** (uzavřené v prostředí `document`).

```latex
\documentclass[a4paper,12pt]{article}
\usepackage[utf8]{inputenc}
\usepackage[czech]{babel}

\begin{document}

\section{Úvod}
Toto je běžný odstavec textu v dokumentu.

\end{document}
```

### Základní stavební prvky
* **Příkazy:** Začínají zpětným lomítkem `\` (například `\section{...}`, `\textbf{...}`, `\emph{...}`). Povinné argumenty se píší do složených závorek `{...}`, nepovinné do hranatých `[...]`.
* **Prostředí (`environments`):** Ohraničují ucelený blok s odlišným chováním pomocí dvojice `\begin{nazev}` a `\end{nazev}`.
* **Odstavce a řádky:** Prázdný řádek ve zdrojovém kódu znamená nový odstavec. Samotný jeden přechod na další řádek ve zdrojovém kódu se při sazbě chová jen jako mezera.

---

## 3. Speciální znaky a základní typografie

Protože některé znaky mají v LaTeXu zvláštní význam, nelze je napsat přímo jako běžný text.

* **Vyhrazené znaky:** Znaky jako `#`, `$`, `%`, `&`, `_`, `{`, `}`, `~`, `^` nebo `\` plní řídicí či matematickou funkci. Například znak `%` uvozuje komentář až do konce řádku.
* **Zápis speciálních znaků:** Většinu řídicích znaků lze vysázet pomocí zpětného lomítka (například `\%`, `\&`, `\_`, `\{`, `\}`).
* **Komentáře:** Vše za znakem `%` překladač ignoruje, což se hodí pro poznámky autora přímo ve zdrojovém kódu.
* **Zvýraznění textu:** Pro tučné písmo se používá `\textbf{...}`, pro kurzívu `\emph{...}` (zdůraznění přizpůsobené kontextu) nebo `\textit{...}`.

---

## 4. Praktická ukázka: Zápis v LaTeXu vs. výsledek po překladu

Následující ukázka kombinuje strukturovaný text, seznam, tabulku a matematický vzorec, aby bylo vidět, jak se zdrojový kód promění ve vysázený dokument.

### Zdrojový kód v LaTeXu

```latex
\documentclass[a4paper,11pt]{article}
\usepackage[utf8]{inputenc}
\usepackage[czech]{babel}
\usepackage{amsmath}

\begin{document}

\section{Pohyb tělesa}

Rovnoměrně zrychlený přímočarý pohyb popisuje vztah pro dráhu:
\[
s = v_0 t + \frac{1}{2} a t^2
\]

Vlastnosti měření jsou přehledně shrnuta v tabulce:

\begin{center}
\begin{tabular}{|l|c|r|}
\hline
\textbf{Veličina} & \textbf{Značka} & \textbf{Jednotka} \\
\hline
Dráha & $s$ & $\mathrm{m}$ \\
Rychlost & $v$ & $\mathrm{m\cdot s^{-1}}$ \\
Zrychlení & $a$ & $\mathrm{m\cdot s^{-2}}$ \\
\hline
\end{tabular}
\end{center}

\end{document}
```

### Vizualizace po překladu do PDF

#### Pohyb tělesa

Rovnoměrně zrychlený přímočarý pohyb popisuje vztah pro dráhu:

$$s = v_0 t + \frac{1}{2} a t^2$$

Vlastnosti měření jsou přehledně shrnuta v tabulce:

| Veličina | Značka | Jednotka |
| :--- | :---: | ---: |
| Dráha | $s$ | $\mathrm{m}$ |
| Rychlost | $v$ | $\mathrm{m\cdot s^{-1}}$ |
| Zrychlení | $a$ | $\mathrm{m\cdot s^{-2}}$ |

---

## 5. Charakteristika a technická omezení

LaTeX je **sázecí a makro systém**, nikoliv běžný kancelářský textový procesor ani univerzální programovací jazyk.

1. **Nutnost překladu:** Zdrojový soubor `.tex` nelze otevřít jako hotový vizuální dokument; musí projít překladem (např. `pdflatex`, `xelatex` nebo `lualatex`), při kterém může dojít k syntaktickým chybám.
2. **Oddělení obsahu a vzhledu:** Autor se soustředí na logickou strukturu a obsah, zatímco detaily sazby řeší zvolená třída dokumentu a balíčky. Ruční zasahování do každého odstavce či pozice prvků jde často proti přirozenému chování systému.
3. **Složitost jemného ladění:** Ačkoliv jsou základní dokumenty a vzorce přímočaré, pokročilé úpravy rozvržení stránek, vlastních makier nebo nestandardních tabulek vyžadují znalost balíčků a chování překladače.
4. **Kódování a čeština:** V moderních distribucích se používá kódování **UTF-8** a balíček `babel` (nebo `polyglossia`), aby fungovala česká typografie, dělení slov a správné uvozovky.
