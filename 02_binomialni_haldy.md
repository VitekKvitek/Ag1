# 02. Binomiální haldy – Zadání z písemek a řešení

Tento dokument obsahuje úlohy z písemek zaměřené na **binomiální stromy** a **binomiální haldy**.

---

## Úloha 2.1: Parametry binomiálního stromu $B_{36}$

* **Zdroj:** FitWiki (Scholtzová 2020)
* **Relevantní skripta:** [Definice 5.1 (Binomiální strom $B_k$)](https://truepeak78.github.io/ag1/05_Binomi%C3%A1ln%C3%AD_haldy/5.1_Binomi%C3%A1ln%C3%AD_stromy/#def-5.1) · [Věta 5.1 (Vlastnosti binomiálního stromu $B_k$)](https://truepeak78.github.io/ag1/05_Binomi%C3%A1ln%C3%AD_haldy/5.1_Binomi%C3%A1ln%C3%AD_stromy/#theorem-5.1)

### Zadání
Uvažujte binomiální strom $B_k$ řádu $k = 36$.
1. Jaký je stupeň jeho kořene?
2. Jaký je celkový počet vrcholů tohoto stromu?
3. Jaká je hloubka stromu a kolik má hladin?
4. Kolik prvků se nachází ve 3. a v 10. hloubce stromu?

> [!success]- Řešení
> Podle definice a [Věty 5.1](https://truepeak78.github.io/ag1/05_Binomi%C3%A1ln%C3%AD_haldy/5.1_Binomi%C3%A1ln%C3%AD_stromy/#theorem-5.1) platí pro binomiální strom $B_k$:
>
> 1. **Stupeň kořene:** $\deg(\text{root}) = k = \mathbf{36}$.
> 2. **Celkový počet vrcholů:** $|V(B_{36})| = 2^k = \mathbf{2^{36}}$.
> 3. **Hloubka a hladiny:**
>    - Hloubka (maximální vzdálenost od kořene k listu) je $k = \mathbf{36}$.
>    - Hladiny se číslují od $0$ (kořen) do $36$, strom má tedy celkem $k + 1 = \mathbf{37}$ hladin.
> 4. **Počet prvků v dané hloubce $d$:**
>    - Počet vrcholů v hloubce $d$ (kde $0 \le d \le k$) je dán kombinačním číslem $\binom{k}{d}$.
>    - V hloubce $d = 3$: $\mathbf{\binom{36}{3}} = \frac{36 \cdot 35 \cdot 34}{3 \cdot 2 \cdot 1} = 6 \cdot 35 \cdot 34 = \mathbf{7\,140}$.
>    - V hloubce $d = 10$: $\mathbf{\binom{36}{10}}$.

---

## Úloha 2.2: Stromy v binomiální haldě vzniklé sloučením

* **Zdroj:** FitWiki (Plachý 2017 & 2018)
* **Relevantní skripta:** [Definice 5.2 (Binomiální halda)](https://truepeak78.github.io/ag1/05_Binomi%C3%A1ln%C3%AD_haldy/5.2_Binomi%C3%A1ln%C3%AD_halda/#def-5.2) · [Věta 5.1 (Vlastnosti binomiálního stromu)](https://truepeak78.github.io/ag1/05_Binomi%C3%A1ln%C3%AD_haldy/5.1_Binomi%C3%A1ln%C3%AD_stromy/#theorem-5.1)

### Zadání
Binomiální halda $H$ vznikne spojením dvou binomiálních hald o $501$ a $660$ prvcích.
1. Kolik prvků má výsledná halda?
2. Z jakých binomiálních stromů se výsledná halda skládá (určete řády stromů)?
3. Pro každý z těchto stromů uveďte jeho řád, počet vrcholů a hloubku.

> [!success]- Řešení
> 1. **Celkový počet prvků:**
>    $$n = 501 + 660 = \mathbf{1\,161}$$
> 2. **Rozklad na binomiální stromy:**
>    V binomiální haldě odpovídá existence stromu $B_i$ právě $i$-tému bitu v binárním zápisu čísla $n$ ([Definice 5.2](https://truepeak78.github.io/ag1/05_Binomi%C3%A1ln%C3%AD_haldy/5.2_Binomi%C3%A1ln%C3%AD_halda/#def-5.2)).
>    Převedeme $1161$ do dvojkové soustavy:
>    $$1161 = 1024 + 128 + 8 + 1 = 2^{10} + 2^7 + 2^3 + 2^0 = (10010001001)_2$$
>    Halda se tedy skládá právě ze stromů:
>    $$\mathbf{B_{10}, B_7, B_3, B_0}$$
>
> 3. **Parametry jednotlivých stromů:**
>    - **Strom $B_{10}$:** řád $k = 10$, počet vrcholů $2^{10} = \mathbf{1\,024}$, hloubka = $\mathbf{10}$.
>    - **Strom $B_7$:** řád $k = 7$, počet vrcholů $2^7 = \mathbf{128}$, hloubka = $\mathbf{7}$.
>    - **Strom $B_3$:** řád $k = 3$, počet vrcholů $2^3 = \mathbf{8}$, hloubka = $\mathbf{3}$.
>    - **Strom $B_0$:** řád $k = 0$, počet vrcholů $2^0 = \mathbf{1}$, hloubka = $\mathbf{0}$.
>    *(Kontrola součtu vrcholů: $1024 + 128 + 8 + 1 = 1161$).*

---

## Úloha 2.3: Spojení dvou hald operací BHMerge a následný ExtractMin

* **Zdroj:** FitWiki (Knop 2020)
* **Relevantní skripta:** [Algoritmus 5.1 (BHMerge)](https://truepeak78.github.io/ag1/05_Binomi%C3%A1ln%C3%AD_haldy/5.3_Operace_na_BH/#algo-5.1) · [Algoritmus 5.3 (BHExtractMin)](https://truepeak78.github.io/ag1/05_Binomi%C3%A1ln%C3%AD_haldy/5.3_Operace_na_BH/#algo-5.3)

### Zadání
Uvažujte dvě minimové binomiální haldy $A$ a $B$:
- Halda $A$ obsahuje stromy: $B_0$ (hodnota kořene 3) a $B_1$ (kořen 4, syn 8).
- Halda $B$ obsahuje stromy: $B_0$ (hodnota kořene 2), $B_1$ (kořen 1, syn 9) a $B_2$ (kořen 0 se syny 5, 6 a vnukem 7 pod 5).

1. Proveďte operaci $\text{BHMerge}(A, B)$ a zakreslete výsledek.
2. Nad výsledkem proveďte operaci $\text{BHExtractMin}$.

> [!success]- Řešení
> ### 1. Spojení hald BHMerge(A, B)
> Funguje analogicky ke sčítání binárních čísel zprava doleva podle řádů stromů:
> - Velikost $A$: $1 + 2 = 3 = (011)_2$ (obsahuje $B_0, B_1$).
> - Velikost $B$: $1 + 2 + 4 = 7 = (111)_2$ (obsahuje $B_0, B_1, B_2$).
> - Výsledná velikost: $3 + 7 = 10 = (1010)_2$ (musí obsahovat stromy $B_3$ a $B_1$).
>
> **Kroky:**
> - **Řád 0 ($B_0$):**
>   - Máme $A: B_0(3)$ a $B: B_0(2)$.
>   - Spojíme je zavěšením stromu s větším kořenem pod strom s menším kořenem: kořen $2 < 3$, tedy $3$ se stává synem $2$.
>   - Vzniká přenosový strom řádu 1: $C_1$ s kořenem $2$ a synem $3$.
>   - Na řádu 0 nezůstává žádný strom.
>
> - **Řád 1 ($B_1$):**
>   - Máme tři stromy řádu 1: $A: B_1(\text{kořen } 4)$, $B: B_1(\text{kořen } 1)$, přenos $C_1(\text{kořen } 2)$.
>   - Jeden strom řádu 1 ponecháme ve výsledné haldě. Ponecháme např. $A: B_1(\text{kořen } 4, \text{syn } 8)$ (nebo přenos).
>   - Zbylé dva stromy spojíme do přenosu řádu 2: spojíme strom s kořenem $1$ a strom s kořenem $2$. Jelikož $1 < 2$, kořen $2$ (se svým synem $3$) se stane novým nejlevějším synem kořene $1$.
>   - Vzniká přenosový strom $C_2$ s kořenem $1$ (synové $2$ a $9$).
>
> - **Řád 2 ($B_2$):**
>   - Máme $B: B_2(\text{kořen } 0)$ a přenos $C_2(\text{kořen } 1)$.
>   - Spojíme je do řádu 3: kořen $0 < 1$, tedy strom $C_2$ (kořen 1) se stává novým nejlevějším synem kořene $0$.
>   - Vzniká strom $B_3$ s kořenem $0$ (synové $1, 5, 6$).
>   - Na řádu 2 nezůstává žádný strom.
>
> **Výsledná halda po BHMerge:**
> - $B_1$: kořen $4$, syn $8$.
> - $B_3$: kořen $0$, jeho synové jsou $1, 5, 6$.
>   - Pod $1$ jsou $2, 9$ (a pod $2$ je $3$).
>   - Pod $5$ je $7$.
>   - Pod $6$ není nic.
>
> ---
>
> ### 2. Operace BHExtractMin
> 1. Najdeme minimum mezi kořeny všech stromů haldy:
>    - Kořeny jsou $4$ (v $B_1$) a $0$ (v $B_3$). Minimum je **0**.
> 2. Odstraníme kořen $0$. Jeho podstromy tvoří novou dočasnou haldu $H'$:
>    - Synové kořene $0$ jsou stromy řádů $2, 1, 0$:
>      - $B_2$ s kořenem $1$
>      - $B_1$ s kořenem $5$
>      - $B_0$ s kořenem $6$
> 3. Původní halda bez $B_3$ obsahuje pouze $B_1$ (s kořenem $4$).
> 4. Provedeme $\text{BHMerge}$ zbylé haldy $\{B_1(4)\}$ a rozpadlého stromu $\{B_0(6), B_1(5), B_2(1)\}$:
>    - $B_0$: strom $B_0(6)$ zůstává.
>    - $B_1$: spojíme $B_1(4)$ a $B_1(5)$. Kořen $4 < 5$, tedy $5$ se stane synem $4$. Vzniká přenosový strom $B_2$ s kořenem $4$.
>    - $B_2$: spojíme stávající $B_2(1)$ a přenos $B_2(4)$. Kořen $1 < 4$, tedy strom s kořenem $4$ se zavěsí pod $1$. Vzniká strom $B_3$ s kořenem $1$.
>
> **Výsledná halda po BHExtractMin:**
> - Strom $B_0$: kořen $6$.
> - Strom $B_3$: kořen $1$.
>   - Synové kořene $1$: $4, 2, 9$.

---

## Úloha 2.4: Operace IncreaseKey v binomiální minimové haldě

* **Zdroj:** FitWiki (Sedláček 2020)
* **Relevantní skripta:** [Věta 5.6 a kapitola 5.3 (Operace na BH)](https://truepeak78.github.io/ag1/05_Binomi%C3%A1ln%C3%AD_haldy/5.3_Operace_na_BH/#theorem-5.6) · [Definice 5.2 (Binomiální halda)](https://truepeak78.github.io/ag1/05_Binomi%C3%A1ln%C3%AD_haldy/5.2_Binomi%C3%A1ln%C3%AD_halda/#def-5.2)

### Zadání
Popište, jak v binomiální minimové haldě funguje operace `IncreaseKey(ukazatelNaVrchol, a)`, která zvýší hodnotu klíče na zadaném vrcholu o kladnou hodnotu $a > 0$ (a zajistí, že po operaci bude halda validní).
Požadovaná časová složitost je $O(\log n)$, kde $n$ je počet prvků v haldě. Odůvodněte časovou složitost i korektnost.

> [!success]- Řešení
> ### Standardní (přednáškový) algoritmus v čase $O(\log n)$
> V binomiální haldě nemá vrchol konstantní počet synů – vrchol v $B_k$ může mít až $k = O(\log n)$ synů. Naivní operace `BubbleDown` (hledání minimálního syna na každé z $O(\log n)$ hladin) by proto v nejhorším případě trvala $O(\log^2 n)$.
> 
> Abychom garantovali přesnou časovou složitost **$O(\log n)$**, převádí se `IncreaseKey` na posloupnost tří standardních operací:
> 
> 1. **`DecreaseKey(v, -\infty)`:**
>    - Snížíme klíč ve vrcholu $v$ na $-\infty$.
>    - Zavoláme `BubbleUp(v)`: vrchol stoupá nahoru k rodiči. V každém kroku se porovnává pouze **jediný** prvek (rodič), což trvá $O(1)$ na hladinu.
>    - Maximální hloubka binomiálního stromu je $\le \log_2 n$, probublání do kořene stromu tedy trvá **$O(\log n)$**.
> 2. **`BHExtractMin()`:**
>    - Vrchol $v$ (s hodnotou $-\infty$) je nyní globálním minimem haldy a je kořenem svého stromu.
>    - Odstraníme ho v čase **$O(\log n)$**: jeho podstromy tvoří samostatnou binomiální haldu, kterou sloučíme se zbytkem původní haldy pomocí `BHMerge`.
> 3. **`BHInsert(v, key_starý + a)`:**
>    - Vložíme vrchol $v$ zpět do haldy s novou hodnotou klíče $\text{key}_{\text{starý}} + a$.
>    - Operace `BHInsert` trvá nejvýše **$O(\log n)$** (odpovídá přičtení jedničky v binárním sčítání).
> 
> ### Korektnost
> - Po `DecreaseKey` a `BubbleUp` zůstane zachována platná haldová relace uvnitř stromu, vrchol $v$ se dostane do kořene.
> - `BHExtractMin` korektně odebere vrchol $v$ a sloučí haldu podle pravidel binomiálních hald.
> - `BHInsert` korektně zařadí $v$ s novou hodnotou klíče. Výsledná struktura je garantovaně validní minimovou binomiální haldou.
> 
> ### Časová složitost
> - `DecreaseKey`: $O(\log n)$
> - `BHExtractMin`: $O(\log n)$
> - `BHInsert`: $O(\log n)$
> - Celková časová složitost je součtem: $O(\log n) + O(\log n) + O(\log n) = \mathbf{O(\log n)}$.


---

## Úloha 2.5: Strukturální vlastnosti binomiálního stromu vysokého řádu ($B_{36}$)
* **Zdroj:** FitWiki (Scholtzová 2020 `scholtzova2020`)
* **Relevantní skripta:** [Definice 5.1 a Věta 5.1 (Binomiální strom a jeho vlastnosti)](https://truepeak78.github.io/ag1/05_Binomi%C3%A1ln%C3%AD_haldy/5.1_Binomi%C3%A1ln%C3%AD_strom/#def-5.1)

### Zadání
Uvažujte binomiální strom řádu $k = 36$ (značíme $B_{36}$).
1. Jaký je stupeň kořene stromu $B_{36}$?
2. Kolik vrcholů celkem strom $B_{36}$ obsahuje? Jaká je jeho hloubka (výška) a kolik má hladin?
3. Určete počet vrcholů ve 3. hloubce a v 10. hloubce stromu $B_{36}$ (kořen je v hloubce 0). Kombinační čísla vyčíslete nebo zapište v základním tvaru.

> [!success]- Řešení
> Podle [Věty 5.1 ze skript](https://truepeak78.github.io/ag1/05_Binomi%C3%A1ln%C3%AD_haldy/5.1_Binomi%C3%A1ln%C3%AD_strom/#theorem-5.1) o vlastnostech binomiálního stromu $B_k$:
>
> 1. **Stupeň kořene:**
>    - Kořen binomiálního stromu $B_k$ má právě $k$ bezprostředních synů, které tvoří kořeny podstromů $B_0, B_1, \dots, B_{k-1}$.
>    - Pro $k = 36$ je stupeň kořene:
>      $$\deg(\text{root}) = \mathbf{36}.$$
>
> 2. **Počet vrcholů, hloubka a počet hladin:**
>    - Celkový počet vrcholů v $B_k$ je přesně $|V(B_k)| = 2^k$.
>      $$|V(B_{36})| = \mathbf{2^{36}} = 68\,719\,476\,736 \text{ vrcholů}.$$
>    - **Hloubka stromu:** Maximální vzdálenost listu od kořene je rovna řádu stromu:
>      $$\text{Hloubka} = k = \mathbf{36}.$$
>    - **Počet hladin:** Hladiny jsou indexovány od $0$ (kořen) do $36$ (nejhlubší list).
>      $$\text{Počet hladin} = k + 1 = 36 + 1 = \mathbf{37 \text{ hladin}}.$$
>
> 3. **Počet vrcholů na zadaných hladinách:**
>    - Podle Věty 5.1 je počet vrcholů v hloubce $d$ ($0 \le d \le k$) roven binomickému koeficientu $\binom{k}{d}$.
>    - **Ve 3. hloubce ($d = 3$):**
>      $$\binom{36}{3} = \frac{36 \cdot 35 \cdot 34}{3 \cdot 2 \cdot 1} = 12 \cdot 35 \cdot 17 = \mathbf{7\,140 \text{ vrcholů}}.$$
>    - **V 10. hloubce ($d = 10$):**
>      $$\binom{36}{10} = \frac{36!}{10! \cdot 26!} = \mathbf{254\,186\,856 \text{ vrcholů}}.$$

---

## Úloha 2.6: Operace Insert a ExtractMax na maximové binomiální haldě
* **Zdroj:** FitWiki (Baier 2016, scan reálného testu `test-ag1-13-12-2016-baier.jpg`)
* **Relevantní skripta:** [Sekce 5.3 (Operace na binomiální haldě – BHMerge, BHInsert, BHExtractMax)](https://truepeak78.github.io/ag1/05_Binomi%C3%A1ln%C3%AD_haldy/5.3_Operace_na_BH/)

### Zadání
Mějme následující **maximovou binomiální haldu** tvořenou třemi binomiálními stromy řádů $0, 1, 3$:
- **Strom $B_0$:** kořen $(8)$
- **Strom $B_1$:** kořen $(14)$ se synem $(9)$
- **Strom $B_3$:** kořen $(24)$ se třemi syny:
  - syn $(3)$ (řád 0)
  - syn $(12)$ (řád 1) se synem $(10)$
  - syn $(6)$ (řád 2) se synem $(4)$ (řád 0) a synem $(5)$ (řád 1, který má syna $(2)$)

1. Pomocí operace `BHInsert` vložte postupně prvky **$15$** a **$7$**. Zakreslete stav haldy po vložení každého z těchto prvků.
2. Následně nad výslednou haldou proveďte operaci **`BHExtractMax()`** a zakreslete výslednou haldu.

> [!success]- Řešení
> V maximové binomiální haldě platí opačná haldová podmínka: klíč v rodiči je $\ge$ než klíče všech jeho potomků.
> Při slučování dvou stromů $B_k$ se stejným řádem se strom s menším kořenem připojí jako syn pod strom s větším kořenem!
>
> ---
>
> ### 1. Vložení prvku $15$
> Vytvoříme jednoprvkový strom $B_0$ s kořenem $(15)$. Původní halda obsahuje stromy $B_0(8), B_1(14), B_3(24)$.
> 1. **Sloučení na řádu 0:**
>    - Máme dva stromy řádu 0: nový $(15)$ a původní $B_0(8)$.
>    - Porovnáme kořeny: $15 > 8 \implies$ uzel $8$ se stane synem uzlu $15$.
>    - Vzniká přenosový strom $B_1$ s kořenem $(15)$ a synem $(8)$.
> 2. **Sloučení na řádu 1:**
>    - Máme přenosový strom $B_1(15)$ a stávající strom $B_1(14)$.
>    - Porovnáme kořeny: $15 > 14 \implies$ strom $B_1(14)$ se stane podstromem kořene $15$.
>    - Vzniká přenosový strom $B_2$ s kořenem $(15)$:
>      - synové kořene $15$: uzel $(8)$ (řád 0) a uzel $(14)$ (řád 1, se synem $9$).
> 3. **Sloučení na řádu 2:**
>    - V původní haldě žádný strom $B_2$ nebyl. Přenosový strom $B_2(15)$ zůstává v haldě.
> 4. **Řád 3:**
>    - Původní strom $B_3(24)$ zůstává beze změny.
> 
> **Stav haldy po vložení 15:**
> Halda obsahuje stromy $B_2$ a $B_3$:
> - $B_2$: kořen $(15)$ se syny $(8)$ a $(14 \to 9)$.
> - $B_3$: kořen $(24)$ (původní strom).
>
> ---
>
> ### 2. Vložení prvku $7$
> Vytvoříme jednoprvkový strom $B_0$ s kořenem $(7)$.
> V haldě aktuálně není žádný strom $B_0$.
> Strom $B_0(7)$ se jednoduše přidá na začátek seznamu kořenů.
> 
> **Stav haldy po vložení 7:**
> Halda obsahuje stromy $B_0, B_2, B_3$:
> - **$B_0$:** kořen $(7)$
> - **$B_2$:** kořen $(15)$ se syny $(8)$ a $(14 \to 9)$
> - **$B_3$:** kořen $(24)$ se syny $(3)$, $(12 \to 10)$, $(6 \to 4, 5 \to 2)$
>
> ---
>
> ### 3. Operace `BHExtractMax()`
> 1. **Nalezení maxima:** Projdeme kořeny stromů v haldě: $\{7, 15, 24\}$. Maximum je **$24$** v kořeni stromu $B_3$.
> 2. **Odstranění kořene 24:**
>    - Odstraněním kořene $24$ se strom $B_3$ rozpadne na své podstromy:
>      - podstrom řádu 0: $(3)$
>      - podstrom řádu 1: $(12 \to 10)$
>      - podstrom řádu 2: $(6 \to 4, 5 \to 2)$
>    - Tyto stromy tvoří novou dočasnou haldu $H'$ se stromy $B_0'(3), B_1'(12), B_2'(6)$.
> 3. **Sloučení $H$ (zbylé stromy $B_0(7), B_2(15)$) a $H'$ pomocí `BHMerge`:**
>    - **Řád 0:** Máme $B_0(7)$ a $B_0'(3)$.
>      - $7 > 3 \implies$ uzel $3$ se stane synem uzlu $7$.
>      - Vzniká přenosový strom $B_1$ s kořenem $(7)$ a synem $(3)$.
>    - **Řád 1:** V $H$ není nic, v $H'$ je $B_1'(12)$, plus máme přenosový strom $B_1(7)$.
>      - Slučujeme dva stromy řádu 1: $B_1'(12)$ a $B_1(7)$.
>      - $12 > 7 \implies$ uzel $7$ se stane synem uzlu $12$.
>      - Vzniká přenosový strom $B_2$ s kořenem $(12)$ se syny $(10)$ a $(7 \to 3)$.
>    - **Řád 2:** Máme tři stromy řádu 2:
>      - ze staré haldy: $B_2(15)$
>      - z rozpadu $B_3$: $B_2'(6)$
>      - z přenosu: $B_2(12)$
>      - Jeden strom ponecháme v haldě: necháme strom $B_2'(6)$.
>      - Zbylé dva stromy $B_2(15)$ a $B_2(12)$ sloučíme:
>        - $15 > 12 \implies$ uzel $12$ se připojí jako syn pod uzel $15$.
>        - Vzniká přenosový strom $B_3$ s kořenem $(15)$.
>    - **Řád 3:** V žádné z hald nebyl další strom $B_3$. Přenosový strom $B_3(15)$ zůstává jako jediný strom řádu 3.
> 
> **Výsledná binomiální halda po `ExtractMax`:**
> Halda obsahuje stromy $B_2$ a $B_3$:
> - **Strom $B_2$:** kořen $(6)$ se syny $(4)$ a $(5 \to 2)$.
> - **Strom $B_3$:** kořen $(15)$ se třemi syny:
>   - syn $(8)$ (řád 0)
>   - syn $(14 \to 9)$ (řád 1)
>   - syn $(12)$ (řád 2) se syny $(10)$ a $(7 \to 3)$.
