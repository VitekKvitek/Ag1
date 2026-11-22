# 09. Amortizovaná analýza a dynamické pole – Zadání z písemek a řešení

Tento dokument obsahuje úlohy z písemek zaměřené na **amortizovanou časovou složitost**, **účetní (penízkovou) metodu**, **agregační metodu**, **potenciálovou metodu**, nafukovací dynamická pole a binární čítače.

---

## Úloha 9.1: Penízková (účetní) metoda pro dynamické pole s realokací $2n$

* **Zdroj:** FitWiki (Catay 2024, Knop 2023, Přednáška 4)
* **Relevantní skripta:** [Definice 4.4 (Amortizovaná časová složitost)](https://truepeak78.github.io/ag1/04_Bin%C3%A1rn%C3%AD_haldy/4.6_Amortizovan%C3%A1_%C4%8Dasov%C3%A1_slo%C5%BEitost/#def-4.4) · [Definice 4.6 a Věta 4.7 (Amortizovaná analýza nafukovacího pole)](https://truepeak78.github.io/ag1/04_Bin%C3%A1rn%C3%AD_haldy/4.6_Amortizovan%C3%A1_%C4%8Dasov%C3%A1_slo%C5%BEitost/#def-4.7)

### Zadání
Mějme dynamické pole (např. `std::vector`), které začíná s kapacitou 1 a počáteční velikostí 0.
Při operaci `PushBack(x)` se prvek zapíše na volnou pozici v čase $O(1)$ (cena 1 operace).
Pokud je kapacita vyčerpána, provede se realokace:
- Alokuje se nové pole s dvojnásobnou kapacitou ($2 \times \text{stávající kapacita}$).
- Všech $k$ stávajících prvků se zkopíruje do nového pole (cena $k$ operací).
- Staré pole se uvolní a nový prvek se vloží do nového pole.

Dokažte pomocí **penízkové (účetní) metody**, že amortizovaná časová složitost jedné operace `PushBack` je **$O(1)$** (konstantní).

> [!success]- Řešení
> ### Princip penízkové (účetní) metody
> Každé operaci přiřadíme tzv. **amortizovanou cenu** $\hat{c}_i$, kterou platíme virtuálními mincemi (kredity).
> Skutečná cena operace je $c_i$.
> - Pokud $\hat{c}_i > c_i$, přebytečný kredit $\hat{c}_i - c_i$ uložíme na konkrétní prvek datové struktury jako rezervu.
> - Pokud $\hat{c}_i < c_i$, drahá operace (např. realokace a kopírování) je zaplacena z dříve nastřádaných kreditů.
> Aby byla amortizovaná složitost platným horním odhadem, musí celkový nastřádaný kredit zůstat v každém okamžiku **nezáporný**:
> $$\sum_{i=1}^n \hat{c}_i \ge \sum_{i=1}^n c_i \quad \text{pro všechna } n \ge 1.$$
>
> ---
>
> ### Důkaz
> 1. **Stanovení skutečných nákladů $c_i$:**
>    - Obyčejné vložení bez realokace: $c_i = 1$ (zápis nového prvku do pole).
>    - Vložení vyžadující realokaci při zaplněné kapacitě $k$:
>      $$c_i = k + 1 \quad (k \text{ kopií starých prvků} + 1 \text{ zápis nového prvku}).$$
>
> 2. **Přiřazení amortizované ceny $\hat{c}_i$:**
>    Každé operaci `PushBack` naúčtujeme amortizovanou cenu:
>    $$\hat{\mathbf{c}}_i = \mathbf{3} \text{ mince}.$$
>
> 3. **Rozdělení mincí:**
>    - **1 mince:** okamžitě zaplatí skutečný zápis nově vkládaného prvku na konec pole ($c_{\text{zápis}} = 1$).
>    - **1 mince:** uloží se jako kredit na tento nově vložený prvek na budoucí zkopírování jeho samého při příští realokaci.
>    - **1 mince:** uloží se jako kredit na jeden ze starších prvků v první polovině pole, který už svůj kredit dříve vyčerpal.
>
> 4. **Bilanční ověření při realokaci:**
>    - Představme si stav bezprostředně po zdvojnásobení kapacity z $k$ na $2k$.
>    - Všechny prvky mají nulový kredit.
>    - Než dojde k další realokaci, musí se pole zcela zaplnit, což vyžaduje vložení dalších $k$ prvků.
>    - Každý z těchto $k$ nových prvků přinese se sebou 2 mince kreditu navíc (celkem $2k$ mincí).
>    - V okamžiku, kdy je pole o velikosti $2k$ zcela plné a je nutné realokovat na $4k$:
>      - Je potřeba zkopírovat $2k$ prvků $\implies$ skutečná cena je $2k$.
>      - K dispozici máme přesně $k \cdot 2 = 2k$ mincí nastřádaného kreditu!
>    - Celé kopírování $2k$ prvků je plně pokryto nastřádaným kreditem, na účtu nezůstane dluh.
>
> Protože v libovolném kroku $m$ platí $\sum_{i=1}^m \hat{c}_i \ge \sum_{i=1}^m c_i$ a $\hat{c}_i = 3 = O(1)$ pro každé $i$, je amortizovaná časová složitost operace `PushBack` rovna **$O(1)$**. $\blacksquare$

---

## Úloha 9.2: Aditivní zvětšování ($n + c$) vs. geometrické zvětšování ($\beta n$)

* **Zdroj:** FitWiki (Scholtzová 2020, Selftest 2018/19)
* **Relevantní skripta:** [Kapitola 4.6 (O amortizované analýze nafukovacího pole)](https://truepeak78.github.io/ag1/04_Bin%C3%A1rn%C3%AD_haldy/4.6_Amortizovan%C3%A1_%C4%8Dasov%C3%A1_slo%C5%BEitost/#def-4.6)

### Zadání
Uvažujte dvě různé strategie zvětšování kapacity dynamického pole při jeho zaplnění:
- **Strategie A (aditivní zvětšování):** Při vyčerpání kapacity se alokuje pole o velikosti větší o pevnou konstantu $c > 0$ (tedy nová kapacita je $\text{kapacita} + c$).
- **Strategie B (geometrické zvětšování):** Při vyčerpání kapacity se nová kapacita zvětší $\beta$-násobkem pro fixní reálné číslo $\beta > 1$ (např. $\beta = 2$ nebo $\beta = 1{,}5$).

1. Analyzujte celkovou časovou složitost posloupnosti $n$ operací `PushBack` začínající z prázdného pole pro Strategii A a odvoďte průměrnou (amortizovanou) cenu jedné operace.
2. Porovnejte výsledky se Strategií B a vysvětlete, proč Strategie A selhává.

> [!success]- Řešení
> ### 1. Analýza Strategie A (aditivní zvětšování o konstantu $c$)
> Nechť počáteční kapacita je $c$.
> Během posloupnosti $n$ vložení dojde k realokaci přibližně $k = \lfloor n/c \rfloor$-krát.
> Realokace nastávají v okamžicích, kdy pole obsahuje:
> $$c, 2c, 3c, 4c, \dots, k \cdot c \text{ prvků}.$$
>
> Spočítáme celkovou skutečnou práci $\sum_{i=1}^n c_i$ pomocí agregační metody:
> - Každý z $n$ prvků zaplatí 1 operaci za samotné vložení: $\sum 1 = n$.
> - Při $j$-té realokaci se kopíruje $j \cdot c$ prvků:
>   $$\text{Cena všech kopií} = \sum_{j=1}^{\lfloor n/c \rfloor} j \cdot c = c \sum_{j=1}^{\lfloor n/c \rfloor} j = c \cdot \frac{\lfloor n/c \rfloor (\lfloor n/c \rfloor + 1)}{2} \approx c \cdot \frac{n^2}{2 c^2} = \mathbf{\frac{n^2}{2c}}.$$
>
> Celkový čas pro $n$ operací:
> $$T(n) = n + \frac{n^2}{2c} = \mathbf{\Theta(n^2)}.$$
> Amortizovaná složitost jedné operace `PushBack`:
> $$\hat{c} = \frac{T(n)}{n} = \frac{\Theta(n^2)}{n} = \mathbf{\Theta(n)}.$$
>
> ---
>
> ### 2. Srovnání se Strategií B (geometrické zvětšování faktorem $\beta > 1$)
> U geometrické strategie s faktorem $\beta > 1$ nastávají realokace v okamžicích, kdy pole obsahuje $\beta^0, \beta^1, \dots, \beta^m$ prvků (kde $\beta^m \le n$).
> Celkový počet kopií tvoří geometrickou řadu:
> $$\sum_{j=0}^{m} \beta^j = \frac{\beta^{m+1} - 1}{\beta - 1} \le \frac{\beta \cdot n}{\beta - 1} = O(n).$$
> Celkový čas pro $n$ operací je tedy $T(n) = n + O(n) = \mathbf{\Theta(n)}$ a amortizovaná složitost je $\mathbf{O(1)}$.
>
> **Závěr:**
> Strategie A selhává, protože konstanta $c$ neumožňuje nastřádat dostatečný kredit pro stále větší a větší počet kopírovaných prvků. Počet realokací roste lineárně s $n$, což vede ke kvadratické celkové složitosti $\Theta(n^2)$.
> Geometrická strategie naopak zaručuje, že počet realokací je pouze logaritmický $O(\log_\beta n)$ a intervaly mezi realokacemi rostou úměrně velikosti pole.

---

## Úloha 9.3: Zmenšování pole při mazání a prevence amortizačního kolapsu (Thrashing)

* **Zdroj:** FitWiki (Selftest ZS 18/19, Přednáška 4)
* **Relevantní skripta:** [Definice 4.4 (Amortizovaná časová složitost)](https://truepeak78.github.io/ag1/04_Bin%C3%A1rn%C3%AD_haldy/4.6_Amortizovan%C3%A1_%C4%8Dasov%C3%A1_slo%C5%BEitost/#def-4.4) · [Definice 4.6 (Nafukovací pole)](https://truepeak78.github.io/ag1/04_Bin%C3%A1rn%C3%AD_haldy/4.6_Amortizovan%C3%A1_%C4%8Dasov%C3%A1_slo%C5%BEitost/#def-4.6)

### Zadání
Představme si naivní strategii pro zmenšování dynamického pole:
- Při zaplnění kapacity $k$ pole zvětšíme na $2k$ (zdvojnásobení).
- Jakmile po operaci `PopBack()` klesne počet prvků na polovinu kapacity ($k/2$), pole ihned zmenšíme na kapacitu $k/2$ (zmenšení na polovinu).

1. Ukažte posloupnost $m$ operací `PushBack` a `PopBack`, pro kterou tato naivní strategie vede k celkové časové složitosti $\Theta(m \cdot n)$, a tedy amortizovaná složitost jedné operace je $\Theta(n)$ (tzv. **thrashing / kmitání**).
2. Navrhněte správnou modifikaci podmínky pro zmenšení pole (princip hystereze), která zaručí amortizovanou složitost $O(1)$ pro obě operace `PushBack` i `PopBack`, a dokažte ji.

> [!success]- Řešení
> ### 1. Konstrukce patologické posloupnosti (Thrashing)
> Nechť je aktuální kapacita pole $N$ a v poli je právě $N$ prvků.
> Uvažujme střídavou posloupnost operací:
> 1. `PushBack(x)`:
>    - Kapacita $N$ je plná. Alokuje se nové pole velikosti $2N$, zkopíruje se $N$ prvků.
>    - Nová velikost: $N + 1$, kapacita: $2N$. Skutečná cena: $N + 1$.
> 2. `PopBack()`:
>    - Počet prvků klesne z $N+1$ na $N$, což je přesně polovina kapacity $2N$ ($N = \frac{2N}{2}$).
>    - Naivní pravidlo zavelí zmenšit pole na polovinu ($N$): alokuje se pole velikosti $N$, zkopíruje se $N$ prvků.
>    - Nová velikost: $N$, kapacita: $N$. Skutečná cena: $N$.
> 3. `PushBack(x)`:
>    - Kapacita $N$ je opět plná! Znovu alokace na $2N$ a kopírování $N$ prvků. Cena: $N+1$.
> 4. `PopBack()`:
>    - Znovu zmenšení na $N$ a kopírování $N$ prvků. Cena: $N$.
>
> Budeme-li střídat `PushBack` a `PopBack` $m$-krát:
> - Každá dvojice operací provede realokaci $N$ prvků za cenu $\approx 2N$.
> - Celková cena za $m$ operací je:
>   $$T(m) = m \cdot \Theta(N).$$
> Amortizovaná cena jedné operace je $\frac{T(m)}{m} = \mathbf{\Theta(N)}$, paměťové kmitání zničilo amortizovanou efektivitu.
>
> ---
>
> ### 2. Správné řešení pomocí hystereze (kvartální zmenšování)
> **Pravidlo:**
> - Zvětšujeme na dvojnásobek ($2k$), jakmile je kapacita $k$ plná.
> - Zmenšujeme na polovinu ($k/2$) teprve tehdy, když počet prvků klesne na **čtvrtinu kapacity** ($\frac{k}{4}$).
>
> **Důkaz amortizované složitosti $O(1)$ (Penízková metoda):**
> V libovolném okamžiku bezprostředně po realokaci (ať už zvětšení nebo zmenšení) je pole zaplněno **přesně z poloviny kapacity**:
> - Po zvětšení z $k$ na $2k$: pole má $k$ prvků v kapacitě $2k$ (zaplněno z $1/2$).
> - Po zmenšení z $k$ na $k/2$: pole má $k/4$ prvků v kapacitě $k/2$ (zaplněno z $1/2$).
>
> Aby došlo k další realokaci:
> - K dalšímu zvětšení musí přibýt $\frac{K}{2}$ prvků pomocí `PushBack`.
> - K dalšímu zmenšení musí ubyvat $\frac{K}{4}$ prvků pomocí `PopBack`.
>
> Nastavíme amortizované ceny:
> - $\hat{c}(\text{PushBack}) = 3$ mince (1 na zápis, 2 do rezervy na budoucí kopírování).
> - $\hat{c}(\text{PopBack}) = 2$ mince (1 na smazání, 1 do rezervy na budoucí kopírování při zmenšení).
>
> Než systém dosáhne hranice čtvrtiny ($k/4$), proběhne alespoň $k/4$ operací `PopBack`, z nichž každá uloží 1 minci kreditu.
> Na účtu bude k dispozici alespoň $k/4$ mincí, což přesně pokryje cenu zkopírování $k/4$ prvků do nového pole velikosti $k/2$.
> Amortizovaná složitost obou operací zůstává **$O(1)$**. $\blacksquare$

---

## Úloha 9.4: Amortizovaná složitost $k$-bitového binárního čítače

* **Zdroj:** FitWiki (Marast 2021, Přednáška 4)
* **Relevantní skripta:** [Definice 4.4 (Amortizovaná časová složitost)](https://truepeak78.github.io/ag1/04_Bin%C3%A1rn%C3%AD_haldy/4.6_Amortizovan%C3%A1_%C4%8Dasov%C3%A1_slo%C5%BEitost/#def-4.4)

### Zadání
Uvažujte $k$-bitový binární čítač reprezentovaný polem bitů $A[0 \dots k-1]$, který začíná s hodnotou $0$ (všechny bity jsou $0$).
Provádíme operaci `Increment()`, která přičte 1 k čítači podle algoritmu:
```text
Increment(A):
    i = 0
    while i < k and A[i] == 1:
        A[i] = 0
        i = i + 1
    if i < k:
        A[i] = 1
```
Cena jedné operace je rovna celkovému počtu bitů, které změnily hodnotu ($0 \to 1$ nebo $1 \to 0$).
Dokažte pomocí **účetní (penízkové) metody** i **agregační metody**, že posloupnost $n$ operací `Increment()` má celkovou časovou složitost $O(n)$ a amortizovaná cena jedné operace je **$O(1)$** (přestože nejhorší případ jedné operace je $O(k)$).

> [!success]- Řešení
> ### 1. Důkaz agregační metodou
> Sledujeme, jak často se jednotlivé bity mění během $n$ po sobě jdoucích inkrementací od 0:
> - Bit $A[0]$ (nultý řád $2^0$) mění hodnotu při **každé** operaci: celkem $n$-krát.
> - Bit $A[1]$ (první řád $2^1$) mění hodnotu při každé **druhé** operaci: celkem $\lfloor n/2 \rfloor$-krát.
> - Bit $A[2]$ (druhý řád $2^2$) mění hodnotu při každé **čtvrté** operaci: celkem $\lfloor n/4 \rfloor$-krát.
> - Obecně bit $A[j]$ mění hodnotu právě $\lfloor n / 2^j \rfloor$-krát.
>
> Celkový počet všech bitových překlopení za celou posloupnost $n$ operací:
> $$T(n) = \sum_{j=0}^{k-1} \left\lfloor \frac{n}{2^j} \right\rfloor < n \sum_{j=0}^{\infty} \frac{1}{2^j} = n \cdot \frac{1}{1 - 1/2} = 2n.$$
> Celkový čas je ostře shora omezen:
> $$T(n) < 2n = O(n).$$
> Amortizovaná složitost jedné operace:
> $$\hat{c} = \frac{T(n)}{n} < \frac{2n}{n} = \mathbf{2} = \mathbf{O(1)}.$$
>
> ---
>
> ### 2. Důkaz účetní (penízkovou) metodou
> - Každému nastavení bitu z $0$ na $1$ naúčtujeme **amortizovanou cenu 2 mince** ($\hat{c} = 2$).
>   - **1 mince:** zaplatí okamžitou práci za změnu bitu $0 \to 1$.
>   - **1 mince:** uloží se na daný bit jako rezerva na budoucí reset $1 \to 0$.
> - Když cyklus `while` resetuje bit z $1$ na $0$, neplatíme nic z nového kreditu: reset je zaplacen z mince, která leží na daném bitu od doby, kdy byl nastaven na 1.
> - Každá operace `Increment()` nastaví na 1 **nejvýše jeden bit** (v příkazu `A[i] = 1`).
> - Všechny ostatní změněné bity byly resetovány z $1$ na $0$ a jejich změna byla předplacena.
>
> Na každou operaci `Increment()` tedy stačí naúčtovat amortizovanou cenu $\hat{c} = 2$ mince.
> Celkový nastřádaný kredit na čítači je v každém okamžiku roven počtu jedničkových bitů v čítači, což je vždy $\ge 0$.
> Amortizovaná časová složitost je tedy striktně **$O(1)$**. $\blacksquare$

---

## Úloha 9.5: „Eliščino pole“ – náhodné vkládání a střední hodnota počtu posunů

* **Zdroj:** FitWiki (Písemka ZS 2019/2020, Šestáková, `uloha3.png`)
* **Relevantní skripta:** [Definice 7.7 a Věta 7.2 (Střední hodnota a linearita)](https://truepeak78.github.io/ag1/07_Randomizace_a_he%C5%A1ov%C3%A1n%C3%AD/7.5_Pravd%C4%9Bpodobnostn%C3%AD_prostor/#def-7.7) · [Kapitola 4.6 (Amortizovaná analýza)](https://truepeak78.github.io/ag1/04_Bin%C3%A1rn%C3%AD_haldy/4.6_Amortizovan%C3%A1_%C4%8Dasov%C3%A1_slo%C5%BEitost/)

### Zadání
Mějme datovou strukturu *„Eliščino pole“* o kapacitě $n$. Tato datová struktura udržuje prvky stále **vzestupně seřazené** v prvních $k$ přihrádkách.
Vyhledat správnou pozici pro aktuálně vkládaný prvek zvládne v čase $O(1)$.
Pokud však vkládá nový prvek na pozici $i$, musí nejprve všechny stávající prvky na pozicích $i, i+1, \dots, k$ posunout o jednu pozici doprava na pozice $i+1, i+2, \dots, k+1$. Každý posun jednoho prvku stojí 1 korunu (tedy posuneme tolik prvků, kolik je jich v poli momentálně větších než vkládané číslo).

Do původně prázdného *Eliščina pole* o kapacitě $n$ postupně vložíme čísla $1, 2, \dots, n$ ve **zcela náhodném pořadí** (na vstupu je rovnoměrně náhodně vybraná permutace čísel $1$ až $n$).
1. Určete střední hodnotu počtu posunutých prvků při vkládání $j$-tého čísla ($j \in \{1, \dots, n\}$).
2. Určete **celkovou střední hodnotu nákladů** (v korunách) za vložení všech $n$ čísel a odvoďte průměrnou cenu jedné operace vložení.

> [!success]- Řešení
> ### 1. Střední hodnota nákladů při vkládání $j$-tého prvku
> Nechť v poli již leží $j - 1$ dříve vložených čísel uspořádaných vzestupně.
> Nyní vkládáme $j$-tý prvek $x_j$.
> Protože vstupní posloupnost je rovnoměrně náhodná permutace čísel $\{1, \dots, n\}$, má vkládaný prvek $x_j$ stejnou pravděpodobnost, že v uspořádané posloupnosti dosavadních $j$ prvků skončí na libovolné z $j$ možných pozic:
> - na 1. pozici (před všemi $j-1$ prvky) $\implies$ musí se posunout všech $j-1$ prvků,
> - na 2. pozici $\implies$ posune se $j-2$ prvků,
> - $\dots$
> - na $j$-té pozici (za všemi prvky, je dosavadním maximem) $\implies$ posune se $0$ prvků.
>
> Každá z těchto pozic nastává s rovnoměrnou pravděpodobností $P = \frac{1}{j}$.
> Náhodná veličina $X_j$ udávající počet posunutých prvků při $j$-tém vložení nabývá hodnot z $\{0, 1, \dots, j-1\}$.
> Střední hodnota $E[X_j]$ je:
> $$E[X_j] = \sum_{i=0}^{j-1} i \cdot P(X_j = i) = \frac{1}{j} \sum_{i=0}^{j-1} i = \frac{1}{j} \cdot \frac{(j-1) j}{2} = \mathbf{\frac{j - 1}{2}}.$$
>
> ---
>
> ### 2. Celková střední hodnota ceny za vložení všech $n$ čísel
> Celková cena je náhodná veličina $X = \sum_{j=1}^n X_j$.
> Díky **linearitě střední hodnoty** ([Věta 7.2](https://truepeak78.github.io/ag1/07_Randomizace_a_he%C5%A1ov%C3%A1n%C3%AD/7.5_Pravd%C4%9Bpodobnostn%C3%AD_prostor/#theorem-7.2)), která platí bez ohledu na závislost veličin:
> $$E[X] = E\left[ \sum_{j=1}^n X_j \right] = \sum_{j=1}^n E[X_j] = \sum_{j=1}^n \frac{j - 1}{2} = \frac{1}{2} \sum_{k=0}^{n-1} k.$$
> 
> Součet aritmetické řady dává:
> $$E[X] = \frac{1}{2} \cdot \frac{(n - 1) n}{2} = \mathbf{\frac{n(n - 1)}{4} \text{ korun}}.$$
> 
> **Průměrná (střední) cena jedné operace vložení:**
> $$\frac{E[X]}{n} = \frac{n(n - 1)}{4n} = \mathbf{\frac{n - 1}{4}} = \mathbf{\Theta(n)}.$$
> Přestože vyhledání pozice trvá $O(1)$, samotné udržování seřazeného pole vyžaduje ve střední hodnotě kvadratický celkový čas $\Theta(n^2)$ a lineární čas na jedno vložení.


---

## Úloha 9.6: Analýza vnořených cyklů s exponenciálním růstem
* **Zdroj:** FitWiki (Dvořák 2022 `dvorak2022`)
* **Relevantní skripta:** [Sekce 1.2 (Asymptotická složitost a sumace)](https://truepeak78.github.io/ag1/01_Asymptotika/1.2_Asymptotick%C3%A1_slo%C5%BEitost/)

### Zadání
Je dán následující algoritmus:
```text
Hvezdicky(n):
    j = 1
    for i = 1 to n:
        j = 2 * j
        k = j
        while k > 0:
            print "*"
            k = k - 1
```
1. Odvoďte přesný vzorec pro celkový počet vytištěných hvězdiček v závislosti na parametru $n$ ($n \ge 1$).
2. Určete přesnou asymptotickou časovou složitost funkce `Hvezdicky(n)` v notaci $\Theta$.

> [!success]- Řešení
> ### 1. Odvození přesného počtu hvězdiček
> Sledujme hodnotu proměnné $j$ v jednotlivých iteracích vnějšího cyklu `for i = 1 to n`:
> - Na začátku před cyklem: $j_0 = 1$.
> - Pro $i = 1$: $j_1 = 2 \cdot 1 = 2^1 = 2$.
>   Vnitřní cyklus `while` se provede $k = 2$-krát $\implies$ vytiskne se $2^1 = 2$ hvězdiček.
> - Pro $i = 2$: $j_2 = 2 \cdot 2^1 = 2^2 = 4$.
>   Vnitřní cyklus se provede $4$-krát $\implies$ vytiskne se $2^2 = 4$ hvězdičky.
> - Obecně pro $i$-tou iteraci: $j_i = 2^i$, vnitřní cyklus proběhne $2^i$-krát a vytiskne $2^i$ hvězdiček.
>
> Celkový počet vytištěných hvězdiček $S(n)$ je součet přes všechny iterace $i = 1, \dots, n$:
> $$S(n) = \sum_{i=1}^n 2^i = 2^1 + 2^2 + 2^3 + \dots + 2^n$$
> 
> Jedná se o konečnou geometrickou řadu s prvním členem $a_1 = 2$ a kvocientem $q = 2$:
> $$S(n) = a_1 \cdot \frac{q^n - 1}{q - 1} = 2 \cdot \frac{2^n - 1}{2 - 1} = \mathbf{2^{n+1} - 2}$$
>
> ### 2. Asymptotická časová složitost
> - Každý tisk `print "*"` a dekrementace $k$ trvá $O(1)$.
> - Režie vnějšího cyklu (násobení $j = 2j$, inicializace $k=j$, inkrementace $i$) se provede $n$-krát $\implies O(n)$.
> - Celkový počet operací je:
>   $$T(n) = c_1 \cdot (2^{n+1} - 2) + c_2 \cdot n = \mathbf{\Theta(2^n)}$$
> Časová složitost je tedy striktně **exponenciální** $\Theta(2^n)$.

---

## Úloha 9.7: Potenciálová metoda pro obousměrně dynamické pole (Push & Pop)
* **Zdroj:** FitWiki (Zkouškový standard, ZS 2021/2022 Pokorný, Catay 2024)
* **Relevantní skripta:** [Sekce 4.6 (Potenciálová metoda amortizované analýzy)](https://truepeak78.github.io/ag1/04_Bin%C3%A1rn%C3%AD_haldy/4.6_Amortizovan%C3%A1_%C4%8Dasov%C3%A1_slo%C5%BEitost/#potencialova-metoda)

### Zadání
Uvažujme dynamické pole s proměnlivou kapacitou $s$ a aktuálním počtem prvků $k$.
Pole podporuje operace `Push(x)` (přidání na konec) a `Pop()` (odebrání z konce):
- Začínáme s prázdným polem o kapacitě $s = 1$ a počtem prvků $k = 0$.
- **`Push(x)`:**
  - Pokud $k = s$ (pole je plné), pole se **zdvojnásobí** na kapacitu $2s$, všech $k$ původních prvků se překopíruje do nového pole a staré pole se uvolní.
  - Následně se prvek $x$ zapíše na index $k+1$ a $k \leftarrow k + 1$.
  - Skutečná cena $c_i$: $1$ při zápisu bez zvětšení, $k + 1$ při nutnosti zdvojení a překopírování.
- **`Pop()`:**
  - Odebere prvek z konce: $k \leftarrow k - 1$.
  - Pokud po odebrání klesne zaplnění na **čtvrtinu kapacity** ($k \le s / 4$) a $s > 1$, pole se **zmenší na polovinu** ($s \leftarrow s / 2$), všech $k$ prvků se překopíruje do menšího pole.
  - Skutečná cena $c_i$: $1$ bez zmenšení, $k + 1$ při zmenšení a překopírování.

Pomocí **potenciálové metody** definujte potenciálovou funkci $\Phi$ a dokažte, že amortizovaná složitost operací `Push` i `Pop` je $O(1)$.

> [!success]- Řešení
> ### 1. Definice potenciálové funkce $\Phi$
> Cílem potenciálové funkce $\Phi$ je akumulovat "energii" v okamžicích, kdy se pole vzdaluje od ideálního naplnění (které je $s/2$), aby tato energie plně zaplatila nákladné překopírování při dosažení mezí ($k = s$ nebo $k = s/4$).
> 
> Definujeme potenciál $\Phi$ pro stav pole s kapacitou $s$ a počtem prvků $k$:
> $$\Phi(k, s) = \begin{cases} 
> 2k - s & \text{pokud } k \ge \frac{s}{2}, \\
> \frac{s}{2} - k & \text{pokud } k < \frac{s}{2}.
> \end{cases}$$
> 
> **Ověření vlastností potenciálu:**
> - V počátečním stavu: $s = 1, k = 0 \implies k < s/2 \implies \Phi_0 = \frac{1}{2} - 0 = \frac{1}{2} \ge 0$.
> - V průběhu práce:
>   - Když $k \ge s/2$, platí $2k \ge s \implies \Phi \ge 0$.
>   - Když $k < s/2$, platí $s/2 > k \implies \Phi \ge 0$.
> - Potenciál je tedy **vždy nezáporný** ($\Phi_i \ge 0$ pro všechna $i$), což zaručuje, že součet amortizovaných cen je horním odhadem skutečných nákladů:
>   $$\sum_{i=1}^m c_i \le \sum_{i=1}^m \hat{c}_i + \Phi_0 = \sum_{i=1}^m \hat{c}_i + O(1).$$
>
> ---
>
> ### 2. Amortizovaná cena operace `Push`
> Nechť před operací je stav $(k, s)$ a po operaci $(k+1, s')$.
> 
> #### Případ A: $k < s$ (bez realokace, $s' = s$)
> Skutečná cena je $c_i = 1$.
> 1. Pokud před operací $k \ge s/2$:
>    - $\Phi_{i-1} = 2k - s$
>    - $\Phi_i = 2(k+1) - s = 2k - s + 2$
>    - $\Delta \Phi = \Phi_i - \Phi_{i-1} = 2$
>    - Amortizovaná cena: $\hat{c}_i = c_i + \Delta \Phi = 1 + 2 = \mathbf{3}$.
> 2. Pokud před operací $k < s/2$:
>    - $\Phi_{i-1} = s/2 - k$
>    - Po vložení je bud $k+1 \le s/2$, pak $\Phi_i = s/2 - (k+1) \implies \Delta \Phi = -1 \implies \hat{c}_i = 1 - 1 = \mathbf{0}$,
>    - nebo $k+1 > s/2$, pak $\Phi_i = 2(k+1) - s \implies \Delta \Phi \le 1 \implies \hat{c}_i \le \mathbf{2}$.
>
> #### Případ B: $k = s$ (s realokací na $s' = 2s$)
> Skutečná cena překopírování a vložení je $c_i = k + 1 = s + 1$.
> - Před operací: $k = s \ge s/2 \implies \Phi_{i-1} = 2s - s = s$.
> - Po operaci: počet prvků je $k' = s + 1$, nová kapacita je $s' = 2s$.
>   - Zkontrolujeme poměr: $k' = s + 1 > s'/2 = s$.
>   - Nový potenciál: $\Phi_i = 2(s + 1) - 2s = 2s + 2 - 2s = 2$.
> - Změna potenciálu:
>   $$\Delta \Phi = \Phi_i - \Phi_{i-1} = 2 - s$$
> - Amortizovaná cena operace `Push`:
>   $$\hat{c}_i = c_i + \Delta \Phi = (s + 1) + (2 - s) = \mathbf{3}.$$
> 
> Ve všech případech je amortizovaná cena `Push` shora omezena $\hat{c}_{\text{Push}} \le 3 = \mathbf{O(1)}$.
>
> ---
>
> ### 3. Amortizovaná cena operace `Pop`
> Nechť před operací je stav $(k, s)$ a po operaci $(k-1, s')$.
> 
> #### Případ A: $k > s/4 + 1$ (bez realokace, $s' = s$)
> Skutečná cena je $c_i = 1$.
> 1. Pokud po odebrání $k - 1 \ge s/2$:
>    - $\Delta \Phi = (2(k-1) - s) - (2k - s) = -2$
>    - $\hat{c}_i = 1 - 2 = -1 \le \mathbf{1}$.
> 2. Pokud před odebráním $k \le s/2$:
>    - $\Phi_{i-1} = s/2 - k$
>    - $\Phi_i = s/2 - (k-1) \implies \Delta \Phi = +1$
>    - $\hat{c}_i = 1 + 1 = \mathbf{2}$.
>
> #### Případ B: Po odebrání je $k - 1 = s/4$ (nastane zmenšení na $s' = s/2$)
> Skutečná cena je překopírování $s/4$ prvků a odebrání: $c_i = \frac{s}{4} + 1$.
> - Před operací: v poli bylo $k = s/4 + 1 < s/2$ prvků.
>   $$\Phi_{i-1} = \frac{s}{2} - \left(\frac{s}{4} + 1\right) = \frac{s}{4} - 1.$$
> - Po operaci: v poli je $k' = s/4$ prvků a kapacita klesla na $s' = s/2$.
>   - Poměr naplnění: $k' = \frac{s'}{2} = \frac{s}{4}$. Pole je přesně z poloviny plné!
>   - Nový potenciál: $\Phi_i = 2 k' - s' = 2 \left(\frac{s}{4}\right) - \frac{s}{2} = 0$.
> - Změna potenciálu:
>   $$\Delta \Phi = \Phi_i - \Phi_{i-1} = 0 - \left(\frac{s}{4} - 1\right) = -\frac{s}{4} + 1.$$
> - Amortizovaná cena operace `Pop`:
>   $$\hat{c}_i = c_i + \Delta \Phi = \left(\frac{s}{4} + 1\right) + \left(-\frac{s}{4} + 1\right) = \mathbf{2}.$$
>
> ### Závěr
> Amortizovaná cena každé operace `Push` je nejvýše $3$ a každé operace `Pop` nejvýše $2$. Obě operace tedy běží v **amortizovaném konstantním čase $O(1)$**, přičemž paměťové plýtvání je omezeno faktorem naplnění $\alpha \in [1/4, 1]$.
