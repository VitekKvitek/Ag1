# 11. Stavové grafy a pokročilé algoritmy – Zadání z písemek a řešení

Tento dokument obsahuje úlohy z 2. zápočtového testu BI-AG1 zaměřené na **návrh pokročilých algoritmů** (typicky 4 až 5 bodové úlohy s rozšiřující tématikou), konstrukci **stavových grafů** pro modifikované BFS, dynamické programování v DAGu a průchody stromy v lineárním čase.

---

## Úloha 11.1: Nejkratší cesta v bludišti se sběrem 3 klíčů

* **Zdroj:** FitWiki (Catay 3.12.2024)
* **Relevantní skripta:** [Algoritmus 1.1 a Věta 1.2 (BFS a nejkratší cesty)](https://truepeak78.github.io/ag1/01_Z%C3%A1klady_graf%C5%AF/1.2_BFS/#algo-1.1) · [Kapitola 1.2 (Upravování grafu pro aplikace BFS)](https://truepeak78.github.io/ag1/01_Z%C3%A1klady_graf%C5%AF/1.2_BFS/)

### Zadání
Mějme neorientovaný graf $G = (V, E)$, který modeluje bludiště.
V grafu jsou dány dva význačné vrcholy: start $s \in V$ a cíl $t \in V$.
V bludišti se nachází 3 různé klíče $K_1, K_2, K_3$. Každý klíč $K_j$ je umístěn v právě jednom vrcholu $v_{K_j} \in V$.
Některé hrany v $G$ jsou zamčené a lze přes ně projít pouze tehdy, má-li hráč příslušný klíč (pro každou zamčenou hranu $e \in E$ je specifikováno, který klíč $K_j$ ji odemyká). Nezadané hrany jsou volně průchozí.
Jakmile hráč klíč získá, může jej použít neomezeněkrát. Hráč se může vracet do již navštívených vrcholů.
Navrhněte algoritmus, který nalezne **nejkratší cestu** (s minimálním počtem průchodů hranami) ze startu $s$ do cíle $t$.
Požadovaná časová složitost je $O(|V| + |E|)$. Dokažte korektnost, konečnost a odvoďte časovou a paměťovou složitost.

> [!success]- Řešení
> ### 1. Myšlenka: Transformace na stavový graf
> Informace o pouhé poloze ve vrcholu $v$ nestačí k jednoznačnému určení stavu, protože dosažení vrcholu $v$ bez klíčů a s klíči dává zcela odlišné možnosti dalšího pohybu.
> Stav hráče je plně určen dvojicí $(v, M)$, kde:
> - $v \in V$ je aktuální pozice v bludišti,
> - $M \subseteq \{1, 2, 3\}$ je množina doposud sebraných klíčů. Tuto množinu lze reprezentovat jako 3-bitový vektor (tzv. bitmask) $M \in \{0, 1, \dots, 7\}$ ($2^3 = 8$ možných stavů klíčů).
>
> Sestrojíme nový orientovaný neohodnocený graf $G' = (V', E')$:
> - **Množina stavových vrcholů $V'$:**
>   $$V' = V \times \{0, 1\}^3 = \{(v, M) \mid v \in V, M \subseteq \{1, 2, 3\}\}.$$
>   Velikost množiny vrcholů je $|V'| = 8 |V|$.
> - **Množina hran $E'$:**
>   Pro každou původní hranu $\{u, v\} \in E$ a každý stav klíčů $M$:
>   1. **Podmínka průchodnosti:**
>      - Je-li hrana volná, lze přes ni projít pro libovolné $M$.
>      - Je-li hrana zamčena klíčem $K_j$, lze přes ni projít pouze tehdy, platí-li $j \in M$.
>   2. **Změna stavu při vstupu do vrcholu:**
>      - Pokud lze projít z $u$ do $v$, určí se nový stav klíčů $M' = M \cup \{j \mid v = v_{K_j}\}$ (pokud je ve vrcholu $v$ klíč, automaticky se sebere).
>      - Do $E'$ přidáme orientovanou hranu $((u, M), (v, M'))$ o jednotkové délce 1.
>      - Analogicky v opačném směru: z $v$ do $u$ s aktualizací o případný klíč ve vrcholu $u$.
>   Počet hran je $|E'| \le 2 \times 8 |E| = 16 |E|$.
>
> ---
>
> ### 2. Algoritmus
> 1. Určíme počáteční stav klíčů ve startu: $M_0 = \{j \mid s = v_{K_j}\}$.
>    Startovním vrcholem v $G'$ je $s' = (s, M_0)$.
> 2. Množina cílových vrcholů je $T' = \{(t, M) \mid M \subseteq \{1, 2, 3\}\}$.
> 3. Spustíme standardní **algoritmus BFS** na grafu $G'$ ze startovního vrcholu $s'$.
> 4. BFS se zastaví v okamžiku, kdy poprvé vyjme z fronty libovolný vrchol $(t, M) \in T'$:
>    - Pomocí pole předchůdců `parent` zrekonstruujeme nalezenou cestu v $G'$ a její projekcí do první složky získáme posloupnost vrcholů v původním grafu $G$.
> 5. Pokud se fronta vyprázdní a žádný cíl není navštíven, cesta neexistuje.
>
> ---
>
> ### 3. Důkaz konečnosti a korektnosti
> - **Konečnost:** Stavový graf $G'$ je konečný ($|V'| = 8|V| < \infty$, $|E'| \le 16|E| < \infty$). Standardní BFS navštíví každý stavový vrchol nejvýše jednou a skončí po konečném počtu kroků.
> - **Korektnost:**
>   - Každá platná trajektorie v bludišti splňující pravidla odemykání odpovídá jednoduché cestě ve stavovém grafu $G'$ o stejné délce hran, protože hrany v $G'$ existují právě tehdy, když je přechod povolen danou množinou klíčů $M$.
>   - Obráceně, libovolná cesta v $G'$ z $(s, M_0)$ do nějakého $(t, M)$ představuje korektní posloupnost kroků v $G$, kde žádná zamčená hrana není překonána bez předchozího navštívení vrcholu s daným klíčem.
>   - Podle Věty o korektnosti BFS ([Věta 1.2](https://truepeak78.github.io/ag1/01_Z%C3%A1klady_graf%C5%AF/1.2_BFS/#algo-1.1)) nalezne BFS v neohodnoceném grafu cestu s minimálním počtem hran.
>
> ---
>
> ### 4. Časová a paměťová složitost
> - Stavový graf nemusíme vytvářet celý dopředu do paměti, sousedy generujeme on-the-fly během BFS.
> - Časová složitost BFS je:
>   $$O(|V'| + |E'|) = O(8|V| + 16|E|) = \mathbf{O(|V| + |E|)}.$$
> - Paměťová složitost pro frontu a pole navštívených stavů `visited[v][M]` velikosti $8|V|$ je $\mathbf{O(|V|)}$.

---

## Úloha 11.2: Nejkratší cesta s vahami $\{0, 300, 500\}$ a cenovým limitem $\le 1000$

* **Zdroj:** FitWiki (Dvořák 2.12.2024)
* **Relevantní skripta:** [Algoritmus 1.1 (BFS)](https://truepeak78.github.io/ag1/01_Z%C3%A1klady_graf%C5%AF/1.2_BFS/#algo-1.1) · [Kapitola 1.2 (Modifikace BFS)](https://truepeak78.github.io/ag1/01_Z%C3%A1klady_graf%C5%AF/1.2_BFS/)

### Zadání
Mějme galaxii modelovanou grafem $G = (V, E)$. Každá hyperprostorová cesta (hrana) má cenu cestování rovnou jedné z hodnot $\{0, 300, 500\}$ kreditů.
Máme zadaný startovní systém $s \in V$ a cílový systém $t \in V$.
Navrhněte algoritmus, který určí, zda existuje cesta z $s$ do $t$ s **celkovou cenou nejvýše $1000$ kreditů**, a pokud ano, nalezne cestu s minimální celkovou cenou.
Časová složitost musí být lineární vzhledem k velikosti grafu $O(|V| + |E|)$. Dokažte korektnost a odvoďte složitost.

> [!success]- Řešení
> ### 1. Myšlenka a stavový prostor
> Hledáme nejlevnější cestu s omezeným diskrétním rozpočtem $\le 1000$.
> Všechny váhy hran jsou násobky $100$: $\{0, 300, 500\} = \{0 \cdot 100, 3 \cdot 100, 5 \cdot 100\}$.
> Celý problém můžeme přeškálovat vydělením 100:
> - Váhy hran jsou $\{0, 3, 5\}$.
> - Maximální povolená cena je $C_{\max} = 1000 / 100 = \mathbf{10}$.
> 
> Protože celková cena je celočíselná a shora omezená malou konstantou $C_{\max} = 10$, můžeme sestrojit stavový graf, kde stav je určen dvojicí $(v, c)$:
> - $v \in V$ je aktuální uzel,
> - $c \in \{0, 1, \dots, 10\}$ je přesná doposud utracená cena.
>
> ### 2. Konstrukce stavového grafu $G'$
> - **Množina vrcholů $V'$:**
>   $$V' = V \times \{0, 1, 2, \dots, 10\}, \quad |V'| = 11 |V|.$$
> - **Množina hran $E'$:**
>   Pro každou hranu $\{u, v\} \in E$ s vahou $w \in \{0, 3, 5\}$ a pro každou cenu $c \in \{0, \dots, 10\}$ takovou, že $c + w \le 10$:
>   - Přidáme orientovanou hranu $((u, c), (v, c + w))$ a opačně $((v, c), (u, c + w))$.
>   - Všechny tyto hrany představují jeden krok přes původní hranu.
>   - Počet hran je $|E'| \le 11 |E|$.
>
> ### 3. Vyhledání nejlevnější cesty
> Protože hrany mají váhy $w \in \{0, 3, 5\}$, nejjednodušší přístup pro nalezení minimální ceny v čase $O(|V| + |E|)$ je:
> - **Varianta A (Rozdělení hran na jednotkové podhrany):**
>   Každou hranu s váhou $w \in \{3, 5\}$ nahradíme cestou délky $w$ zavedením $w-1$ fiktivních vrcholů. Hrany s vahou 0 zkontrahujeme (nebo použijeme 0-1 BFS s oboustrannou frontou `std::deque`).
>   Protože maximální váha je konstanta $5$, vzroste počet vrcholů a hran nejvýše $5$-krát.
> - **Varianta B (Dijkstra s prioritní frontou v konstantním rozsahu / Dialův algoritmus):**
>   Jelikož maximální vzdálenost je $C_{\max} \le 10$, prioritní frontu nahradíme polem 11 obyčejných front `bucket[0 ... 10]`.
>   Vkládání i výběr minima z pole bucketů trvá $O(1)$.
>
> **Postup Dialova algoritmu:**
> 1. Inicializujeme pole vzdáleností $d[v] \leftarrow \infty$ pro všechny $v \in V$, $d[s] \leftarrow 0$.
> 2. Vložíme $s$ do `bucket[0]`.
> 3. Procházíme indexy $\text{curr\_dist}$ od $0$ do $10$:
>    - Dokud `bucket[curr_dist]` není prázdný:
>      - Vyjmeme vrchol $u$. Pokud $d[u] < \text{curr\_dist}$, přeskočíme ho.
>      - Pro každou hranu $\{u, v\} \in E$ s vahou $w \in \{0, 3, 5\}$:
>        - Pokud $d[u] + w < d[v]$ a $d[u] + w \le 10$:
>          - $d[v] \leftarrow d[u] + w$
>          - Vložíme $v$ do `bucket[d[v]]`.
> 4. Pokud po skončení platí $d[t] \le 10$, vrátíme hodnotu $d[t] \times 100$ kreditů. Jinak cesta s cenou $\le 1000$ neexistuje.
>
> ### 4. Složitost a korektnost
> - **Korektnost:** Algoritmus odpovídá Dijkstrovu algoritmu s nezápornými celočíselnými vahami. Jelikož v každém kroku zpracováváme vrcholy s minimální vzdáleností $\text{curr\_dist}$, jsou vzdálenosti optimální.
> - **Časová složitost:** Každý vrchol je vyjmut z bucketu nejvýše jednou (po nalezení optimální vzdálenosti). Každá hrana je relaxována nejvýše konstantněkrát. Procházení 11 bucketů trvá $O(11) = O(1)$.
>   Celková časová složitost je striktně **$O(|V| + |E|)$**.
> - **Paměťová složitost:** $O(|V|)$ pro pole vzdáleností a buckety.

---

## Úloha 11.3: Cesta s omezeným počtem vnějších vrcholů za sebou (Geralt z Rivie)

* **Zdroj:** FitWiki (Knop 1.12.2022)
* **Relevantní skripta:** [Algoritmus 1.1 (BFS)](https://truepeak78.github.io/ag1/01_Z%C3%A1klady_graf%C5%AF/1.2_BFS/#algo-1.1) · [Kapitola 1.2 (Stavové grafy)](https://truepeak78.github.io/ag1/01_Z%C3%A1klady_graf%C5%AF/1.2_BFS/)

### Zadání
Zaklínač Geralt hledá nejkratší cestu mezi dvěma místy v neorientovaném grafu $G = (V, E)$ ze startu $s$ do cíle $t$.
Množina vrcholů je rozdělena na dvě disjunktní skupiny $V = V_{\text{in}} \cup V_{\text{out}}$:
- $V_{\text{in}}$ jsou vnitřní (chráněné) vrcholy,
- $V_{\text{out}}$ jsou vnější (nebezpečné) vrcholy.

Pravidlo pohybu stanoví, že Geralt nesmí na své cestě projít **více než 5 vnějšími vrcholy bezprostředně za sebou** (jakmile vstoupí do vnitřního vrcholu z $V_{\text{in}}$, počítadlo po sobě jdoucích vnějších vrcholů se resetuje zpět na 0). Geralt se může do stejného vrcholu vrátit vícekrát, pokud se nachází v jiném stavu vyčerpání.
Navrhněte algoritmus v čase $O(|V| + |E|)$, který nalezne nejkratší přípustnou cestu z $s$ do $t$. Dokažte konečnost, korektnost a složitost.

> [!success]- Řešení
> ### 1. Konstrukce stavového grafu $G'$
> Počet navštívených vnějších vrcholů za sebou může nabývat pouze hodnot $k \in \{0, 1, 2, 3, 4, 5\}$.
> Vytvoříme stavový graf $G' = (V', E')$ skládající se z 6 vrstev:
> - **Stavové vrcholy $V'$:**
>   $$V' = V \times \{0, 1, 2, 3, 4, 5\}, \quad |V'| = 6 |V|.$$
>   Stav $(v, k)$ značí, že Geralt stojí ve vrcholu $v$ a právě prošel $k$ vnějšími vrcholy v řadě za sebou.
>
> - **Stavové hrany $E'$:**
>   Pro každou hranu $\{u, v\} \in E$ a každý stav $k \in \{0, \dots, 5\}$:
>   - **Případ 1 (Cílový vrchol $v \in V_{\text{in}}$ je vnitřní):**
>     Vstupem do $v$ se počítadlo resetuje na 0.
>     Z libovolného stavu $(u, k)$ vede hrana do stavu $((u, k), (v, 0))$.
>   - **Případ 2 (Cílový vrchol $v \in V_{\text{out}}$ je vnější):**
>     Vstupem do $v$ se počítadlo zvýší o 1.
>     Tento krok je povolen pouze tehdy, pokud $k < 5$.
>     Pokud $k \in \{0, 1, 2, 3, 4\}$, přidáme hranu do stavu $((u, k), (v, k + 1))$.
>     Pokud $k = 5$, hrana nevzniká (krok by překročil limit 5 vnějších vrcholů v řadě).
>   Počet hran v $G'$ je $|E'| \le 6 \cdot 2 |E| = 12 |E|$.
>
> ### 2. Algoritmus
> 1. Určíme počáteční stav:
>    - Pokud $s \in V_{\text{in}}$, počáteční uzel je $s' = (s, 0)$.
>    - Pokud $s \in V_{\text{out}}$, počáteční uzel je $s' = (s, 1)$.
> 2. Cílová množina je $T' = \{(t, k) \mid k \in \{0, 1, 2, 3, 4, 5\}\}$.
> 3. Spustíme standardní neohodnocený algoritmus **BFS** na grafu $G'$ ze startu $s'$.
> 4. První dosažený uzel z množiny $T'$ dává nejkratší přípustnou cestu.
>
> ### 3. Důkaz konečnosti, korektnosti a složitosti
> - **Konečnost:** Graf $G'$ je konečný ($6|V|$ vrcholů a $\le 12|E|$ hran). BFS navštíví každý stavový uzel nejvýše jednou a skončí.
> - **Korektnost:** Každý přechod v $G'$ striktně vynucuje podmínku zadání (reset na 0 při vstupu do $V_{\text{in}}$, zvýšení o 1 při vstupu do $V_{\text{out}}$, zablokování přechodu při $k=5$). BFS zaručuje nalezení cesty s minimálním počtem hran podle Věty 1.2.
> - **Časová složitost:** $O(|V'| + |E'|) = O(6|V| + 12|E|) = \mathbf{O(|V| + |E|)}$.
> - **Paměťová složitost:** $O(|V|)$ pro pole navštívených stavů velikosti $6|V|$.

---

## Úloha 11.4: Počet všech cest v DAGu v lineárním čase $O(|V| + |E|)$

* **Zdroj:** FitWiki (Mitura 2017)
* **Relevantní skripta:** [Definice 3.4 a Algoritmus 3.1 (Topologické uspořádání)](https://truepeak78.github.io/ag1/03_Stromy/3.4_Topsort/#def-3.4) · [Věta 3.7 (Existence TopSort v DAGu)](https://truepeak78.github.io/ag1/03_Stromy/3.4_Topsort/#theorem-3.7)

### Zadání
Mějme orientovaný acyklický graf (DAG) $G = (V, E)$ a zadaný startovní vrchol $s \in V$.
Navrhněte algoritmus, který pro každý vrchol $v \in V$ určí počet různých orientovaných cest ze startu $s$ do vrcholu $v$.
Algoritmus musí pracovat v časové složitosti $O(|V| + |E|)$. Dokažte korektnost a analyzujte časovou složitost.

> [!success]- Řešení
> ### 1. Princip algoritmu (Dynamické programování v topologickém uspořádání)
> V obecném grafu s cykly může být počet cest nekonečný. V DAGu však neexistují žádné orientované cykly, všechny hrany směřují v souladu s topologickým uspořádáním.
> 
> Označme $C[v]$ počet různých orientovaných cest z $s$ do $v$.
> Platí základní rekurence:
> - Pro startovní vrchol: $C[s] = 1$ (cesta nulové délky tvořená pouze uzlem $s$).
> - Pro libovolný vrchol $v \neq s$: Každá cesta do $v$ musí vstoupit přes nějakou přicházející hranu $(u, v) \in E$. Proto:
>   $$C[v] = \sum_{(u, v) \in E} C[u].$$
> 
> ### 2. Algoritmus
> 1. Provedeme topologické uspořádání grafu $G$ pomocí algoritmu `TopSort` ([Algoritmus 3.1](https://truepeak78.github.io/ag1/03_Stromy/3.4_Topsort/#algo-3.1)) $\implies$ získáme posloupnost vrcholů $(v_1, v_2, \dots, v_n)$.
> 2. Inicializujeme pole počtu cest:
>    - $C[v] \leftarrow 0$ pro všechny $v \in V$.
>    - $C[s] \leftarrow 1$.
> 3. Procházíme vrcholy $u$ v topologickém pořadí:
>    - Pokud $C[u] > 0$:
>      - Pro každou vystupující hranu $(u, w) \in E$:
>        $$C[w] \leftarrow C[w] + C[u].$$
> 4. Vrátíme pole $C$.
>
> ### 3. Důkaz korektnosti
> - Díky vlastnosti topologického uspořádání vedou všechny hrany pouze dopředu (od menších indexů k větším).
> - Když algoritmus zpracovává vrchol $w$, byly již dříve zpracovány všechny vrcholy $u$, ze kterých vede hrana do $w$ (neboť $\pi(u) < \pi(w)$).
> - Hodnota $C[u]$ je v okamžiku přičtení již finální a nemůže se dále změnit.
> - Každá orientovaná cesta z $s$ do $w$ má jednoznačného bezprostředního předchůdce $u$. Sečtením počtu cest přes všechny přímé předchůdce získáme přesný počet všech disjunktních cest do $w$.
>
> ### 4. Časová a paměťová složitost
> - Topologické uspořádání pomocí prohledávání / stupňů trvá $O(|V| + |E|)$.
> - Inicializace pole: $O(|V|)$.
> - Procházení vrcholů a relaxace hran: Každý vrchol je navštíven jednou a přes každou hranu $(u, w)$ se přičte hodnota právě jednou $\implies O(|V| + |E|)$.
> - Celková časová složitost je **$O(|V| + |E|)$**.
> - Paměťová složitost je $O(|V|)$ pro uložení pole $C$.

---

## Úloha 11.5: Výpočet sumy $\max(\text{rank}(v), k)$ na binárním stromu v čase $O(n)$

* **Zdroj:** FitWiki (Dvořák 2.12.2024)
* **Relevantní skripta:** [Definice 3.1 (Stromy a podstromy)](https://truepeak78.github.io/ag1/03_Stromy/3.1_Z%C3%A1kladn%C3%AD_pojmy/#def-3.1)

### Zadání
Mějme zakořeněný binární strom $T$ s $n$ vrcholy a zadané přirozené číslo $k \ge 1$.
Pro každý vrchol $v \in V(T)$ definujeme $\text{rank}(v)$ jako počet vrcholů v podstromu zakořeněném ve vrcholu $v$ (včetně vrcholu $v$ samotného).
Navrhněte algoritmus, který v čase $O(n)$ spočítá celkovou sumu:
$$S = \sum_{v \in V(T)} \max\big(\text{rank}(v), k\big)$$
Dokažte korektnost, konečnost a odvoďte časovou a paměťovou složitost.

> [!success]- Řešení
> ### 1. Princip algoritmus (Post-order průchod)
> Hodnota $\text{rank}(v)$ závisí výhradně na počtu vrcholů v levém a pravém podstromu:
> $$\text{rank}(v) = 1 + \text{rank}(v.\text{left}) + \text{rank}(v.\text{right})$$
> kde pro prázdný podstrom (`null`) definujeme $\text{rank}(\text{null}) = 0$.
> 
> Abychom spočítali $\text{rank}(v)$ v čase $O(1)$, musíme znát velikosti podstromů jeho synů. Toho dosáhneme standardním **post-order průchodem** stromu (rekurzí od listů ke kořeni).
>
> ### 2. Algoritmus (Pseudokód)
> ```text
> ComputeSum(root, k):
>     total_sum = 0
>     
>     // Pomocná rekurzivní funkce vracející rank aktuálního uzlu
>     PostOrder(u):
>         if u == null:
>             return 0
>         
>         left_rank = PostOrder(u.left)
>         right_rank = PostOrder(u.right)
>         
>         u_rank = 1 + left_rank + right_rank
>         total_sum = total_sum + max(u_rank, k)
>         
>         return u_rank
>     
>     PostOrder(root)
>     return total_sum
> ```
>
> ### 3. Důkaz konečnosti a korektnosti
> - **Konečnost:** Strom $T$ má $n$ vrcholů a je konečný a acyklický. Funkce `PostOrder` navštíví každý vrchol stromu právě jednou. Rekurze se nezacyklí a po projití $n$ vrcholů skončí.
> - **Korektnost:**
>   - Matematickou indukcí podle výšky podstromu:
>     - Pro listy: synové jsou `null` $\implies \text{left\_rank} = 0, \text{right\_rank} = 0 \implies \text{rank} = 1 + 0 + 0 = 1$, což je správná velikost jednoprvkového podstromu.
>     - Pro vnitřní uzel: z indukčního předpokladu vrátí volání pro levého a pravého syna správné počty vrcholů v jejich podstromech. Protože podstromy levého a pravého syna v binárním stromu jsou disjunktní, je počet vrcholů podstromu uzlu $u$ roven $1 + |V(T_{\text{left}})| + |V(T_{\text{right}})|$, což je přesně hodnota `u_rank`.
>   - V každém uzlu se k celkové sumě `total_sum` přičte přesně hodnota $\max(\text{rank}(u), k)$. Po návratu z kořene obsahuje proměnná `total_sum` přesný součet přes všechny vrcholy stromu $T$.
>
> ### 4. Časová a paměťová složitost
> - **Časová složitost:** Každý uzel je navštíven právě jednou. V každém uzlu se provedou pouze základní aritmetické operace sčítání a porovnání s $k$ v čase $O(1)$. Celková časová složitost je:
>   $$T(n) = n \cdot O(1) = \mathbf{O(n)}.$$
> - **Paměťová složitost:** Paměť je dána hloubkou zásobníku volání rekurze. Pro vyvážený strom je to $O(\log n)$, v nejhorším případě (degenerovaný strom na cestu) $O(n)$.


---

## Úloha 11.6: Pizzerie a doručení s omezením odbočování vlevo
* **Zdroj:** FitWiki (Pokorný 2021 `sbirka-2021`, scan testu `20211125_091802.jpg`)
* **Relevantní skripta:** [Sekce 6.1 (Prohledávání do šířky – BFS)](https://truepeak78.github.io/ag1/06_Z%C3%A1kladn%C3%AD_grafov%C3%A9_algoritmy/6.1_Prohled%C3%A1v%C3%A1n%C3%AD_do_%C5%A1%C3%AD%C5%99ky/#algoritmus-bfs) · [Sekce 2.1 (Grafové modely a stavy)](https://truepeak78.github.io/ag1/02_Souvislost_graf%C5%AF/2.1_Souvislost_graf%C5%AF/)

### Zadání
Město je modelováno jako neorientovaný graf $G = (V, E)$, kde vrcholy $V$ reprezentují křižovatky a hrany $E$ reprezentují obousměrné ulice.
Uprostřed jedné z ulic $e_0 = \{s_1, s_2\} \in E$ se nachází pizzerie.
Časová pravidla pohybu kurýra jsou následující:
- Průjezd celou ulicí (mezi dvěma křižovatkami) trvá přesně **1 minutu** (výjezd z pizzerie na křižovatku $s_1$ nebo $s_2$ trvá $0{,}5$ minuty).
- Na křižovatce: pokračování **rovně** nebo odbočení **doprava** trvá **1 minutu**.
- Odbočení **doleva** je z bezpečnostních důvodů zakázáno (trvá neomezeně dlouho / nelze provést).
- Otočení se do protisměru na křižovatce je rovněž zakázáno.

Na vstupu dostanete cílový dům nacházející se na křižovatce $d \in V$.
Navrhněte algoritmus, který rozhodne, zda je kurýr schopen doručit pizzu do domu $d$ v limitu **do 15 minut**. Dokažte korektnost, konečnost a odvoďte časovou a paměťovou složitost.

> [!success]- Řešení
> ### 1. Konstrukce stavového grafu $G' = (V', E')$
> Problém nelze řešit přímo na vrcholech původního grafu $V$, protože povolené směry odbočení na křižovatce $v$ závisí na tom, ze které předchozí křižovatky $u$ jsme do $v$ přijeli.
> 
> - **Množina stavů $V'$:**
>   Stavem je uspořádaná dvojice $(u, v)$ reprezentující **orientovaný příjezd** do křižovatky $v$ po ulici $\{u, v\} \in E$:
>   $$V' = \{ (u, v) \mid \{u, v\} \in E \}$$
>   Celkový počet stavů je roven počtu orientovaných hran: $|V'| = 2 |E|$.
>   Navíc zavedeme počáteční fiktivní stav $S_0$ (start v pizzerii).
> 
> - **Množina přechodů $E'$:**
>   Ze stavu $(u, v)$ vede hrana do stavu $(v, w)$ (kde $\{v, w\} \in E$ a $w \neq u$) právě tehdy, když geometrický manévr z $u$ přes $v$ do $w$ je **rovně** nebo **doprava**.
>   - Čas takového kroku: $1\text{ min (průjezd křižovatkou } v) + 1\text{ min (průjezd ulicí } \{v, w\}) = \mathbf{2 \text{ minuty}}$.
>   - Všechny přechody v grafu $G'$ mají **stejnou váhu 2 minuty**!
> 
> - **Počáteční přechody ze startu $S_0$:**
>   Kurýr může z pizzerie vyrazit do $s_1$ (stav $(s_2, s_1)$) za $0{,}5$ minuty, nebo do $s_2$ (stav $(s_1, s_2)$) za $0{,}5$ minuty.
> 
> ---
> 
> ### 2. Algoritmus
> Protože po prvním kroku (za $0{,}5$ min) mají všechny další přechody jednotnou délku 2 minuty, použijeme standardní **BFS (prohledávání do šířky)** od stavů $(s_2, s_1)$ a $(s_1, s_2)$ do maximálního časového limitu:
> $$\text{Počet kroků} \le \left\lfloor \frac{15 - 0{,}5}{2} \right\rfloor = \lfloor 7{,}25 \rfloor = \mathbf{7 \text{ kroků}}.$$
> 
> ```text
> Algorithm CanDeliverPizza(G, e0={s1, s2}, d):
>     Queue Q
>     Dist array initialized to infinity
>     
>     // Inicializace ze dvou možných směrů výjezdu z pizzerie
>     Dist[(s2, s1)] = 0.5;  Q.push((s2, s1))
>     Dist[(s1, s2)] = 0.5;  Q.push((s1, s2))
>     
>     while Q is not empty:
>         (u, v) = Q.pop()
>         
>         if v == d and Dist[(u, v)] <= 15.0:
>             return TRUE
>         
>         if Dist[(u, v)] + 2.0 > 15.0:
>             continue
>             
>         for each neighbor w of v (w != u):
>             if Maneuver(u, v, w) in {STRAIGHT, RIGHT}:
>                 if Dist[(v, w)] == infinity:
>                     Dist[(v, w)] = Dist[(u, v)] + 2.0
>                     Q.push((v, w))
>                     
>     return FALSE
> ```
> 
> ### 3. Důkaz konečnosti a korektnosti
> - **Konečnost:** Stavový prostor má nejvýše $2|E|$ stavů. Algoritmus BFS navštíví každý stav nejvýše jednou (díky poli `Dist`). Proto algoritmus vždy skončí po prozkoumání nanejvýš $2|E|$ stavů.
> - **Korektnost:** Všechny přechody ve stavovém prostoru přesně reprezentují povolené fyzické jízdy (vylučují odbočení doleva a otáčení do protisměru). Protože všechny hrany mají stejnou váhu 2, BFS garantuje nalezení cesty s minimálním počtem přechodů, a tedy s minimálním časem příjezdu.
> 
> ### 4. Časová a paměťová složitost
> - Počet stavů: $|V'| = 2|E|$.
> - Počet přechodů: Z každého stavu $(u, v)$ vedou hrany nejvýše do $\deg(v) - 1$ dalších stavů. Celkový počet hran je $|E'| = \sum_{v \in V} \deg(v)^2 = O(|V| \cdot \Delta)$.
> - BFS projde stavy a hrany v čase $O(|V'| + |E'|) = \mathbf{O(|E| + \sum \deg(v)^2)}$.
> - Paměťová složitost je $O(|E|)$ pro frontu a pole vzdáleností.

---

## Úloha 11.7: Hladová a líná sytá kočička
* **Zdroj:** FitWiki (Dvořák 2022 `dvorak2022`)
* **Relevantní skripta:** [Sekce 6.1 (BFS a vážené modifikace)](https://truepeak78.github.io/ag1/06_Z%C3%A1kladn%C3%AD_grafov%C3%A9_algoritmy/6.1_Prohled%C3%A1v%C3%A1n%C3%AD_do_%C5%A1%C3%AD%C5%99ky/)

### Zadání
Mějme neorientovaný neohodnocený graf $G = (V, E)$, kde kočička začíná ve vrcholu $s \in V$ a chce se dostat do svého pelíšku ve vrcholu $t \in V$.
V grafu se nachází množina misek s jídlem $M \subseteq V$.
Pravidla pohybu kočičky:
1. Kočička se **před usnutím v pelíšku musí alespoň jednou napapat** v některé misce $m \in M$ (pokud je $s \in M$, může se napapat hned na startu).
2. Dokud je kočička **hladová**, překonání každé hrany jí trvá **1 minutu**.
3. Jakmile se kočička napapá, stane se sytou a línou, a od tohoto okamžiku jí překonání každé hrany trvá **2 minuty**.
4. Jakmile se kočička jednou napapá, zůstává sytá po celý zbytek cesty (stav je nevratný).

Navrhněte algoritmus, který v lineárním čase $O(|V| + |E|)$ nalezne nejrychlejší možnou cestu kočičky ze startu $s$ do pelíšku $t$, nebo oznámí, že cíl je nedosažitelný.

> [!success]- Řešení
> ### 1. Stavový prostor
> Stav kočičky je jednoznačně popsán dvojicí $(v, \text{fed})$, kde:
> - $v \in V$ je aktuální vrchol grafu,
> - $\text{fed} \in \{0, 1\}$ indikuje stav: $0 = \text{hladová}$, $1 = \text{sytá}$.
> 
> Celkový počet stavů je $|V'| = 2 |V|$.
> 
> **Přechody mezi stavy:**
> Pro každou hranu $\{u, v\} \in E$:
> 1. **Pohyb za hladova:** $(u, 0) \to (v, 0)$ s váhou $w = 1$.
> 2. **Pohyb a současné najezení (pokud $v \in M$):** $(u, 0) \to (v, 1)$ s váhou $w = 1$ (kočička doběhla do misky hladová za 1 min a v cílovém vrcholu $v$ se najedla).
> 3. **Pohyb za syta:** $(u, 1) \to (v, 1)$ s váhou $w = 2$.
> 
> Startovní stav:
> - Pokud $s \in M$: startujeme ve stavu $(s, 1)$ s časem 0 (napapá se hned na startu) a případně i $(s, 0)$.
> - Jinak: startujeme ve stavu $(s, 0)$ s časem 0.
> 
> Cílový stav: stav $(t, 1)$ (pelíšek v sytém stavu).
> 
> ---
> 
> ### 2. Transformace na neohodnocený graf a BFS v čase $O(|V| + |E|)$
> Váhy hran ve stavovém prostoru jsou pouze z množiny $\{1, 2\}$.
> Abychom nemuseli volat Dijkstrův algoritmus s haldou (který by měl složitost $O(|E| \log |V|)$), transformujeme každou hranu váhy 2 na **dvě sériové hrany váhy 1**:
> - Pro každou hranu $\{u, v\} \in E$ ve vrstvě sytosti rozdělíme přechod $(u, 1) \to (v, 1)$ vložením fiktivního vrcholu $X_{\{u,v\}}$:
>   $$(u, 1) \xrightarrow{\text{váha } 1} X_{\{u,v\}} \xrightarrow{\text{váha } 1} (v, 1)$$
> 
> **Velikost rozšířeného grafu:**
> - Počet vrcholů: $|V''| = 2|V| + |E| = O(|V| + |E|)$.
> - Počet hran: $|E''| \le 2|E| + 2|E| = O(|E|)$.
> - Všechny hrany mají váhu přesně 1.
> 
> ### 3. Algoritmus
> 1. Sestrojíme graf $G'' = (V'', E'')$.
> 2. Spustíme standardní **BFS** ze startovního vrcholu $(s, 0)$ (resp. $(s, 1)$).
> 3. Po ukončení BFS vrátíme vzdálenost do vrcholu $(t, 1)$. Pokud je vrchol nedosažitelný, vrátíme zprávu o neexistenci cesty.
> 
> ### 4. Složitost
> - Konstrukce stavového prostoru vyžaduje $O(|V| + |E|)$ času.
> - BFS na grafu $G''$ navštíví každý vrchol a hranu nejvýše jednou:
>   $$T(|V|, |E|) = O(|V''| + |E''|) = \mathbf{O(|V| + |E|)}.$$
> - Paměťová složitost je $O(|V| + |E|)$ pro uložení stavového grafu a BFS fronty.

---

## Úloha 11.8: Křeček ve víceúrovňovém bludišti se skluzavkami dolů
* **Zdroj:** FitWiki (Knop 2023 `knop_2023_11_30`, Opravný 2021 `opravny_2021`)
* **Relevantní skripta:** [Sekce 5.1 (Topologické uspořádání a acyklické orientované grafy)](https://truepeak78.github.io/ag1/05_Orientovan%C3%A9_grafy/5.1_Z%C3%A1kladn%C3%AD_pojmy/#def-5.3) · [Sekce 6.1 (BFS)](https://truepeak78.github.io/ag1/06_Z%C3%A1kladn%C3%AD_grafov%C3%A9_algoritmy/6.1_Prohled%C3%A1v%C3%A1n%C3%AD_do_%C5%A1%C3%AD%C5%99ky/)

### Zadání
Křeček se pohybuje v mnohopatrovém teráriu s patry očíslovanými $1, 2, \dots, K$.
- Každé patro $i$ obsahuje množinu místností $V_i$ a obousměrných chodeb $E_i$. Průchod chodbou na stejném patře trvá **1 sekundu**.
- Mezi místnostmi různých pater existují jednosměrné skluzavky $S$. Každá skluzavka vede výhradně **z vyššího patra do ostře nižšího patra** (tj. z patra $i$ do patra $j$, kde $i < j$) a její sjetí trvá **1 sekundu**.
- Z nižšího patra do vyššího se křeček **nikdy nemůže vrátit** (všechny mezivrstevní přechody jsou ostře jednosměrné směrem dolů).

Křeček začíná v místnosti $s \in V_1$ a chce se dostat do místnosti $t \in V_K$ v nejkratším možném čase.
Navrhněte algoritmus, který nalezne nejkratší čas průchodu v lineárním čase $O(|V| + |E|)$ vůči celkovému počtu místností a chodeb se skluzavkami.

> [!success]- Řešení
> ### 1. Analýza struktury grafu
> Celý graf bludiště $G = (V, E_{\text{all}})$ má množinu vrcholů $V = \bigcup_{i=1}^K V_i$ a hrany:
> $$E_{\text{all}} = \left(\bigcup_{i=1}^K E_i\right) \cup S$$
> Všechny hrany mají jednotkovou váhu $w(e) = 1$.
> 
> Klíčové pozorování: Mezi patry vedou pouze skluzavky s $i < j$. Jakmile křeček sjede skluzavkou do patra $j$, už nikdy nemůže navštívit žádné patro $i \le j - 1$.
> Graf pater tvoří orientovaný acyklický graf (DAG).
> 
> ### 2. Algoritmus
> Protože **všechny hrany** (chodby uvnitř pater i skluzavky mezi patry) mají přesně stejnou délku **1 sekunda**, jedná se o standardní problém nejkratší cesty v neohodnoceném orientovaném grafu!
> 
> K nalezení nejkratší cesty v neohodnoceném grafu stačí spustit standardní **BFS**:
> ```text
> HamsterShortestPath(G, s, t):
>     Dist array initialized to infinity for all v in V
>     Queue Q
>     
>     Dist[s] = 0
>     Q.push(s)
>     
>     while Q is not empty:
>         u = Q.pop()
>         
>         if u == t:
>             return Dist[t]
>             
>         for each directed neighbor v of u:
>             if Dist[v] == infinity:
>                 Dist[v] = Dist[u] + 1
>                 Q.push(v)
>                 
>     return "Cíl nedosažitelný"
> ```
> 
> ### 3. Korektnost a složitost
> - **Korektnost:** Věta o korektnosti BFS z přednášky garantuje, že prohledávání do šířky na neohodnoceném grafu prozkoumává vrcholy v neklesajícím pořadí podle vzdálenosti ze startu $s$ a nalezne nejkratší cestu do všech dosažitelných vrcholů. Absence cyklů přes skluzavky navíc vylučuje nekonečné smyčky mezi patry.
> - **Časová složitost:** Každý vrchol je vložen do fronty nejvýše jednou a každá hrana je prozkoumána nejvýše jednou. Časová složitost je přesně:
>   $$T(V, E) = \mathbf{O(|V| + |E_{\text{all}}|)}$$
> - **Paměťová složitost:** $O(|V|)$ pro frontu a pole vzdáleností.

---

## Úloha 11.9: Nejkratší sled přes podmnožinu 8 klíčových vrcholů
* **Zdroj:** FitWiki (Hušek 2023 `husek_2023_11_29`)
* **Relevantní skripta:** [Sekce 6.1 (Prohledávání do šířky ve stavovém prostoru)](https://truepeak78.github.io/ag1/06_Z%C3%A1kladn%C3%AD_grafov%C3%A9_algoritmy/6.1_Prohled%C3%A1v%C3%A1n%C3%AD_do_%C5%A1%C3%AD%C5%99ky/)

### Zadání
Dostanete neorientovaný neohodnocený graf $G = (V, E)$, počáteční vrchol $s$, cílový vrchol $t$ a pevnou podmnožinu klíčových vrcholů $A \subseteq V$ o velikosti $|A| = 8$.
Dostat se do cíle $t$ je povoleno **pouze po navštívení všech 8 vrcholů z množiny $A$** (v libovolném pořadí).
Vrcholy i hrany grafu $G$ je dovoleno navštívit opakovaně (hledáme nejkratší sled).
Navrhněte algoritmus, který najde délku nejkratšího takového sledu. Dokažte správnost, konečnost a časovou složitost.

> [!success]- Řešení
> ### 1. Konstrukce stavového prostoru
> Protože množina klíčových vrcholů $A$ je malá ($|A| = 8$), můžeme množinu dosud navštívených klíčových vrcholů zakódovat jako stav:
> 
> - **Množina stavů $V'$:**
>   $$V' = V \times \mathcal{P}(A)$$
>   kde $\mathcal{P}(A)$ je potenční množina $A$.
>   Stav je dvojice $(v, S)$, kde $v \in V$ je aktuální pozice a $S \subseteq A$ je množina dosud navštívených klíčových vrcholů.
>   Množinu $S$ reprezentujeme jako 8bitové číslo (bitovou masku): bit na pozici $i$ je 1 právě tehdy, když byl $i$-tý vrchol z $A$ již navštíven.
>   Počet stavů je:
>   $$|V'| = |V| \cdot 2^{|A|} = 2^8 \cdot |V| = \mathbf{256 |V|}$$
> 
> - **Přechody $E'$:**
>   Pro každou hranu $\{u, v\} \in E$ a každou podmnožinu $S \subseteq A$ vytvoříme orientovaný přechod:
>   $$(u, S) \longrightarrow (v, S \cup (\{v\} \cap A))$$
>   s jednotkovou váhou $w = 1$.
>   Počet přechodů je:
>   $$|E'| = 2 \cdot 2^{|A|} |E| = \mathbf{512 |E|}$$
> 
> - **Startovní a cílový stav:**
>   - Startovní stav: $(s, \{s\} \cap A)$.
>   - Cílový stav: libovolný stav tvaru $(t, A)$ (vrchol $t$, kde maska obsahuje všech 8 bitů nastavených na 1).
> 
> ---
> 
> ### 2. Algoritmus (BFS nad stavovým prostorem)
> Protože všechny hrany mají jednotkovou délku 1, spustíme standardní BFS:
> ```text
> ShortestVisitAll(G, s, t, A):
>     Dist array of size |V| x 256, initialized to infinity
>     Queue Q
>     
>     start_mask = (s in A) ? bit(s) : 0
>     Dist[s][start_mask] = 0
>     Q.push((s, start_mask))
>     
>     all_visited_mask = (1 << 8) - 1   // 255 (všech 8 bitů)
>     
>     while Q is not empty:
>         (u, mask) = Q.pop()
>         
>         if u == t and mask == all_visited_mask:
>             return Dist[u][mask]
>             
>         for each neighbor v of u in G:
>             next_mask = mask | ((v in A) ? bit(v) : 0)
>             if Dist[v][next_mask] == infinity:
>                 Dist[v][next_mask] = Dist[u][mask] + 1
>                 Q.push((v, next_mask))
>                 
>     return "Cesta neexistuje"
> ```
> 
> ### 3. Důkaz správnosti a konečnosti
> - **Konečnost:** Stavový prostor má konečnou velikost $256 |V|$. BFS navštíví každý stav nejvýše jednou, algoritmus se tedy nemůže zacyklit a vždy skončí.
> - **Správnost:** Každý platný sled v grafu $G$, který navštíví všech 8 vrcholů $A$, jednoznačně odpovídá orientované cestě ve stavovém prostoru ze stavu $(s, \text{start\_mask})$ do stavu $(t, \text{all\_visited})$. BFS na neohodnoceném grafu zaručeně nalezne cestu s minimálním počtem hran.
> 
> ### 4. Časová a paměťová složitost
> - Časová složitost BFS je lineární vůči velikosti stavového prostoru:
>   $$T(|V|, |E|) = O(|V'| + |E'|) = O(256 |V| + 512 |E|) = \mathbf{O(|V| + |E|)}$$
> - Pro pevné $|A| = 8$ je algoritmus striktně lineární v čase i paměti.

---

## Úloha 11.10: Jednoznačnost topologického uspořádání a míchání chemikálií
* **Zdroj:** FitWiki (Hušek 2024 `husek_2024_12_5`, Scholtzová 2020 `scholtzova2020`)
* **Relevantní skripta:** [Definice 5.3 a Věta 5.1 (Topologické uspořádání a acykličnost)](https://truepeak78.github.io/ag1/05_Orientovan%C3%A9_grafy/5.1_Z%C3%A1kladn%C3%AD_pojmy/#def-5.3)

### Zadání
Máme množinu $n$ chemikálií $S = \{s_1, s_2, \dots, s_n\}$ a dokumentaci popisující bezpečnostní omezení: pravidlo $s_i \to s_j$ znamená, že chemikálie $s_i$ musí být do směsi přimíchána **dříve** než chemikálie $s_j$, jinak směs exploduje.
Navrhněte algoritmus, který v čase $O(|V| + |E|)$:
1. Zjistí, zda vůbec existuje bezpečné pořadí smíchání všech chemikálií.
2. Pokud existuje, rozhodne, zda je toto bezpečné pořadí **jednoznačné**, nebo zda existují **alespoň 2 různá** bezpečná uspořádání.
Dokažte korektnost a odvoďte složitost.

> [!success]- Řešení
> ### 1. Grafový model
> Vytvoříme orientovaný graf $G = (V, E)$, kde:
> - Vrcholy $V$ reprezentují jednotlivé chemikálie $s_1, \dots, s_n$, tedy $|V| = n$.
> - Orientovaná hrana $(s_i, s_j) \in E$ vede z $s_i$ do $s_j$ právě tehdy, když $s_i$ musí předcházet $s_j$.
> 
> Bezpečné pořadí smíchání odpovídá **topologickému uspořádání** orientovaného grafu $G$.
> - Existence bezpečného pořadí je ekvivalentní tomu, že graf $G$ je **acyklický (DAG)** ([Věta 5.1](https://truepeak78.github.io/ag1/05_Orientovan%C3%A9_grafy/5.1_Z%C3%A1kladn%C3%AD_pojmy/#theorem-5.1)).
> - Jednoznačnost topologického uspořádání nastává právě tehdy, když v DAGu existuje **orientovaná Hamiltonovská cesta** (cesta procházející všemi vrcholy).
> 
> ---
> 
> ### 2. Algoritmus (Modifikovaný Kahnův algoritmus)
> Využijeme Kahnův algoritmus založený na frontě zdrojů (vrcholů se vstupním stupněm $\deg_{\text{in}}(v) = 0$):
> 
> ```text
> AnalyzeChemicalMixing(V, E):
>     Compute in-degree deg_in[v] for all v in V
>     Queue Q
>     for each v in V:
>         if deg_in[v] == 0:
>             Q.push(v)
>             
>     processed_count = 0
>     is_unique = TRUE
>     Order = []
>     
>     while Q is not empty:
>         // Klíčový test jednoznačnosti:
>         if Q.size() > 1:
>             is_unique = FALSE
>             
>         u = Q.pop()
>         Order.append(u)
>         processed_count = processed_count + 1
>         
>         for each directed neighbor w of u:
>             deg_in[w] = deg_in[w] - 1
>             if deg_in[w] == 0:
>                 Q.push(w)
>                 
>     if processed_count < |V|:
>         return "CHYBA: Existuje cyklus, žádné bezpečné pořadí neexistuje!"
>     else if is_unique:
>         return ("Jednoznačné pořadí:", Order)
>     else:
>         return "Existují alespoň 2 různá bezpečná uspořádání."
> ```
> 
> ### 3. Důkaz korektnosti
> 1. **Detekce cyklu:**
>    Podle Kahnova algoritmu: pokud graf obsahuje orientovaný cyklus, žádný vrchol na cyklu nemůže nikdy dosáhnout vstupního stupně 0. Algoritmus tedy skončí s `processed_count < |V|` a správně ohlásí neexistenci uspořádání.
> 
> 2. **Kritérium jednoznačnosti:**
>    - V každém kroku algoritmu představuje fronta $Q$ množinu všech dosud nezařazených chemikálií, které již nemají žádné nesplněné prerekvizity (mohou být přidány jako další v pořadí).
>    - **Pokud $|Q| = 1$ v každém kroku:** V každé pozici uspořádání máme právě jeden jediný kandidátní vrchol. Volba je v každém kroku deterministická a vynucená $\implies$ existuje právě jedno topologické uspořádání. Mezi každými dvěma po sobě zařazenými vrcholy navíc nutně vede hrana, což tvoří Hamiltonovskou cestu.
>    - **Pokud v některém kroku $|Q| \ge 2$:** Ve frontě leží alespoň dva nezávislé vrcholy $x, y \in Q$, mezi nimiž nevede žádná orientovaná hrana (ani cesta). Můžeme tedy buď zvolit $x$ a po něm $y$, nebo zvolit $y$ a po něm $x$. Obě volby lze nezávisle dokončit do platného topologického uspořádání, což přímo generuje **alespoň 2 různá** bezpečná uspořádání.
> 
> ### 4. Časová a paměťová složitost
> - Výpočet vstupních stupňů: $O(|V| + |E|)$.
> - Každý vrchol je vložen a vybrán z fronty právě jednou $\implies O(|V|)$.
> - Každá hrana je dekrementována právě jednou $\implies O(|E|)$.
> - Test velikosti fronty `Q.size() > 1` trvá $O(1)$.
> - Celková časová složitost je striktně lineární:
>   $$T(|V|, |E|) = \mathbf{O(|V| + |E|)}.$$
> - Paměťová složitost: $O(|V|)$ pro pole `deg_in` a frontu $Q$.
