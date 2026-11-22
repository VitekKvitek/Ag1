# 03. Teorie grafů: Stupně, regulární grafy, indukce a izomorfismus

Tento dokument obsahuje úlohy z písemek zaměřené na stupně vrcholů, princip sudosti, cesty a kružnice, izomorfismus grafů a doplňků a důkazy matematickou indukcí.

---

## Úloha 3.1: Právě dva vrcholy lichého stupně

* **Zdroj:** FitWiki (Hušek 2024, Sedláček 2020, Šestáková 2018)
* **Relevantní skripta:** [Věta 1.1 (Princip sudosti – Handshaking Lemma)](https://truepeak78.github.io/ag1/01_Z%C3%A1klady_graf%C5%AF/1.1_Z%C3%A1kladn%C3%AD_pojmy/#theorem-1.1) · [Definice 2.1 a 2.2 (Souvislost a komponenty)](https://truepeak78.github.io/ag1/02_Souvislost_graf%C5%AF/2.1_Souvislost_graf%C5%AF/#def-2.1) · [Definice 1.3 (Cesta a indukovaná cesta)](https://truepeak78.github.io/ag1/01_Z%C3%A1klady_graf%C5%AF/1.1_Z%C3%A1kladn%C3%AD_pojmy/#def-1.3)

### Zadání
Nechť $G = (V, E)$ je neorientovaný graf (ne nutně souvislý) a $u, v \in V$ ($u \neq v$) jsou jeho **jediné** dva vrcholy lichého stupně (všechny ostatní vrcholy v $G$ mají sudý stupeň).
1. Dokažte, že v $G$ existuje $u$-$v$-cesta (tedy $u$ a $v$ leží ve stejné souvislé komponentě).
2. *(Varianta z 3.11.2025):* Dokažte nebo vyvraťte: Mezi $u$ a $v$ existuje v $G$ **indukovaná** cesta.

> [!success]- Řešení
> ### 1. Důkaz existence cesty mezi $u$ a $v$
> Nechť $K_u$ je souvislá komponenta grafu $G$ obsahující vrchol $u$ ([Definice 2.2](https://truepeak78.github.io/ag1/02_Souvislost_graf%C5%AF/2.1_Souvislost_graf%C5%AF/#def-2.2)).
> Podgraf $K_u$ je sám o sobě platným grafem. Podle **Věty o principu sudosti** ([Věta 1.1](https://truepeak78.github.io/ag1/01_Z%C3%A1klady_graf%C5%AF/1.1_Z%C3%A1kladn%C3%AD_pojmy/#theorem-1.1)) platí:
> $$\sum_{x \in V(K_u)} \deg_{K_u}(x) = 2 |E(K_u)|$$
> Jelikož mezi komponentami nevedou žádné hrany, platí pro každý vrchol $x \in V(K_u)$, že $\deg_{K_u}(x) = \deg_G(x)$.
> Součet stupňů v $K_u$ je sudé číslo ($2|E(K_u)|$), proto musí komponenta $K_u$ obsahovat **sudý počet vrcholů lichého stupně**.
>
> V celém grafu $G$ existují právě dva vrcholy lichého stupně ($u$ a $v$). Vrchol $u$ leží v $K_u$ (lichý počet = 1). Aby byl počet vrcholů lichého stupně v $K_u$ sudý, **musí** v $K_u$ ležet i druhý lichý vrchol $v$.
> Protože $K_u$ je souvislý podgraf obsahující jak $u$, tak $v$, existuje mezi $u$ a $v$ z definice souvislosti cesta. $\blacksquare$
>
> ---
>
> ### 2. Existence indukované cesty
> **Tvrzení PLATÍ.**
> Z části 1 víme, že v $G$ existuje alespoň jedna $u$-$v$-cesta.
> Uvažujme ze všech existujících $u$-$v$-cest takovou cestu $P$, která má **minimální délku** (minimální počet hran).
>
> Sporem dokážeme, že tato nejkratší cesta $P = (u = x_0, x_1, \dots, x_k = v)$ je indukovaná:
> - Pokud by $P$ nebyla indukovaná, existovala by v $G$ hrana $e = \{x_i, x_j\}$ pro nějaké $j > i + 1$.
> - Pak bychom ale mohli úsek cesty mezi $x_i$ a $x_j$ nahradit touto jedinou hranou $\{x_i, x_j\}$, čímž bychom získali $u$-$v$-sled délky menší než $k$.
> - Ze zkráceného sledu lze vybrat $u$-$v$-cestu s délkou ostře menší než $k$, což je spor s minimalitou délky cesty $P$.
> Tedy nejkratší $u$-$v$-cesta je vždy indukovanou cestou. $\blacksquare$

---

## Úloha 3.2: Souvislost grafu o $2n$ vrcholech s minimálním stupněm $n$

* **Zdroj:** FitWiki (Knop 2023, Scholtzová 2020)
* **Relevantní skripta:** [Definice 1.2 (Stupeň vrcholu)](https://truepeak78.github.io/ag1/01_Z%C3%A1klady_graf%C5%AF/1.1_Z%C3%A1kladn%C3%AD_pojmy/#def-1.2) · [Definice 2.1 a 2.2 (Souvislost a komponenty)](https://truepeak78.github.io/ag1/02_Souvislost_graf%C5%AF/2.1_Souvislost_graf%C5%AF/#def-2.1)

### Zadání
Dokažte nebo vyvraťte:
*„Nechť $G$ je neorientovaný graf o $2n$ vrcholech ($n \ge 1$), ve kterém pro každý vrchol $v \in V(G)$ platí $\deg(v) \ge n$. Pak je $G$ souvislý.“*

> [!success]- Řešení
> **Tvrzení PLATÍ.**
>
> **Důkaz (sporem):**
> Předpokládejme pro spor, že graf $G$ o $|V(G)| = 2n$ vrcholech není souvislý.
> Pak $G$ obsahuje alespoň dvě disjunktní souvislé komponenty $K_1, K_2, \dots, K_c$ ($c \ge 2$).
> Vyberme komponentu s nejmenším počtem vrcholů, označme ji $K_{\min}$.
> Jelikož součet vrcholů všech $c \ge 2$ komponent je $2n$, pro velikost nejmenší komponenty nutně platí:
> $$|V(K_{\min})| \le \frac{2n}{2} = n.$$
> Zvolme libovolný vrchol $u \in V(K_{\min})$. Protože mezi různými komponentami nevedou žádné hrany, mohou všichni sousedé vrcholu $u$ ležet pouze uvnitř komponenty $K_{\min}$.
> V jednoduchém grafu bez smyček a násobných hran může mít vrchol $u$ nejvýše tolik sousedů, kolik je ostatních vrcholů v jeho komponentě:
> $$\deg(u) \le |V(K_{\min})| - 1 \le n - 1 < n.$$
> To je však v přímém **sporu** se zadáním, že pro každý vrchol platí $\deg(u) \ge n$.
> Předpoklad byl chybný, graf $G$ musí mít právě jednu komponentu, je tedy souvislý. $\blacksquare$

---

## Úloha 3.3: Existence trojúhelníku $K_3$ při vysokém stupni

* **Zdroj:** FitWiki (Mitura 2017)
* **Relevantní skripta:** [Definice 1.2 (Sousedé a stupeň vrcholu)](https://truepeak78.github.io/ag1/01_Z%C3%A1klady_graf%C5%AF/1.1_Z%C3%A1kladn%C3%AD_pojmy/#def-1.2) · [Definice 1.4 (Běžné grafy – úplný graf $K_n$)](https://truepeak78.github.io/ag1/01_Z%C3%A1klady_graf%C5%AF/1.3_B%C4%9B%C5%BEn%C3%A9_grafy/#def-1.4)

### Zadání
Nechť $G = (V, E)$ je graf na $n \ge 3$ vrcholech takový, že pro každý vrchol $v \in V$ platí:
$$\deg(v) > \frac{n}{2}$$
Dokažte, že graf $G$ obsahuje trojúhelník (tedy podgraf izomorfní $K_3$).

> [!success]- Řešení
> **Důkaz:**
> 1. Protože $\deg(v) > n/2 \ge 3/2 > 0$ pro každý vrchol, graf $G$ neobsahuje žádné izolované vrcholy a má alespoň jednu hranu. Zvolme libovolnou hranu $e = \{u, v\} \in E$.
> 2. Označme množiny sousedů vrcholů $u$ a $v$:
>    $$N(u) = \{x \in V \mid \{u, x\} \in E\}, \quad N(v) = \{y \in V \mid \{v, y\} \in E\}$$
>    Podle zadání platí $|N(u)| = \deg(u) > n/2$ a $|N(v)| = \deg(v) > n/2$.
> 3. Zkoumejme velikost průniku sousedů $N(u) \cap N(v)$ pomocí principu inkluze a exkluze.
>    Obě množiny $N(u)$ i $N(v)$ jsou podmnožinami množiny všech $n$ vrcholů $V$, tedy $|N(u) \cup N(v)| \le n$:
>    $$|N(u) \cap N(v)| = |N(u)| + |N(v)| - |N(u) \cup N(v)| > \frac{n}{2} + \frac{n}{2} - n = 0$$
> 4. Jelikož $|N(u) \cap N(v)|$ je celé číslo ostře větší než 0, platí:
>    $$|N(u) \cap N(v)| \ge 1$$
>    Existuje tedy alespoň jeden vrchol $w \in V$, který je současně sousedem $u$ i sousedem $v$ ($w \in N(u) \cap N(v)$).
> 5. Protože $u \notin N(u)$ a $v \notin N(v)$, vrchol $w$ je různý od $u$ i $v$. Vrcholy $\{u, v, w\}$ jsou vzájemně propojeny hranami $\{u, v\}, \{u, w\}, \{v, w\}$, a tvoří tudíž podgraf $K_3$. $\blacksquare$

---

## Úloha 3.4: Cesty v $k$-regulárním grafu

* **Zdroj:** FitWiki (Opravný 2021, Malík 2018)
* **Relevantní skripta:** [Definice 1.2 (Regulární graf)](https://truepeak78.github.io/ag1/01_Z%C3%A1klady_graf%C5%AF/1.1_Z%C3%A1kladn%C3%AD_pojmy/#def-1.2) · [Definice 1.3 (Cesta v grafu)](https://truepeak78.github.io/ag1/01_Z%C3%A1klady_graf%C5%AF/1.1_Z%C3%A1kladn%C3%AD_pojmy/#def-1.3)

### Zadání
Dokažte nebo vyvraťte:
*„V každém $k$-regulárním grafu $G$ ($k \ge 1$) existuje cesta délky alespoň $k$.“*

> [!success]- Řešení
> **Tvrzení PLATÍ.**
>
> **Důkaz (princip nejdelší cesty):**
> Graf $G$ je konečný, existuje v něm tedy cesta maximální délky. Označme ji:
> $$P = (v_0, v_1, v_2, \dots, v_m)$$
> kde $m$ je délka cesty (počet hran). Cesta obsahuje $m+1$ vrcholů.
>
> Uvažujme koncový vrchol cesty $v_m$:
> - Protože $G$ je $k$-regulární, platí $\deg(v_m) = k$.
> - Vrchol $v_m$ nemůže mít žádného souseda $w \notin V(P)$, protože jinak bychom mohli cestu $P$ prodloužit o hranu $\{v_m, w\}$ na délku $m+1$, což je spor s maximalitou cesty $P$.
> - Všech $k$ sousedů vrcholu $v_m$ proto musí ležet uvnitř množiny vrcholů cesty $P \setminus \{v_m\} = \{v_0, v_1, \dots, v_{m-1}\}$.
> - Množina $\{v_0, \dots, v_{m-1}\}$ má velikost $m$.
> Protože $v_m$ má $k$ různých sousedů v množině velikosti $m$, nutně platí:
> $$m \ge k.$$
> Nejdelší cesta má tedy délku alespoň $k$. $\blacksquare$

---

## Úloha 3.5: Souvislý $2$-regulární graf po odebrání vrcholu

* **Zdroj:** FitWiki (3.11.2025)
* **Relevantní skripta:** [Definice 1.4 (Běžné grafy – kružnice $C_n$ a cesty $P_n$)](https://truepeak78.github.io/ag1/01_Z%C3%A1klady_graf%C5%AF/1.3_B%C4%9B%C5%BEn%C3%A9_grafy/#def-1.4) · [Definice 2.1 (Souvislost grafu)](https://truepeak78.github.io/ag1/02_Souvislost_graf%C5%AF/2.1_Souvislost_graf%C5%AF/#def-2.1)

### Zadání
Rozhodněte a zdůvodněte, zda existuje $2$-regulární graf $G$ takový, že $G$ je souvislý, ale graf $G - v$ (pro nějaký vrchol $v \in V(G)$) je nesouvislý.

> [!success]- Řešení
> **Takový graf NEEXISTUJE (tvrzení neplatí).**
>
> **Důkaz:**
> 1. Každý konečný $2$-regulární graf je disjunktním sjednocením kružnic $C_k$ ($k \ge 3$).
> 2. Požadavek, aby $G$ byl souvislý, znamená, že $G$ musí tvořit právě **jedna jediná kružnice** $C_n$ o $n \ge 3$ vrcholech ($V = \{v_1, \dots, v_n\}$ s hranami $\{v_i, v_{i+1}\}$ a $\{v_n, v_1\}$).
> 3. Odebráním libovolného vrcholu $v$ z kružnice $C_n$ odebereme vrchol $v$ a obě jeho dvě incidentní hrany.
> 4. Výsledný graf $C_n - v$ je graf izomorfní cestě $P_{n-1}$ o $n-1$ vrcholech.
> 5. Cesta $P_{n-1}$ je pro každé $n \ge 3$ vždy **souvislým grafem**.
> Graf $G - v$ tedy nemůže být nikdy nesouvislý. $\blacksquare$

---

## Úloha 3.6: Lichá kružnice jako podgraf vs. indukovaný podgraf

* **Zdroj:** FitWiki (Hušek 2024, Knop 2021)
* **Relevantní skripta:** [Definice 1.3 a 1.4 (Kružnice a podgrafy)](https://truepeak78.github.io/ag1/01_Z%C3%A1klady_graf%C5%AF/1.3_B%C4%9B%C5%BEn%C3%A9_grafy/#def-1.4) · [Věta 1.3 (Bipartitní graf a liché kružnice)](https://truepeak78.github.io/ag1/01_Z%C3%A1klady_graf%C5%AF/1.3_B%C4%9B%C5%BEn%C3%A9_grafy/#theorem-1.3)

### Zadání
Dokažte následující větu:
*„Pokud graf $G$ obsahuje jako podgraf lichou kružnici, pak obsahuje jako indukovaný podgraf lichou kružnici.“*

> [!success]- Řešení
> **Důkaz:**
> Nechť $G$ obsahuje alespoň jednu lichou kružnici. Uvažujme ze všech lichých kružnic v $G$ kružnici $C$ s **minimální délkou** (minimálním počtem vrcholů $k$, kde $k \ge 3$ je liché).
> Označme vrcholy $C$ jako $(v_0, v_1, \dots, v_{k-1}, v_0)$.
>
> Ukážeme, že tato minimální lichá kružnice $C$ je indukovaným podgrafem grafu $G$:
> - Sporem: předpokládejme, že $C$ není indukovaná. Pak v $G$ existuje hrana (tětiva) $e = \{v_i, v_j\}$ mezi dvěma nesousedními vrcholy kružnice $C$ (tj. $j \not\equiv i \pm 1 \pmod k$).
> - Tato tětiva rozdělí kružnici $C$ na dvě menší kružnice $C_1$ a $C_2$, které sdílejí hranu $e$.
> - Nechť délka první kružnice je $l_1$ a druhé $l_2$. Součet jejich délek je $l_1 + l_2 = k + 2$ (společná hrana $e$ je započítána v obou).
> - Protože $k$ je liché, je součet $l_1 + l_2 = k + 2$ také liché číslo.
> - Součet dvou celých čísel je lichý právě tehdy, když je jedno z nich sudé a druhé **liché**.
> - Jedna z kružnic (např. $C_1$) je tedy nutně **lichá**.
> - Její délka však splňuje $l_1 < k$ (protože $e$ je tětiva nesousedních vrcholů, tedy $l_2 \ge 3 \implies l_1 = k + 2 - l_2 \le k - 1 < k$).
> - Našli jsme lichou kružnici $C_1$ o délce ostře menší než $k$, což je **spor** s předpokladem, že $C$ byla nejkratší lichá kružnice.
> Minimální lichá kružnice v $G$ proto nemá žádné tětivy a je indukovaným podgrafem. $\blacksquare$

---

## Úloha 3.7: Cesty izomorfní se svým doplňkem

* **Zdroj:** FitWiki (Knop 2022)
* **Relevantní skripta:** [Definice 2.5 (Izomorfismus grafů)](https://truepeak78.github.io/ag1/02_Souvislost_graf%C5%AF/2.3_Izomorfismus/#def-2.5) · [Definice 1.4 (Běžné grafy – cesta $P_n$ a doplněk grafu)](https://truepeak78.github.io/ag1/01_Z%C3%A1klady_graf%C5%AF/1.3_B%C4%9B%C5%BEn%C3%A9_grafy/#def-1.4)

### Zadání
Určete, které grafy typu cesta $P_n$ ($n \ge 1$) jsou izomorfní se svým vlastním doplňkem $\overline{P_n}$, a dokažte, že žádné jiné takové cesty neexistují.

> [!success]- Řešení
> **Výsledek:** Pouze cesty $P_1$ a $P_4$.
>
> **Důkaz:**
> 1. Cesta $P_n$ má $n$ vrcholů a $|E(P_n)| = n - 1$ hran.
> 2. Úplný graf $K_n$ má $\binom{n}{2} = \frac{n(n-1)}{2}$ hran.
> 3. Doplněk grafu $\overline{P_n}$ má stejnou množinu vrcholů a počet hran:
>    $$|E(\overline{P_n})| = \binom{n}{2} - |E(P_n)| = \frac{n(n-1)}{2} - (n - 1) = \frac{(n-1)(n-2)}{2}$$
> 4. Jsou-li dva grafy izomorfní ($P_n \cong \overline{P_n}$), musí mít **stejný počet hran**:
>    $$n - 1 = \frac{(n-1)(n-2)}{2}$$
>    Úpravou rovnice:
>    $$(n - 1) \left[ 1 - \frac{n-2}{2} \right] = 0 \iff (n - 1) \left( \frac{4 - n}{2} \right) = 0$$
> 5. Z této rovnosti plynou dvě řešení:
>    - **Případ $n = 1$:** $P_1$ má 1 vrchol a 0 hran, doplněk $\overline{P_1}$ má 1 vrchol a 0 hran. $P_1 \cong \overline{P_1}$ triviálně platí.
>    - **Případ $n = 4$:** $P_4$ má 4 vrcholy a 3 hrany. Doplněk má $\binom{4}{2} - 3 = 6 - 3 = 3$ hrany.
>      Nechť $P_4$ má hrany $\{1,2\}, \{2,3\}, \{3,4\}$. Hrany doplňku $\overline{P_4}$ jsou $\{1,3\}, \{1,4\}, \{2,4\}$. To tvoří cestu $2 - 4 - 1 - 3$, která je izomorfní $P_4$.
>    - Pro libovolné jiné $n \notin \{1, 4\}$ platí $|E(P_n)| \neq |E(\overline{P_n})|$, grafy tedy nemohou být izomorfní. $\blacksquare$

---

## Úloha 3.8: Automorfismy grafu ze spojených úplných podgrafů

* **Zdroj:** FitWiki (Šestáková 2018, Plachý 2018)
* **Relevantní skripta:** [Definice 2.5 (Automorfismus a izomorfismus)](https://truepeak78.github.io/ag1/02_Souvislost_graf%C5%AF/2.3_Izomorfismus/#def-2.5) · [Definice 1.4 (Úplný graf $K_n$)](https://truepeak78.github.io/ag1/01_Z%C3%A1klady_graf%C5%AF/1.3_B%C4%9B%C5%BEn%C3%A9_grafy/#def-1.4)

### Zadání
Mějme graf $G$ skládající se ze čtyř disjunktních úplných grafů $K_n^1, K_n^2, K_n^3, K_n^4$ (kde $n \ge 3$). Tyto podgrafy jsou vzájemně propojeny tak, že:
- mezi $K_n^1$ a $K_n^2$ vede právě 1 hrana,
- mezi $K_n^1$ a $K_n^3$ vede právě 1 hrana,
- mezi $K_n^1$ a $K_n^4$ vede právě 1 hrana,
přičemž tyto tři propojovací hrany vycházejí z **jednoho stejného vrcholu** $w \in V(K_n^1)$ a vedou do vrcholů $u_2 \in V(K_n^2)$, $u_3 \in V(K_n^3)$, $u_4 \in V(K_n^4)$. Žádné další hrany mezi bloky nevedou.
Určete počet automorfismů $|\text{Aut}(G)|$.

> [!success]- Řešení
> Automorfismus grafu je izomorfismus $f: V(G) \to V(G)$ grafu $G$ se sebou samým.
> Analyzujeme invarianty struktury:
> 1. **Role podgrafů:**
>    - Blok $K_n^1$ je centrální: je jediný spojen se 3 dalšími bloky. Libovolný automorfismus musí zobrazit $K_n^1$ na $K_n^1$.
>    - Bloky $K_n^2, K_n^3, K_n^4$ jsou navzájem izomorfní „listové“ bloky spojené s $K_n^1$. Lze je libovolně permutovat mezi sebou: existuje $3! = \mathbf{6}$ permutací těchto tří bloků.
> 2. **Fixace kontaktních vrcholů:**
>    - Vrchol $w \in K_n^1$ má stupeň uvnitř $K_n^1$ roven $n-1$ plus 3 vnější hrany, celkem $\deg(w) = n + 2$. Všechny ostatní vrcholy v $K_n^1$ mají stupeň $n-1$. Jelikož $n+2 > n-1$, vrchol $w$ je jednoznačně určen svým stupněm a musí se v každém automorfismu zobrazit sám na sebe ($f(w) = w$).
>    - Ostatních $n-1$ vrcholů v $K_n^1$ tvoří úplný graf a lze je navzájem libovolně permutovat: $(n - 1)!$ možností.
> 3. **Automorfismy v listových blocích:**
>    - Pro každý listový blok $K_n^i$ ($i \in \{2,3,4\}$) má kontaktní vrchol $u_i$ stupeň $(n-1) + 1 = n$, zatímco ostatní vrcholy v bloku mají stupeň $n-1$. Kontaktní vrchol $u_i$ se tedy při zobrazení bloku musí nutně zobrazit na kontaktní vrchol cílového bloku.
>    - Zbylých $n-1$ vrcholů uvnitř každého z těchto tří bloků lze libovolně permutovat: $[(n - 1)!]^3$ možností.
>
> **Celkový počet automorfismů:**
> $$|\text{Aut}(G)| = 3! \cdot (n - 1)! \cdot [(n - 1)!]^3 = \mathbf{6 \cdot [(n - 1)!]^4}$$


---

## Úloha 3.9: Počet komponent a mezní počty hran grafu a jeho doplňku
* **Zdroj:** FitWiki (Šestáková 2019, scan testu `test_ag1_sestakova_29_11_2019_18_00_1.jpg`)
* **Relevantní skripta:** [Definice 2.1 (Komponenty souvislosti)](https://truepeak78.github.io/ag1/02_Souvislost_graf%C5%AF/2.1_Souvislost_graf%C5%AF/#def-2.1) · [Definice 1.5 (Doplněk grafu)](https://truepeak78.github.io/ag1/01_Z%C3%A1klady_graf%C5%AF/1.4_Grafov%C3%A9_operace/#def-1.5)

### Zadání
Uvažujte neorientovaný jednoduchý graf $G = (V, E)$ s $|V| = 11$ vrcholy a $c = 5$ komponentami souvislosti.
1. Jaký je **minimální počet hran** grafu $G$? Svou odpověď zdůvodněte.
2. Jaký je **minimální počet hran** doplňku grafu $\overline{G}$? Svou odpověď zdůvodněte.

> [!success]- Řešení
> ### 1. Minimální počet hran grafu $G$
> Nechť komponenty souvislosti grafu $G$ jsou $C_1, C_2, \dots, C_5$, kde $|V(C_i)| = n_i \ge 1$ a $\sum_{i=1}^5 n_i = 11$.
> - V každé souvislé komponentě $C_i$ musí být počet hran alespoň $n_i - 1$ (rovnost nastává právě tehdy, když je komponenta stromem).
> - Sečteme počty hran přes všechny komponenty:
>   $$|E(G)| = \sum_{i=1}^5 |E(C_i)| \ge \sum_{i=1}^5 (n_i - 1) = \sum_{i=1}^5 n_i - \sum_{i=1}^5 1 = 11 - 5 = \mathbf{6}.$$
> - **Dosažitelnost minima:** Hodnoty 6 hran lze dosáhnout například volbou:
>   - 4 komponenty jsou izolované vrcholy $K_1$ (0 hran),
>   - 1 komponenta je strom na zbývajících $11 - 4 = 7$ vrcholech (6 hran).
>   Celkem $0 + 0 + 0 + 0 + 6 = 6$ hran.
> - Minimální počet hran grafu $G$ je **$6$**.
>
> ---
>
> ### 2. Minimální počet hran doplňku $\overline{G}$
> Počet hran v úplném grafu na 11 vrcholech je:
> $$|E(K_{11})| = \binom{11}{2} = \frac{11 \cdot 10}{2} = 55.$$
> 
> Počet hran v doplňku grafu $\overline{G}$ je dán vztahem:
> $$|E(\overline{G})| = |E(K_{11})| - |E(G)| = 55 - |E(G)|.$$
> 
> Abychom **minimalizovali** počet hran v doplňku $|E(\overline{G})|$, musíme **maximalizovat** počet hran v původním grafu $G$ za podmínky, že $G$ má 11 vrcholů a 5 komponent:
> $$|E(\overline{G})|_{\min} = 55 - |E(G)|_{\max}$$
> 
> #### Maximalizace počtu hran grafu s 11 vrcholy a 5 komponentami:
> - Každá komponenta $C_i$ může mít nanejvýš tolik hran, kolik má úplný graf na $n_i$ vrcholech, tedy $\binom{n_i}{2}$.
> - Chceme maximalizovat součet $\sum_{i=1}^5 \binom{n_i}{2}$ při podmínce $\sum_{i=1}^5 n_i = 11$ a $n_i \ge 1$.
> - Funkce $\binom{x}{2} = \frac{x(x-1)}{2}$ je ostře konvexní. Součet konvexních funkcí nabývá maxima v extrémním bodě simplexu — tedy když jsou 4 komponenty co nejmenší ($n_1 = n_2 = n_3 = n_4 = 1$) a zbývající 5. komponenta absorbuje všechny zbývající vrcholy:
>   $$n_5 = 11 - 4 \cdot 1 = 7.$$
> - V tomto případě má komponenta $C_5$ maximální počet hran jako úplný graf $K_7$:
>   $$|E(G)|_{\max} = \binom{7}{2} + 4 \cdot \binom{1}{2} = \frac{7 \cdot 6}{2} + 0 = \mathbf{21 \text{ hran}}.$$
> 
> #### Dosazení do vzorce pro doplněk:
> $$|E(\overline{G})|_{\min} = 55 - |E(G)|_{\max} = 55 - 21 = \mathbf{34 \text{ hran}}.$$
> 
> Minimální počet hran doplňku $\overline{G}$ je **$34$**.

---

## Úloha 3.10: Graf s méně hranami než vrcholy má vrchol stupně 0 nebo 1
* **Zdroj:** FitWiki (Baier 2016, scan testu `test-ag1-13-12-2016-baier.jpg`)
* **Relevantní skripta:** [Věta 1.1 (Princip sudosti a stupně)](https://truepeak78.github.io/ag1/01_Z%C3%A1klady_graf%C5%AF/1.1_Z%C3%A1kladn%C3%AD_pojmy/#theorem-1.1) · [Definice 2.2 (Stromy a acyklické grafy)](https://truepeak78.github.io/ag1/02_Souvislost_graf%C5%AF/2.2_Stromy/#def-2.2)

### Zadání
Je dán jednoduchý neorientovaný graf $G = (V, E)$ s alespoň dvěma vrcholy ($|V| \ge 2$) a s ostře méně hranami než vrcholy, tedy:
$$|E| < |V|$$
Dokažte, že graf $G$ nutně obsahuje alespoň jeden vrchol stupně 0 nebo 1 (tj. $\exists v \in V: \deg(v) \le 1$).

> [!success]- Řešení
> ### Důkaz (Sporem přes součet stupňů)
> 1. Nechť $n = |V| \ge 2$ a $m = |E|$. Ze zadání platí nerovnost:
>    $$m \le n - 1$$
> 
> 2. Předpokládejme pro spor, že graf $G$ **neobsahuje žádný vrchol stupně 0 ani 1**.
>    To znamená, že pro každý vrchol $v \in V$ platí:
>    $$\deg(v) \ge 2$$
> 
> 3. Sečteme stupně všech vrcholů grafu $G$:
>    $$\sum_{v \in V} \deg(v) \ge \sum_{v \in V} 2 = 2n$$
> 
> 4. Podle **Handshaking lemmatu** ([Věta 1.1](https://truepeak78.github.io/ag1/01_Z%C3%A1klady_graf%C5%AF/1.1_Z%C3%A1kladn%C3%AD_pojmy/#theorem-1.1)) je součet stupňů přesně roven dvojnásobku počtu hran:
>    $$\sum_{v \in V} \deg(v) = 2m$$
> 
> 5. Spojením obou vztahů dostáváme:
>    $$2m = \sum_{v \in V} \deg(v) \ge 2n \implies 2m \ge 2n \implies m \ge n$$
> 
> 6. Dostáváme nerovnost $m \ge n$, což je v přímém **SPORU** se zadáním $m < n$ (tj. $m \le n - 1$).
> 
> Předpoklad byl tedy nepravdivý a graf $G$ musí obsahovat alespoň jeden vrchol se stupněm $\le 1$, tedy vrchol stupně 0 (izolovaný vrchol) nebo stupně 1 (list).

---

## Úloha 3.11: Klasifikace neizomorfních grafů na 10 vrcholech se 41 hranami
* **Zdroj:** FitWiki (Suchý 2017, scan testu `ag1_zapoctak.jpeg`)
* **Relevantní skripta:** [Definice 1.2 a 1.5 (Izomorfismus a doplněk grafu)](https://truepeak78.github.io/ag1/01_Z%C3%A1klady_graf%C5%AF/1.2_Izomorfismus_graf%C5%AF/#def-1.2)

### Zadání
Uvažme všechny jednoduché neorientované grafy s 10 vrcholy a 41 hranami.
1. Kolik existuje navzájem **neizomorfních** grafů s těmito parametry?
2. Najděte všechny tyto neizomorfní grafy a vhodně je popište (např. pomocí jejich doplňků).

> [!success]- Řešení
> ### 1. Využití doplňku grafu
> Úplný graf na 10 vrcholech má celkem:
> $$|E(K_{10})| = \binom{10}{2} = \frac{10 \cdot 9}{2} = 45 \text{ hran.}$$
> 
> Nechť $G$ je libovolný graf s $|V(G)| = 10$ a $|E(G)| = 41$.
> Jeho doplněk $\overline{G}$ má stejných 10 vrcholů a jeho počet hran je:
> $$|E(\overline{G})| = 45 - 41 = \mathbf{4 \text{ hrany}}.$$
> 
> **Klíčová věta o izomorfismu doplňků:**
> Dva grafy $G_1$ a $G_2$ jsou izomorfní právě tehdy, když jsou izomorfní jejich doplňky:
> $$G_1 \cong G_2 \iff \overline{G_1} \cong \overline{G_2}$$
> 
> Hledání všech neizomorfních grafů na 10 vrcholech se 41 hranami je tedy **přesně ekvivalentní** hledání všech neizomorfních grafů na 10 vrcholech se **4 hranami**!
> 
> ---
> 
> ### 2. Klasifikace všech neizomorfních grafů se 4 hranami
> Graf se 4 hranami se skládá z neizolovaných hranových komponent a izolovaných vrcholů (kterých je $10 - k$, kde $k$ je počet neizolovaných vrcholů, přičemž $k \le 8 \le 10$).
> Možné neizomorfní struktury hran rozdělíme podle rozkladu čísla 4 na součty velikostí komponent a jejich topologie:
> 
> 1. **Jedna komponenta se 4 hranami:**
>    - **$C_4$:** Jednoduchá kružnice na 4 vrcholech ($+ 6$ izolovaných vrcholů).
>    - **$K_{1,4}$ (hvězda):** Jeden centrální vrchol stupně 4 a 4 listy ($+ 5$ izolovaných vrcholů).
>    - **$P_5$ (cesta):** Cesta o 4 hranách na 5 vrcholech ($+ 5$ izolovaných vrcholů).
>    - **Trojúhelník s přívěskem ($C_3 + e$):** Cyklus $C_3$ a jedna hrana vycházející z jednoho vrcholu cyklu do nového listu (4 vrcholy, $+ 6$ izolovaných).
>    - **Paw s rozdělenou hranou / $Y$-strom:** Vrchol stupně 3 spojený s cestou délky 2 a dvěma listy (5 vrcholů).
>    - **Úplný bipartitní graf $K_{3} \setminus \dots$ / dvojitý trojúhelník:** Dva trojúhelníky se společnou hranou mají 5 hran. Graf $K_3$ se 4 hranami neexistuje.
> 
> 2. **Dvě komponenty:**
>    - **$C_3 + P_2$:** Trojúhelník $K_3$ (3 hrany) a jedna nezávislá hrana $K_2$ ($+ 5$ izolovaných vrcholů).
>    - **$P_4 + P_2$:** Cesta délky 3 (3 hrany, 4 vrcholy) a jedna nezávislá hrana $K_2$ ($+ 4$ izolované vrcholy).
>    - **$K_{1,3} + P_2$:** Hvězda $K_{1,3}$ (3 hrany, 4 vrcholy) a nezávislá hrana $K_2$ ($+ 4$ izolované vrcholy).
>    - **$P_3 + P_3$:** Dvě disjunktní cesty délky 2 ($2 + 2 = 4$ hrany, $3 + 3 = 6$ vrcholů, $+ 4$ izolované vrcholy).
> 
> 3. **Tři komponenty:**
>    - **$P_3 + P_2 + P_2$:** Jedna cesta délky 2 a dvě nezávislé hrany ($2 + 1 + 1 = 4$ hrany, $3 + 2 + 2 = 7$ vrcholů, $+ 3$ izolované vrcholy).
> 
> 4. **Čtyři komponenty (párování):**
>    - **$4 P_2$ (nezávislé hrany):** Čtyři vzájemně disjunktní hrany (1-faktor na 8 vrcholech, $+ 2$ izolované vrcholy).
> 
> Celkem existuje přesně **11 neizomorfních grafů** se 4 hranami (a tedy právě **11 neizomorfních grafů** s 10 vrcholy a 41 hranami).
