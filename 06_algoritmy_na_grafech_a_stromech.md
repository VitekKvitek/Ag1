# 06. Algoritmické úlohy na grafech a stromech – Zadání z písemek a řešení

Tento dokument obsahuje algoritmické úlohy z 1. zápočtového testu, kde je požadován formální návrh algoritmu v lineárním čase $O(|V| + |E|)$ nebo $O(n)$, důkaz jeho korektnosti, konečnosti a časové složitosti.

---

## Úloha 6.1: Detekce kružnice v neorientovaném grafu v čase $O(|V| + |E|)$

* **Zdroj:** FitWiki (7.11.2025, Hušek 2022 & 2023)
* **Relevantní skripta:** [Algoritmus 1.1 a Věta 1.2 (BFS a jeho korektnost)](https://truepeak78.github.io/ag1/01_Z%C3%A1klady_graf%C5%AF/1.2_BFS/#algo-1.1) · [Věta 3.3 (Charakterizace stromů)](https://truepeak78.github.io/ag1/03_Stromy/3.2_Charakterizace_strom%C5%AF/#theorem-3.3)

### Zadání
Navrhněte algoritmus, který pro zadaný neorientovaný graf $G = (V, E)$ určí, zda obsahuje alespoň jednu kružnici.
Požadovaná časová složitost je $O(|V| + |E|)$. Dokažte konečnost, korektnost a odvoďte časovou složitost.

> [!success]- Řešení
> ### Myšlenka a algoritmus
> Graf neobsahuje kružnici právě tehdy, když je **lesem**.
> V lese s $c$ souvislými komponentami je celkový počet hran roven přesně $|E| = |V| - c$.
> Pokud v libovolné komponentě $K_i$ platí $|E(K_i)| \ge |V(K_i)|$, pak tato komponenta nutně obsahuje kružnici.
>
> **Algoritmus:**
> 1. **Rychlý test počtu hran:** Pokud $|E| \ge |V|$, vrátíme ihned **ANO** a skončíme.
>    *(Odůvodnění: v acyklickém grafu s $c \ge 1$ komponentami je $|E| = |V| - c \le |V| - 1 < |V|$. Pokud má graf alespoň $|V|$ hran, nutně obsahuje kružnici).*
> 2. Pokud $|E| < |V|$, inicializujeme pole navštívených vrcholů $\text{visited}[v] \leftarrow \text{false}$ pro všechny $v \in V$.
> 3. Procházíme vrcholy $v \in V$:
>    - Pokud $\text{visited}[v] == \text{false}$:
>      - Spustíme prohledávání (BFS nebo DFS) z vrcholu $v$.
>      - Během prohledávání spočítáme počet vrcholů $n_i$ a počet hran $m_i$ v této souvislé komponentě.
>      - Pokud $m_i \ge n_i$ (nebo pokud prohledávání narazí na hranu do již navštíveného vrcholu, který není přímým rodičem):
>        - Vrátíme **ANO** (graf obsahuje kružnici) a skončíme.
> 4. Projdeme-li všechny komponenty a v každé platí $m_i = n_i - 1$, vrátíme **NE** (graf je acyklický).
>
> ### Důkaz konečnosti
> Graf je konečný ($|V| < \infty, |E| < \infty$). Každý vrchol a každá hrana je prozkoumána nejvýše konstantněkrát v rámci BFS/DFS. Algoritmus se nezacyklí a po konečném počtu kroků skončí.
>
> ### Důkaz korektnosti
> - Pokud $|E| \ge |V|$: podle Věty o charakterizaci stromů a lesů má les vždy $|E| = |V| - c < |V|$. Graf s $|E| \ge |V|$ tedy nemůže být lesem a musí obsahovat alespoň jednu kružnici.
> - Pokud algoritmus v průběhu BFS najde komponentu s $m_i \ge n_i$: acyklická komponenta by byla stromem a měla by $m_i = n_i - 1$ hran ([Věta 3.3](https://truepeak78.github.io/ag1/03_Stromy/3.2_Charakterizace_strom%C5%AF/#theorem-3.3)). Spor, komponenta musí obsahovat kružnici.
> - Pokud algoritmus vrátí NE: v každé komponentě platí $m_i = n_i - 1$ a komponenta je souvislá. Podle Věty 3.3 je každá komponenta stromem, graf je tedy les a neobsahuje žádnou kružnici.
>
> ### Časová složitost
> - Počáteční porovnání $|E| \ge |V|$ trvá $O(1)$.
> - BFS se spouští pouze tehdy, když $|E| < |V|$. Prohledání všech komponent tedy trvá $O(|V| + |E|) = O(|V|)$.
> - Celková časová složitost splňuje zadání a je **$O(|V| + |E|)$** (dokonce $O(|V|)$).

---

## Úloha 6.2: Rozpoznání grafu „strom + 1 hrana“ v čase $O(|V| + |E|)$

* **Zdroj:** FitWiki (4.11.2025 Varianta D)
* **Relevantní skripta:** [Definice 2.1 a 2.2 (Souvislost a komponenty)](https://truepeak78.github.io/ag1/02_Souvislost_graf%C5%AF/2.1_Souvislost_graf%C5%AF/#def-2.1) · [Věta 3.3 (Charakterizace stromů)](https://truepeak78.github.io/ag1/03_Stromy/3.2_Charakterizace_strom%C5%AF/#theorem-3.3) · [Algoritmus 1.1 (BFS)](https://truepeak78.github.io/ag1/01_Z%C3%A1klady_graf%C5%AF/1.2_BFS/#algo-1.1)

### Zadání
Navrhněte algoritmus, který pro neorientovaný graf $G = (V, E)$ určí, zda vznikl z nějakého stromu přidáním právě jedné hrany.
Požadovaná časová složitost pro plný počet bodů je $O(|V| + |E|)$. Dokažte korektnost a analyzujte složitost.

> [!success]- Řešení
> ### Teoretická charakterizace
> Graf $G$ vznikl ze stromu přidáním právě jedné hrany právě tehdy, když:
> 1. $G$ je **souvislý** (přidáním hrany do souvislého grafu se souvislost neporuší).
> 2. Počet hran je roven přesně počtu vrcholů: $|E| = |V|$ (strom s $|V|$ vrcholy má $|V|-1$ hran, po přidání jedné hrany má $|V|-1+1 = |V|$ hran).
> 3. Graf nemá násobné hrany ani smyčky (jednoduchý graf).
>
> ### Algoritmus
> 1. Zkontrolujeme podmínku na počet hran:
>    - Pokud $|E| \neq |V|$, vrátíme **NE** a skončíme.
> 2. Zkontrolujeme souvislost:
>    - Pokud $|V| == 0$, vrátíme NE.
>    - Zvolíme libovolný vrchol $s \in V$ a spustíme standardní BFS z vrcholu $s$.
>    - Spočítáme počet navštívených vrcholů $k$.
>    - Pokud $k == |V|$, vrátíme **ANO** (graf je souvislý a má $|E| = |V|$).
>    - Jinak vrátíme **NE** (graf je nesouvislý).
>
> ### Důkaz korektnosti
> - $\implies$ (Nutnost): Vznikl-li $G$ ze stromu $T$ přidáním jedné hrany $e$, pak je $G$ souvislý (neboť $T$ byl souvislý) a má $|E(G)| = |E(T)| + 1 = (|V| - 1) + 1 = |V|$.
> - $\impliedby$ (Postačující podmínka): Nechť $G$ je souvislý a $|E| = |V|$. Zvolme libovolnou kostru $T$ grafu $G$. Kostra má $|V| - 1$ hran. Množina zbývajících hran mimo kostru má velikost $|E| - (|V| - 1) = |V| - (|V| - 1) = 1$. Graf $G$ se tedy skládá z kostry $T$ (což je strom) a právě jedné další hrany. Podmínka je ekvivalentní.
>
> ### Časová složitost
> - Porovnání $|E| == |V|$: $O(1)$.
> - BFS průchod: navštíví každý vrchol a projde každou hranu právě jednou, což trvá $O(|V| + |E|)$.
> - Celková časová složitost je **$O(|V| + |E|)$**.

---

## Úloha 6.3: Uspořádání vrcholů stromu s nejvýše 1 sousedem v prefixu

* **Zdroj:** FitWiki (4.11.2025 Varianta A, Křišťan 2022, Opler 2022, Knop 2022)
* **Relevantní skripta:** [Algoritmus 1.1 (BFS)](https://truepeak78.github.io/ag1/01_Z%C3%A1klady_graf%C5%AF/1.2_BFS/#algo-1.1) · [Definice 3.1 a Věta 3.3 (Stromy)](https://truepeak78.github.io/ag1/03_Stromy/3.2_Charakterizace_strom%C5%AF/#theorem-3.3)

### Zadání
Mějme strom $T = (V, E)$ s $n$ vrcholy.
Navrhněte algoritmus, který najde pořadí vrcholů $v_1, v_2, \dots, v_n$ takové, že pro každý vrchol $v_i$ ($i \in \{2, \dots, n\}$) se v množině $\{v_1, \dots, v_{i-1}\}$ nachází **právě jeden** jeho soused.
Algoritmus musí pracovat v časové složitosti $O(n)$. Dokažte konečnost, korektnost a složitost.

> [!success]- Řešení
> ### Algoritmus (Průchod stromem pomocí BFS z libovolného kořene)
> 1. Zvolíme libovolný vrchol $r \in V$ jako kořen a položíme $v_1 \leftarrow r$.
> 2. Spustíme standardní BFS z vrcholu $r$:
>    - Fronta $Q \leftarrow [r]$, pole $\text{visited}[r] \leftarrow \text{true}$, pro ostatní $\text{false}$.
>    - Výstupní seznam $L \leftarrow []$.
>    - Dokud $Q$ není prázdná:
>      - Vyjmeme vrchol $u$ z čela $Q$, přidáme $u$ na konec seznamu $L$.
>      - Pro každého souseda $w \in N(u)$:
>        - Pokud $\text{visited}[w] == \text{false}$:
>          - $\text{visited}[w] \leftarrow \text{true}$
>          - Vložíme $w$ do $Q$.
> 3. Vrátíme posloupnost $L = (v_1, v_2, \dots, v_n)$.
>
> ### Důkaz konečnosti
> Strom má $n$ vrcholů. Každý vrchol je vložen do fronty $Q$ a vyjmut z ní právě jednou. Algoritmus provede právě $n$ iterací a skončí.
>
> ### Důkaz korektnosti
> Uvažujme libovolný vrchol $v_i$ na pozici $i \ge 2$ ve výsledném seznamu:
> - Vrchol $v_i$ byl vložen do fronty $Q$ v okamžiku, kdy byl zpracováván nějaký jeho soused $u = \text{parent}(v_i)$.
> - Tento rodič $u$ musel být vyjmut z fronty před vrcholem $v_i$, tedy $u \in \{v_1, \dots, v_{i-1}\}$. Vrchol $v_i$ má tedy v prefixu alespoň jednoho souseda (svého rodiče $u$).
> - Mohl by mít $v_i$ v prefixu ještě dalšího souseda $w \in \{v_1, \dots, v_{i-1}\}$ ($w \neq u$)?
>   - Kdyby měl, pak by v $T$ existovala hrana $\{v_i, w\}$.
>   - Protože $u, w \in \{v_1, \dots, v_{i-1}\}$ a oba byly navštíveny před $v_i$, existují cesty z kořene $r$ do $u$ i do $w$, které neobsahují vrchol $v_i$.
>   - Přidáním hran $\{u, v_i\}$ a $\{v_i, w\}$ by vznikla kružnice procházející vrcholem $v_i$.
>   - To je však **spor** s tím, že $T$ je strom (acyklický graf).
> Každý vrchol $v_i$ ($i \ge 2$) má v množině $\{v_1, \dots, v_{i-1}\}$ právě jednoho souseda.
>
> ### Časová složitost
> Strom má $|V| = n$ a $|E| = n - 1$.
> Průchod BFS nad stromem má složitost $O(|V| + |E|) = O(n + n - 1) = \mathbf{O(n)}$.

---

## Úloha 6.4: Existence souvislého indukovaného podgrafu libovolné velikosti $k$

* **Zdroj:** FitWiki (7.11.2025)
* **Relevantní skripta:** [Věta 3.4 (Existence kostry)](https://truepeak78.github.io/ag1/03_Stromy/3.3_Kostra_grafu/#theorem-3.4) · [Věta 3.2 (Existence listů ve stromu)](https://truepeak78.github.io/ag1/03_Stromy/3.1_Z%C3%A1kladn%C3%AD_pojmy/#theorem-3.2)

### Zadání
Nechť $G = (V, E)$ je souvislý neorientovaný graf o $n$ vrcholech.
Dokažte, že pro každé celé číslo $k \in \{1, 2, \dots, n\}$ existuje v $G$ **souvislý indukovaný podgraf** o právě $k$ vrcholech.

> [!success]- Řešení
> **Důkaz (konstrukcí přes odtrhávání listů kostry):**
> 1. Protože $G$ je souvislý graf, existuje v něm kostra $T = (V, E_T)$ ([Věta 3.4](https://truepeak78.github.io/ag1/03_Stromy/3.3_Kostra_grafu/#theorem-3.4)), která obsahuje všech $n$ vrcholů a je stromem.
> 2. Provedeme indukci pozpátku od $n$ dolů k $1$:
>    - Pro $k = n$: Množina všech vrcholů $V$ indukuje celý graf $G[V] = G$, který je ze zadání souvislý.
>    - Indukční krok ($m \to m - 1$): Nechť máme souvislý strom $T_m$ o $m$ vrcholech ($m \ge 2$).
>      - Podle [Věty 3.2](https://truepeak78.github.io/ag1/03_Stromy/3.1_Z%C3%A1kladn%C3%AD_pojmy/#theorem-3.2) obsahuje každý strom s $m \ge 2$ vrcholy alespoň dva listy (vrcholy stupně 1).
>      - Zvolme jeden list $l$ stromu $T_m$ a odeberme ho: $T_{m-1} = T_m - l$.
>      - Odebráním listu ze stromu se neporuší souvislost zbývajících $m - 1$ vrcholů, $T_{m-1}$ je tedy opět strom (a tudíž souvislý).
> 3. Tímto postupem dokážeme pro libovolné $k \in \{1, \dots, n\}$ zkonstruovat podmnožinu vrcholů $V_k \subseteq V$ o velikosti $|V_k| = k$ takovou, že podgraf $T[V_k]$ je souvislý strom.
> 4. Indukovaný podgraf $G[V_k]$ v původním grafu $G$ obsahuje všechny hrany podgrafu $T[V_k]$ (a případně i další hrany grafu $G$).
>    Protože $T[V_k]$ je souvislý a $E(T[V_k]) \subseteq E(G[V_k])$, je podgraf $G[V_k]$ nutně také souvislý. $\blacksquare$

---

## Úloha 6.5: Jednoznačný střed stromu

* **Zdroj:** FitWiki (6.11.2025)
* **Relevantní skripta:** [Definice 1.3 a 3.1 (Vzdálenost a cesty ve stromech)](https://truepeak78.github.io/ag1/01_Z%C3%A1klady_graf%C5%AF/1.1_Z%C3%A1kladn%C3%AD_pojmy/#def-1.3) · [Věta 3.3 (Jednoznačnost cest ve stromech)](https://truepeak78.github.io/ag1/03_Stromy/3.2_Charakterizace_strom%C5%AF/#theorem-3.3)

### Zadání
Nechť $T = (V, E)$ je strom a $u, v, c \in V$ jsou jeho vrcholy takové, že:
- $d_T(u, v) = 2r$ (pro nějaké celé číslo $r \ge 1$),
- $u$-$v$-cesta je nejdelší cestou v celém stromu $T$,
- $d_T(u, c) = r$ a vrchol $c$ leží na této $u$-$v$-cestě.

Dokažte, že $c$ je **jediný** vrchol v $T$, pro který platí, že vzdálenost od všech ostatních vrcholů stromu je nejvýše $r$ (tj. $\max_{w \in V} d_T(c, w) \le r$).

> [!success]- Řešení
> **Důkaz:**
> Označme $P = (u = x_0, \dots, x_r = c, \dots, x_{2r} = v)$ nejdelší cestu v $T$.
> Platí $d(u, c) = r$ a $d(c, v) = 2r - r = r$.
>
> 1. **Vrchol $c$ má vzdálenost k libovolnému vrcholu $\le r$:**
>    - Zvolme libovolný vrchol $w \in V$. Ve stromu existuje z $w$ do cesty $P$ jednoznačná cesta, která se s $P$ poprvé protne v nějakém vrcholu $x_i$ ($0 \le i \le 2r$).
>    - Vzdálenost z $w$ do konce $u$ je $d(w, x_i) + d(x_i, u) = d(w, x_i) + i$.
>    - Vzdálenost z $w$ do konce $v$ je $d(w, x_i) + d(x_i, v) = d(w, x_i) + (2r - i)$.
>    - Protože cesta $P$ je nejdelší cestou v celém stromu, žádná cesta nemůže mít délku větší než $2r$:
>      $$\max(d(w, u), d(w, v)) \le 2r \implies d(w, x_i) + \max(i, 2r - i) \le 2r$$
>    - Protože $\max(i, 2r - i) \ge r$, nutně platí:
>      $$d(w, x_i) \le 2r - \max(i, 2r - i) = \min(i, 2r - i)$$
>    - Vzdálenost z $w$ do středu $c = x_r$ je:
>      $$d(w, c) = d(w, x_i) + d(x_i, x_r) \le \min(i, 2r - i) + |r - i| = r.$$
>    Tedy pro každý vrchol $w \in V$ platí $d(c, w) \le r$.
>
> 2. **Jednoznačnost (žádný jiný vrchol tuto vlastnost nemá):**
>    - Zvolme libovolný jiný vrchol $c' \neq c$.
>    - Ve stromu je cesta mezi $u$ a $v$ procházející přes $c$ jediná. Vrchol $c'$ tedy nemůže ležet na obou úsecích současně.
>    - Pokud $c'$ leží na větvi blíže k $v$ (nebo $d(u, c') > d(u, c) = r$), pak:
>      $$d(c', u) = d(c', c) + d(c, u) = d(c', c) + r > r.$$
>    - Pokud $c'$ leží na větvi blíže k $u$ (nebo $d(v, c') > d(v, c) = r$), pak:
>      $$d(c', v) = d(c', c) + d(c, v) = d(c', c) + r > r.$$
>    - Pro každý vrchol $c' \neq c$ tedy existuje alespoň jeden z koncových vrcholů $u$ nebo $v$, jehož vzdálenost od $c'$ je ostře větší než $r$.
> Vrchol $c$ je proto jediným vrcholem s touto vlastností. $\blacksquare$


---

## Úloha 6.6: Rozpoznání grafu stromu s jednou přidanou hranou v $O(|V| + |E|)$
* **Zdroj:** FitWiki (Úterý 4.11.2025 `utery_2025_11_04`)
* **Relevantní skripta:** [Věta 2.1 (Vlastnosti stromů)](https://truepeak78.github.io/ag1/02_Souvislost_graf%C5%AF/2.2_Stromy/#theorem-2.1) · [Sekce 6.1 a 6.2 (BFS a DFS)](https://truepeak78.github.io/ag1/06_Z%C3%A1kladn%C3%AD_grafov%C3%A9_algoritmy/)

### Zadání
Navrhněte algoritmus, který pro zadaný neorientovaný jednoduchý graf $G = (V, E)$ určí, zda graf $G$ vznikl ze stromu přidáním právě jedné hrany (tzv. **unicyklický graf**, graf obsahující právě jednu jednoduchou kružnici).
Pro plný počet bodů je požadována asymptotická časová složitost $O(|V| + |E|)$. Dokažte korektnost, konečnost a časovou složitost.

> [!success]- Řešení
> ### 1. Teoretická charakterizace
> Nechť $T = (V, E_T)$ je strom na $|V| = n$ vrcholech.
> - Podle [Věty 2.1](https://truepeak78.github.io/ag1/02_Souvislost_graf%C5%AF/2.2_Stromy/#theorem-2.1) je $T$ souvislý a má přesně $|E_T| = n - 1$ hran.
> - Mezi libovolnými dvěma vrcholy $u, v \in V$ existuje ve stromu $T$ právě jedna jednoduchá cesta.
> - Přidáním nové hrany $e = \{u, v\} \notin E_T$ do stromu $T$ vznikne graf $G = (V, E_T \cup \{e\})$:
>   1. **Souvislost:** Protože $T$ byl souvislý a přidáním hrany se žádná cesta nepřeruší, graf $G$ je **souvislý**.
>   2. **Počet hran:** Počet hran grafu $G$ je přesně:
>      $$|E(G)| = |E_T| + 1 = (n - 1) + 1 = \mathbf{n = |V(G)|}.$$
>   3. **Právě jedna kružnice:** Nová hrana $e$ spojí konce jednoznačné jednoduché cesty v $T$, čímž vznikne právě jedna kružnice. Žádná další kružnice vzniknout nemůže.
> 
> **Ekvivalence:** Neorientovaný graf $G = (V, E)$ vznikl ze stromu přidáním právě jedné hrany právě tehdy, když:
> $$\mathbf{G \text{ je souvislý}} \quad \land \quad \mathbf{|E| = |V|}$$
> *(Důkaz zpětné implikace: Je-li $G$ souvislý a $|E| = |V|$, kostra $T$ má $|V|-1$ hran. Zbývá právě $1$ hrana mimo kostru, jejímž přidáním ke kostře vznikne $G$ s právě 1 kružnicí).*
> 
> ---
> 
> ### 2. Algoritmus (Pseudokód)
> ```text
> Algorithm IsTreePlusOneEdge(V, E):
>     // Rychlý test počtu hran:
>     if |E| != |V|:
>         return FALSE
>     if |V| < 3:
>         return FALSE      // Jednoduchý graf s |E|=|V| musí mít alespoň 3 vrcholy pro kružnici
>         
>     // Test souvislosti pomocí BFS:
>     Zvol libovolný vrchol s in V
>     Queue Q
>     Visited array initialized to FALSE
>     
>     Visited[s] = TRUE
>     Q.push(s)
>     visited_count = 1
>     
>     while Q is not empty:
>         u = Q.pop()
>         for each neighbor v of u:
>             if not Visited[v]:
>                 Visited[v] = TRUE
>                 visited_count = visited_count + 1
>                 Q.push(v)
>                 
>     // Graf je souvislý, pokud BFS navštívilo všech |V| vrcholů
>     return (visited_count == |V|)
> ```
> 
> ### 3. Důkaz korektnosti, konečnosti a složitosti
> - **Konečnost:** BFS navštíví každý vrchol nejvýše jednou díky poli `Visited`. Cyklus `while` proběhne nejvýše $|V|$-krát. Algoritmus se nezacyklí a vždy skončí.
> - **Korektnost:** 
>   - Pokud $|E| \neq |V|$, graf nemůže splňovat nutnou podmínku na počet hran, algoritmus ihned správně vrátí `FALSE`.
>   - Pokud $|E| = |V|$, zbývá ověřit souvislost. Algoritmus BFS z libovolného vrcholu $s$ navštíví celou komponentu obsahující $s$. Pokud `visited_count == |V|`, má graf právě jednu komponentu, je tedy souvislý. Z výše dokázané ekvivalence plyne, že $G$ je unicyklický graf.
> - **Časová složitost:**
>   - Test $|E| == |V|$ trvá $O(1)$.
>   - Inicializace a běh BFS navštíví $|V|$ vrcholů a projde $2|E|$ hran, tedy $O(|V| + |E|)$.
>   - Jelikož BFS spouštíme pouze tehdy, když $|E| = |V|$, je časová složitost:
>     $$T(|V|, |E|) = O(|V| + |V|) = \mathbf{O(|V|)} = \mathbf{O(|V| + |E|)}.$$
> - **Paměťová složitost:** $O(|V|)$ pro frontu a pole `Visited`.

---

## Úloha 6.7: Detekce kružnice v neorientovaném grafu v čase $O(|V|)$
* **Zdroj:** FitWiki (Pátek 7.11.2025 `patek_2025_11_07`, Hušek 2022 `husek_2022_11_30`, Hušek 2023)
* **Relevantní skripta:** [Definice 2.2 a Věta 2.1 (Acyklické grafy a lesy)](https://truepeak78.github.io/ag1/02_Souvislost_graf%C5%AF/2.2_Stromy/#theorem-2.1) · [Sekce 6.2 (DFS a detekce cyklů)](https://truepeak78.github.io/ag1/06_Z%C3%A1kladn%C3%AD_grafov%C3%A9_algoritmy/6.2_Prohled%C3%A1v%C3%A1n%C3%AD_do_hloubky/)

### Zadání
Navrhněte algoritmus, který pro obecný jednoduchý neorientovaný graf $G = (V, E)$ rozhodne, zda graf obsahuje alespoň jednu kružnici, přičemž algoritmus musí běžet v časové složitosti **$O(|V|)$** (tedy striktně nezávisle na počtu hran v hustém grafu). Dokažte správnost, konečnost a časovou složitost.

> [!success]- Řešení
> ### 1. Klíčové teoretické pozorování
> Nechť graf $G = (V, E)$ je acyklický (neobsahuje žádnou kružnici, tedy je lesem) a má $c \ge 1$ komponent souvislosti $C_1, \dots, C_c$.
> Každá komponenta $C_i$ je strom na $n_i = |V(C_i)|$ vrcholech, proto má přesně $n_i - 1$ hran.
> Celkový počet hran v lese je:
> $$|E| = \sum_{i=1}^c (n_i - 1) = \sum_{i=1}^c n_i - c = |V| - c \le |V| - 1$$
> 
> Odtud plyne základní věta:
> > **Jestliže v jednoduchém neorientovaném grafu $G = (V, E)$ platí $|E| \ge |V|$, potom graf $G$ NUTNĚ obsahuje alespoň jednu kružnici!**
> 
> ### 2. Algoritmus
> Díky tomuto pozorování nemusíme v hustých grafech (kde $|E| \ge |V|$) graf vůbec procházet!
> 
> ```text
> Algorithm DetectCycleFast(V, E):
>     // Krok 1: Okamžitý test horní meze na počet hran
>     if |E| >= |V|:
>         return TRUE    // Z Dirichletova principu / vlastností lesa graf zaručeně obsahuje kružnici!
>         
>     // Krok 2: Pokud |E| < |V|, graf je zaručeně řídký (|E| = O(|V|)).
>     // Spustíme standardní DFS s detekcí zpětných hran:
>     Visited array initialized to FALSE
>     Parent array initialized to null
>     
>     for each vertex s in V:
>         if not Visited[s]:
>             if DFS_Visit(s):
>                 return TRUE
>                 
>     return FALSE
> 
> Function DFS_Visit(u):
>     Visited[u] = TRUE
>     for each neighbor v of u:
>         if not Visited[v]:
>             Parent[v] = u
>             if DFS_Visit(v):
>                 return TRUE
>         else if v != Parent[u]:
>             // Narazili jsme na již navštívený vrchol, který není otcem -> nalezena kružnice!
>             return TRUE
>     return FALSE
> ```
> 
> ### 3. Důkaz správnosti a časové složitosti
> - **Správnost:**
>   - Pokud $|E| \ge |V|$, pak graf $G$ nemůže být lesem, neboť les na $|V|$ vrcholech má nejvýše $|V|-1$ hran. Tvrzení je matematicky korektní.
>   - Pokud $|E| < |V|$, algoritmus provede standardní DFS. V neorientovaném grafu existuje kružnice právě tehdy, když DFS narazí na hranu vedoucí do již dříve navštíveného vrcholu, který není bezprostředním rodičem aktuálního vrcholu v prohledávacím stromě.
> - **Časová složitost:**
>   - Krok 1 (porovnání celých čísel $|E| \ge |V|$) trvá $O(1)$.
>   - Pokud je podmínka splněna, algoritmus končí v čase $O(1)$.
>   - Pokud podmínka splněna není, platí $|E| < |V|$. Následné DFS tedy běží na grafu, kde $|E| = O(|V|)$.
>   - Časová složitost DFS na řídkém grafu je:
>     $$T(|V|, |E|) = O(|V| + |E|) = O(|V| + |V|) = \mathbf{O(|V|)}.$$
>   Ve všech případech je časová složitost shora omezena striktně $\mathbf{O(|V|)}$.
