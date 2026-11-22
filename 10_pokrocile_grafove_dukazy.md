# 10. Pokročilé grafové důkazy a vlastnosti – Zadání z písemek a řešení

Tento dokument obsahuje teoretické a důkazové úlohy z 2. zápočtového testu BI-AG1 zaměřené na indukované podgrafy ($P_3, P_4, C_4$), univerzální vrcholy, cesty v bipartitních grafech, souvislost přes redukované podgrafy $G - v$ a izomorfismus doplňků.

---

## Úloha 10.1: Grafy bez indukovaného $P_4$ a $C_4$ a existence univerzálního vrcholu

* **Zdroj:** FitWiki (Dvořák 2.12.2024, Knop 2023)
* **Relevantní skripta:** [Definice 1.3 (Cesta a indukovaný podgraf)](https://truepeak78.github.io/ag1/01_Z%C3%A1klady_graf%C5%AF/1.1_Z%C3%A1kladn%C3%AD_pojmy/#def-1.3) · [Definice 2.1 (Souvislost grafu)](https://truepeak78.github.io/ag1/02_Souvislost_graf%C5%AF/2.1_Souvislost_graf%C5%AF/#def-2.1)

### Zadání
Nechť $G = (V, E)$ je souvislý neorientovaný jednoduchý graf, který jako indukovaný podgraf neobsahuje cestu $P_4$ (cestu o 4 vrcholech) ani kružnici $C_4$ (kružnici o 4 vrcholech).
Dokažte, že v $G$ existuje vrchol $u \in V(G)$, který sousedí se **všemi ostatními vrcholy** grafu (tzv. univerzální vrchol, tj. $\deg(u) = |V(G)| - 1$).
*(Nápověda ze zadání: uvažujte vrchol s maximálním stupněm).*

> [!success]- Řešení
> **Důkaz:**
> 1. Zvolme ve stromu/grafu $G$ vrchol $u \in V(G)$ s **maximálním stupněm**:
>    $$\deg(u) = \Delta(G) = \max_{v \in V} \deg(v).$$
> 2. Označme $N(u) = \{x \in V \mid \{u, x\} \in E\}$ množinu všech sousedů vrcholu $u$.
>    Chceme dokázat, že $N(u) = V \setminus \{u\}$.
> 3. Předpokládejme pro spor, že $u$ nesousedí se všemi vrcholy, tedy množina nesousedů:
>    $$R = V \setminus (N(u) \cup \{u\}) \neq \emptyset.$$
> 4. Protože graf $G$ je souvislý, existuje mezi množinou $N(u)$ a množinou $R$ alespoň jedna hrana.
>    Vyberme vrchol $w \in R$, který má alespoň jednoho souseda v $N(u)$.
>    Nechť $v \in N(u)$ je soused vrcholu $w$ (tedy $\{v, w\} \in E$, přičemž $\{u, w\} \notin E$).
> 
> 5. Zkoumejme sousedy vrcholu $u$ a vrcholu $v$:
>    - Protože $\deg(u) \ge \deg(v)$, nemůže mít vrchol $v$ více sousedů než vrchol $u$.
>    - Vrchol $v$ sousedí s $w \in R$, ale $u$ s $w$ nesousedí.
>    - Aby mohl mít vrchol $u$ stupeň alespoň tak velký jako $v$, musí existovat nějaký vrchol $z \in N(u)$, který je sousedem $u$, ale **není sousedem $v$** (tedy $\{u, z\} \in E$, $\{v, z\} \notin E$).
> 
> 6. Nyní analyzujeme vztah mezi vrcholy $z$ a $w$:
>    - **Případ A ($\{z, w\} \notin E$):**
>      Podívejme se na čtveřici vrcholů $(w, v, u, z)$:
>      - Hrany přítomné v $G$: $\{w, v\} \in E$, $\{v, u\} \in E$, $\{u, z\} \in E$.
>      - Hrany chybějící v $G$: $\{w, u\} \notin E$ (z definice $R$), $\{v, z\} \notin E$ (z volby $z$), $\{w, z\} \notin E$ (předpoklad případu A).
>      - Tyto 4 vrcholy indukují právě cestu $w - v - u - z$, což je **indukovaná $P_4$**.
>      - To je v přímém **sporu** s předpokladem, že $G$ neobsahuje indukovanou $P_4$.
>    - **Případ B ($\{z, w\} \in E$):**
>      Podívejme se na čtveřici vrcholů $(w, v, u, z)$:
>      - Hrany přítomné v $G$: $\{w, v\} \in E$, $\{v, u\} \in E$, $\{u, z\} \in E$, $\{z, w\} \in E$.
>      - Hrany chybějící v $G$: $\{w, u\} \notin E$, $\{v, z\} \notin E$.
>      - Tyto 4 vrcholy indukují kružnici $w - v - u - z - w$ bez jakýchkoliv tětiv, což je **indukovaná $C_4$**.
>      - To je v přímém **sporu** s předpokladem, že $G$ neobsahuje indukovanou $C_4$.
> 
> V obou případech docházíme ke sporu. Množina nesousedů $R$ musí být prázdná ($R = \emptyset$), a tudíž vrchol $u$ s maximálním stupněm sousedí se všemi ostatními vrcholy grafu: $\deg(u) = |V(G)| - 1$. $\blacksquare$

---

## Úloha 10.2: Existence indukované cesty $P_3$ v neúplném souvislém grafu

* **Zdroj:** FitWiki (Dvořák 2.12.2024, Šestáková 2018)
* **Relevantní skripta:** [Definice 1.3 (Indukovaný podgraf a cesta)](https://truepeak78.github.io/ag1/01_Z%C3%A1klady_graf%C5%AF/1.1_Z%C3%A1kladn%C3%AD_pojmy/#def-1.3) · [Definice 1.4 (Úplný graf $K_n$)](https://truepeak78.github.io/ag1/01_Z%C3%A1klady_graf%C5%AF/1.3_B%C4%9B%C5%BEn%C3%A9_grafy/#def-1.4)

### Zadání
Dokažte následující tvrzení:
*„Nechť $G = (V, E)$ je souvislý neorientovaný jednoduchý graf o alespoň 3 vrcholech ($|V| \ge 3$), který **není úplným grafem**. Pak v $G$ existuje trojice navzájem různých vrcholů $u, v, w \in V$ taková, že $\{u, w\} \in E$, $\{v, w\} \in E$, ale $\{u, v\} \notin E$ (vrcholy $\{u, w, v\}$ tedy indukují cestu $P_3$).“*

> [!success]- Řešení
> **Důkaz:**
> 1. Protože graf $G$ není úplný, existují v $G$ alespoň dva různé vrcholy $a, b \in V$, mezi kterými nevede hrana:
>    $$\{a, b\} \notin E.$$
> 2. Graf $G$ je podle předpokladu souvislý, existuje tedy mezi $a$ a $b$ alespoň jedna cesta.
> 3. Zvolme nejkratší cestu mezi všemi dvojicemi nesousedních vrcholů v grafu $G$, nebo jednoduše nejkratší cestu mezi $a$ a $b$:
>    $$P = (a = x_0, x_1, x_2, \dots, x_k = b).$$
> 4. Jelikož $\{a, b\} \notin E$, délka této nejkratší cesty musí být alespoň 2 ($k \ge 2$).
> 5. Uvažujme první tři po sobě jdoucí vrcholy na této nejkratší cestě:
>    $$u = x_0 = a, \quad w = x_1, \quad v = x_2.$$
> 6. Ověříme hrany mezi touto trojicí:
>    - Hrany cesty $P$: $\{u, w\} = \{x_0, x_1\} \in E$ a $\{w, v\} = \{x_1, x_2\} \in E$.
>    - Dvojice $\{u, v\} = \{x_0, x_2\}$: Kdyby $\{u, v\} \in E$, pak bychom v cestě $P$ mohli nahradit úsek $(x_0, x_1, x_2)$ přímou hranou $\{x_0, x_2\}$, čímž bychom získali cestu mezi $a$ a $b$ délky $k - 1$. To je však ve sporu s tím, že $P$ byla nejkratší cestou mezi $a$ a $b$.
> 7. Platí tedy $\{u, v\} \notin E$.
> Trojice $(u, w, v)$ splňuje $\{u, w\} \in E, \{v, w\} \in E$ a $\{u, v\} \notin E$, což je přesně indukovaná cesta $P_3$. $\blacksquare$

---

## Úloha 10.3: Souvislost grafu a souvislost podgrafů $G - v$

* **Zdroj:** FitWiki (Suchý 2017, Malík 2018, Scholtzová 2020)
* **Relevantní skripta:** [Definice 2.1 a 2.2 (Souvislost a komponenty)](https://truepeak78.github.io/ag1/02_Souvislost_graf%C5%AF/2.1_Souvislost_graf%C5%AF/#def-2.1) · [Věta 3.2 a 3.4 (Kostra a listy stromu)](https://truepeak78.github.io/ag1/03_Stromy/3.1_Z%C3%A1kladn%C3%AD_pojmy/#theorem-3.2)

### Zadání
Dokažte větu:
*„Nechť $G = (V, E)$ je neorientovaný graf o $n \ge 3$ vrcholech. Graf $G$ je souvislý právě tehdy, když množina podgrafů $\{G - v \mid v \in V\}$ obsahuje alespoň dva souvislé grafy.“*

> [!success]- Řešení
> Dokazujeme ekvivalenci $\mathcal{A} \iff \mathcal{B}$:
> - $\mathcal{A}$: Graf $G$ je souvislý.
> - $\mathcal{B}$: V množině $\{G - v \mid v \in V\}$ existují alespoň dva souvislé grafy (existují alespoň dva různé vrcholy $u \neq w$ takové, že $G - u$ i $G - w$ jsou souvislé).
>
> ---
>
> ### 1. Směr $\implies$ (Je-li $G$ souvislý, existují alespoň dva vrcholy, jejichž odebráním zůstane graf souvislý):
> 1. Protože $G$ je souvislý graf na $n \ge 3$ vrcholech, existuje v $G$ kostra $T = (V, E_T)$ ([Věta 3.4](https://truepeak78.github.io/ag1/03_Stromy/3.3_Kostra_grafu/#theorem-3.4)).
> 2. Kostra $T$ je stromem s $n \ge 3$ vrcholy. Podle [Věty 3.2](https://truepeak78.github.io/ag1/03_Stromy/3.1_Z%C3%A1kladn%C3%AD_pojmy/#theorem-3.2) obsahuje každý strom s $n \ge 2$ vrcholy **alespoň dva různé listy** (vrcholy stupně 1).
> 3. Označme dva z těchto listů jako $u$ a $w$ ($u \neq w$).
> 4. Ve stromu $T$ má list $u$ stupeň 1 (je spojen právě s jedním sousedem $p$). Odebráním listu $u$ ze stromu $T$ zůstane strom $T - u$ souvislý (odstranili jsme koncový vrchol, cesty mezi všemi ostatními vrcholy neprocházely přes $u$).
> 5. Jelikož $T - u$ je podgrafem grafu $G - u$ a $T - u$ je souvislý a obsahuje všechny vrcholy $V \setminus \{u\}$, je podgraf $G - u$ **souvislý**.
> 6. Zcela stejná úvaha platí pro druhý list $w$: podgraf $T - w$ je souvislý, a tedy i $G - w$ je **souvislý**.
> Našli jsme dva různé vrcholy $u \neq w$, pro které jsou grafy $G - u$ a $G - w$ souvislé. Směr $\implies$ platí.
>
> ---
>
> ### 2. Směr $\impliedby$ (Existují-li dva různé vrcholy $u \neq w$ se souvislými $G - u$ a $G - w$, pak je $G$ souvislý):
> 1. Zvolme libovolné dva různé vrcholy $x, y \in V(G)$. Chceme dokázat, že v $G$ mezi nimi existuje cesta.
> 2. Protože $n \ge 3$ a máme dva různé vrcholy $u \neq w$:
>    - Pokud $u \notin \{x, y\}$, pak oba vrcholy $x$ i $y$ leží v podgrafu $G - u$. Protože $G - u$ je souvislý, existuje v $G - u$ (a tedy i v $G$) cesta mezi $x$ a $y$.
>    - Pokud jeden z vrcholů je $u$ (např. $x = u$), pak druhý vrchol $w \neq u$ splňuje $w \notin \{x, y\}$ (pokud $y \neq w$). Pak oba vrcholy $x$ a $y$ leží v podgrafu $G - w$, který je souvislý $\implies$ cesta existuje.
>    - Zbývá případ $\{x, y\} = \{u, w\}$: Protože $n \ge 3$, existuje v grafu třetí vrchol $z \notin \{u, w\}$.
>      - V souvislém grafu $G - w$ leží vrcholy $u$ i $z$, existuje tedy $u$-$z$-cesta $P_1$.
>      - V souvislém grafu $G - u$ leží vrcholy $z$ i $w$, existuje tedy $z$-$w$-cesta $P_2$.
>      - Spojením cest $P_1$ a $P_2$ získáme $u$-$w$-sled v $G$, z něhož lze vybrat jednoduchou $u$-$w$-cestu.
> Graf $G$ je tedy souvislý. $\blacksquare$

---

## Úloha 10.4: Počet cest dané délky v úplném bipartitním grafu $K_{n, 5}$

* **Zdroj:** FitWiki (Mitura 2017)
* **Relevantní skripta:** [Definice 1.3 (Cesta v grafu)](https://truepeak78.github.io/ag1/01_Z%C3%A1klady_graf%C5%AF/1.1_Z%C3%A1kladn%C3%AD_pojmy/#def-1.3) · [Definice 1.4 (Bipartitní graf $K_{n, m}$)](https://truepeak78.github.io/ag1/01_Z%C3%A1klady_graf%C5%AF/1.3_B%C4%9B%C5%BEn%C3%A9_grafy/#def-1.4)

### Zadání
Mějme úplný bipartitní graf $K_{n, 5}$ s partitami $A$ (velikost $n$) a $B$ (velikost 5), kde $n \ge 3$.
Zvolme dva libovolné **sousední vrcholy** $u \in A$ a $v \in B$ (tedy $\{u, v\} \in E$).
Určete celkový počet různých (jednoduchých) cest mezi $u$ a $v$ o délkách:
1. Délka 3
2. Délka 4
3. Délka 5

> [!success]- Řešení
> **Vlastnost bipartitního grafu:**
> Každá hrana spojuje vrchol z $A$ s vrcholem z $B$.
> Libovolná cesta začínající v $u \in A$ střídá partity:
> $$A \to B \to A \to B \to A \to B \dots$$
> Cesta délky $L$ obsahuje $L$ hran a $L + 1$ vrcholů.
>
> ---
>
> ### 1. Cesta délky 3 mezi $u \in A$ a $v \in B$:
> - Vrcholy na cestě: $(u = a_0, b_1, a_1, v = b_2)$, kde $a_0, a_1 \in A$ a $b_1, b_2 \in B$.
> - Cesta je jednoduchá, všechny její vrcholy musí být navzájem různé:
>   - Vrchol $b_1 \in B \setminus \{v\}$: vybíráme z $5 - 1 = \mathbf{4}$ zbývajících vrcholů v partitě $B$.
>   - Vrchol $a_1 \in A \setminus \{u\}$: vybíráme z $n - 1$ zbývajících vrcholů v partitě $A$.
> - V úplném bipartitním grafu vedou všechny hrany mezi partitami, takže každá taková volba dává platnou cestu.
> - **Celkový počet cest délky 3:**
>   $$N(L=3) = 4 \cdot (n - 1) = \mathbf{4n - 4}$$
>
> ---
>
> ### 2. Cesta délky 4 mezi $u \in A$ a $v \in B$:
> - Cesta délky 4 má 4 hrany a 5 vrcholů.
> - Posloupnost partit pro cestu délky 4 začínající v $A$:
>   $$A \to B \to A \to B \to A$$
> - Poslední vrchol cesty délky 4 musí nutně ležet v partitě $A$!
> - Cílový vrchol $v$ však leží v partitě $B$.
> - **V bipartitním grafu neexistuje žádná cesta sudé délky mezi vrcholy z různých partit.**
> - **Celkový počet cest délky 4:** $\mathbf{0}$.
>
> ---
>
> ### 3. Cesta délky 5 mezi $u \in A$ a $v \in B$:
> - Vrcholy na cestě: $(u = a_0, b_1, a_1, b_2, a_2, v = b_3)$.
> - Různé vrcholy:
>   - $a_0 = u$, $a_1, a_2 \in A \setminus \{u\}$: vybíráme uspořádanou dvojici 2 různých vrcholů z $n - 1$ zbývajících v partitě $A$:
>     $$(n - 1)(n - 2) \text{ možností}.$$
>   - $b_3 = v$, $b_1, b_2 \in B \setminus \{v\}$: vybíráme uspořádanou dvojici 2 různých vrcholů z $5 - 1 = 4$ zbývajících v partitě $B$:
>     $$4 \cdot 3 = 12 \text{ možností}.$$
> - Každá dvojice výběrů určuje jednoznačnou jednoduchou cestu $u - b_1 - a_1 - b_2 - a_2 - v$.
> - **Celkový počet cest délky 5:**
>   $$N(L=5) = 12 \cdot (n - 1)(n - 2) = \mathbf{12(n^2 - 3n + 2)}$$

---

## Úloha 10.5: Počet neizomorfních grafů na 10 vrcholech se 41 hranami

* **Zdroj:** FitWiki (Suchý 2017)
* **Relevantní skripta:** [Definice 1.4 (Doplněk grafu a úplný graf $K_n$)](https://truepeak78.github.io/ag1/01_Z%C3%A1klady_graf%C5%AF/1.3_B%C4%9B%C5%BEn%C3%A9_grafy/#def-1.4) · [Definice 2.5 (Izomorfismus grafů)](https://truepeak78.github.io/ag1/02_Souvislost_graf%C5%AF/2.3_Izomorfismus/#def-2.5)

### Zadání
Určete, kolik existuje vzájemně neizomorfních neorientovaných jednoduchých grafů na 10 vrcholech, které mají právě 41 hran.
Vysvětlete postup a popište strukturu všech těchto grafů.

> [!success]- Řešení
> ### 1. Princip duality přes doplněk grafu
> Úplný graf na $n = 10$ vrcholech má celkem:
> $$|E(K_{10})| = \binom{10}{2} = \frac{10 \cdot 9}{2} = 45 \text{ hran}.$$
> Má-li graf $G$ na 10 vrcholech právě 41 hran, pak jeho doplňkový graf $\overline{G} = (V, \binom{V}{2} \setminus E)$ má stejnou množinu 10 vrcholů a přesně:
> $$|E(\overline{G})| = 45 - 41 = \mathbf{4 \text{ hrany}}.$$
> 
> Platí základní věta teorie grafů:
> $$G_1 \cong G_2 \iff \overline{G_1} \cong \overline{G_2}.$$
> Počet neizomorfních grafů o 10 vrcholech se 41 hranami je tedy **přesně roven počtu neizomorfních grafů o 10 vrcholech se 4 hranami**.
> Izolované vrcholy (vrcholy stupně 0) nemají na izomorfismus žádný vliv – stačí nalézt všechny neizomorfní grafy se 4 hranami (a nejvýše 10 vrcholy) a zbylé vrcholy do 10 doplnit jako izolované.
>
> ---
>
> ### 2. Klasifikace všech neizomorfních grafů se 4 hranami:
> Hledáme všechny způsoby, jak uspořádat 4 hrany v jednoduchém neorientovaném grafu. Klasifikujeme je podle velikostí a typů souvislých komponent s alespoň jednou hranou:
>
> 1. **Komponenta je kružnice $C_4$:**
>    - Čtyři hrany tvoří kružnici na 4 vrcholech: $C_4$ (+ 6 izolovaných vrcholů).
> 2. **Kružnice $C_3$ a nezávislá hrana $K_2$:**
>    - Trojúhelník a jedna samostatná hrana: $K_3 \cup K_2$ (+ 5 izolovaných vrcholů).
> 3. **Kružnice $C_3$ s připojeným trnem (praporek):**
>    - Trojúhelník s jedním listem zavěšeným na libovolný vrchol: graf o 4 vrcholech (+ 6 izolovaných).
> 4. **Hvězda $K_{1, 4}$:**
>    - Jeden centrální vrchol stupně 4 spojený se 4 listy (+ 5 izolovaných).
> 5. **Cesta $P_5$:**
>    - Jednoduchá cesta o 4 hranách na 5 vrcholech: $P_5$ (+ 5 izolovaných).
> 6. **Cesta $P_4$ a nezávislá hrana $K_2$:**
>    - Cesta délky 3 (4 vrcholy) a jedna samostatná hrana: $P_4 \cup K_2$ (+ 4 izolované).
> 7. **Hvězda $K_{1, 3}$ a nezávislá hrana $K_2$:**
>    - Vrchol stupně 3 se 3 listy a jedna samostatná hrana: $K_{1, 3} \cup K_2$ (+ 4 izolované).
> 8. **Rozvětvený strom (Y-graf):**
>    - Cesta délky 3, kde na jeden vnitřní vrchol je napojen další list (centrální uzel stupně 3 s větvemi délek 2, 1, 1): strom o 5 vrcholech (+ 5 izolovaných).
> 9. **Cesta $P_3$ a dvě nezávislé hrany $K_2$:**
>    - Cesta o 2 hranách a dvě disjunktní hrany: $P_3 \cup K_2 \cup K_2$ (+ 3 izolované).
> 10. **Dvě nezávislé cesty $P_3$:**
>     - Dvě disjunktní cesty délky 2: $P_3 \cup P_3$ (+ 4 izolované).
> 11. **Čtyři nezávislé hrany (párování):**
>     - Čtyři disjunktní hrany: $4 K_2$ (+ 2 izolované).
>
> Žádné další uspořádání 4 hran bez smyček a násobných hran neexistuje.
>
> ### Výsledek:
> Existuje právě **11 vzájemně neizomorfních grafů** na 10 vrcholech se 41 hranami.

---

## Úloha 10.6: Existence kostry obsahující libovolně zvolenou hranu

* **Zdroj:** FitWiki (Písemka ZS 2019/2020, Písemka ZS 2020/2021 Knop, `stromecek.png`, `knop20201216_720.png`)
* **Relevantní skripta:** [Definice 3.3 a Věta 3.4 (Kostra grafu)](https://truepeak78.github.io/ag1/03_Stromy/3.3_Kostra_grafu/#def-3.3) · [Věta 3.3 (Ekvivalentní definice stromu)](https://truepeak78.github.io/ag1/03_Stromy/3.2_Charakterizace_strom%C5%AF/#theorem-3.3)

### Zadání
Nechť $G = (V, E)$ je souvislý neorientovaný graf s alespoň dvěma vrcholy ($|V| \ge 2$) a $e \in E$ je jeho libovolná hrana.
Dokažte, že v $G$ existuje kostra $T = (V, E_T)$ taková, že **$e \in E_T$** (tedy každá hrana souvislého grafu je obsažena v alespoň jedné kostře).

> [!success]- Řešení
> Uvedeme dva nezávislé a rigorózní důkazy:
>
> ### Důkaz 1: Přes kontrakci hrany
> 1. Označme hranu $e = \{u, v\}$.
> 2. Provedeme **kontrakci hrany $e$** v grafu $G$ (sloučíme vrcholy $u$ a $v$ do jediného vrcholu $w$ a odstraníme případnou vzniklou smyčku $e$). Výsledný graf označme $G / e$.
> 3. Protože graf $G$ byl souvislý, je i zkontrahovaný graf $G / e$ souvislý a má $|V| - 1$ vrcholů.
> 4. Podle [Věty 3.4](https://truepeak78.github.io/ag1/03_Stromy/3.3_Kostra_grafu/#theorem-3.4) má každý souvislý graf kostru. Nechť $T'$ je libovolná kostra grafu $G / e$. Kostra $T'$ je strom na $|V| - 1$ vrcholech, má tedy přesně $(|V| - 1) - 1 = |V| - 2$ hran a neobsahuje žádné cykly.
> 5. Nyní hranu $e$ vrátíme zpět: položíme $E_T = E(T') \cup \{e\}$.
> 6. Ověříme vlastnosti podgrafu $T = (V, E_T)$ v původním grafu $G$:
>    - $T$ má $|V|$ vrcholů.
>    - Počet hran je $|E_T| = (|V| - 2) + 1 = |V| - 1$.
>    - Graf $T$ je souvislý (cesty mezi všemi vrcholy v $T'$ zůstaly zachovány a vrcholy $u, v$ jsou propojeny přímou hranou $e$).
>    - Podle [Věty 3.3](https://truepeak78.github.io/ag1/03_Stromy/3.2_Charakterizace_strom%C5%AF/#theorem-3.3) je každý souvislý graf na $|V|$ vrcholech s $|V| - 1$ hranami stromem!
> 7. Graf $T$ je tedy kostrou grafu $G$ a z konstrukce platí $e \in E_T$. $\blacksquare$
>
> ---
>
> ### Důkaz 2: Pomocí Kruskalova algoritmu (váhový argument)
> 1. Definujme ohodnocení hran grafu $G$:
>    $$w(e) = -1, \quad w(e') = 1 \quad \text{pro všechny ostatní hrany } e' \in E \setminus \{e\}.$$
> 2. Spustíme Kruskalův algoritmus pro nalezení minimální kostry grafu $G$.
> 3. Kruskalův algoritmus nejprve seřadí hrany vzestupně podle váhy. Hrana $e$ má striktně nejmenší váhu $-1$, algoritmus ji tedy testuje jako první ze všech hran.
> 4. Jelikož v počátečním stavu je les tvořen pouze izolovanými vrcholy bez hran, hrana $e = \{u, v\}$ (pro $u \neq v$) nemůže vytvořit cyklus. Kruskalův algoritmus hranu $e$ **garantovaně zařadí do kostry**.
> 5. Po dokončení algoritmus vrátí kostru $T$, která nutně obsahuje hranu $e$. $\blacksquare$

---

## Úloha 10.7: Turnaj 26 týmů a existence grafu (Handshaking lemma)

* **Zdroj:** FitWiki (Písemka ZS 2019/2020, `stromecek.png`)
* **Relevantní skripta:** [Věta 1.1 (Handshaking lemma / Princip sudosti)](https://truepeak78.github.io/ag1/01_Z%C3%A1klady_graf%C5%AF/1.1_Z%C3%A1kladn%C3%AD_pojmy/#theorem-1.1) · [Definice 1.1](https://truepeak78.github.io/ag1/01_Z%C3%A1klady_graf%C5%AF/1.1_Z%C3%A1kladn%C3%AD_pojmy/#def-1.1)

### Zadání
Do sportovního turnaje se přihlásilo 26 týmů, které byly rozděleny do dvou skupin $A$ a $B$ po 13 týmech.
Pravidla stanoví, že každý tým má odehrát celkem **14 zápasů** tak, že:
- **11 zápasů** odehraje proti týmům ze své vlastní skupiny,
- zbylé **3 zápasy** odehraje proti týmům z opačné skupiny.

Rozhodněte, zda je možné podle těchto pravidel turnaj uspořádat. Své tvrzení rigorózně dokažte.

> [!success]- Řešení
> **Tvrzení:** Turnaj podle zadaných pravidel **není možné uspořádat**.
>
> **Důkaz:**
> 1. Reprezentujme zápasy uvnitř skupiny $A$ jako jednoduchý neorientovaný graf $G_A = (A, E_A)$, kde:
>    - vrcholy jsou týmy skupiny $A$, tedy $|A| = 13$,
>    - hrany představují vzájemné zápasy odehrané mezi dvěma týmy ze skupiny $A$.
> 2. Podle zadání musí každý tým ze skupiny $A$ odehrát právě 11 zápasů s jinými týmy ze skupiny $A$.
> 3. To znamená, že pro každý vrchol $v \in A$ musí v grafu $G_A$ platit:
>    $$\deg_{G_A}(v) = 11.$$
> 4. Sečtěme stupně všech vrcholů v grafu $G_A$:
>    $$\sum_{v \in A} \deg_{G_A}(v) = 13 \cdot 11 = \mathbf{143}.$$
> 5. Podle základní věty teorie grafů (**Handshaking lemma / Princip sudosti**, [Věta 1.1](https://truepeak78.github.io/ag1/01_Z%C3%A1klady_graf%C5%AF/1.1_Z%C3%A1kladn%C3%AD_pojmy/#theorem-1.1)) platí pro každý neorientovaný graf:
>    $$\sum_{v \in V} \deg(v) = 2 |E|.$$
>    Součet stupňů všech vrcholů musí být vždy **sudé číslo**, protože každá hrana přispívá k součtu stupňů právě hodnotou 2.
> 6. Hodnota $143$ je však **liché číslo**, což je v přímém sporu s větou o podání rukou.
> 
> Graf $G_A$ nemůže existovat, a proto není možné rozpis turnaje splňující zadaná pravidla sestavit. $\blacksquare$

---

## Úloha 10.8: Komise KTI a reprezentace grafem (Úplný graf $K_8$)

* **Zdroj:** FitWiki (Písemka ZS 2019/2020, `stromecek.png`)
* **Relevantní skripta:** [Definice 1.1 (Graf)](https://truepeak78.github.io/ag1/01_Z%C3%A1klady_graf%C5%AF/1.1_Z%C3%A1kladn%C3%AD_pojmy/#def-1.1) · [Definice 1.4 (Úplný graf $K_n$)](https://truepeak78.github.io/ag1/01_Z%C3%A1klady_graf%C5%AF/1.3_B%C4%9B%C5%BEn%C3%A9_grafy/#def-1.4)

### Zadání
Na Katedře teoretické informatiky (KTI) působí 8 různých odborných komisí. O jejich složení víme:
- Každé dvě různé komise mají **právě jednoho společného člena**.
- Každý zaměstnanec katedry je členem **právě dvou komisí**.

Kolik zaměstnanců má KTI? Převeďte úlohu na grafový model a odpověď zdůvodněte.

> [!success]- Řešení
> ### Grafový model:
> 1. Sestrojíme graf $G = (V, E)$, kde:
>    - **Množina vrcholů $V$:** Každý vrchol odpovídá jedné komisi. Máme 8 komisí $\implies |V| = 8$.
>    - **Množina hran $E$:** Každý zaměstnanec je členem právě 2 komisí. Zaměstnanec tedy jednoznačně určuje neuspořádanou dvojici komisí $\{K_i, K_j\}$, což přesně odpovídá neorientované hraně mezi vrcholy $K_i$ a $K_j$.
> 2. **Vlastnosti grafu ze zadání:**
>    - Každé dvě různé komise mají právě jednoho společného člena $\implies$ mezi každými dvěma různými vrcholy $u \neq v$ v grafu $G$ vede **právě jedna hrana**.
>    - Žádný zaměstnanec nesedí v jediné komisi (nejsou smyčky) ani ve více než dvou komisích (hrana spojuje právě 2 vrcholy).
> 3. Graf $G$ je tedy jednoduchý neorientovaný graf, ve kterém existuje hrana mezi každou dvojicí vrcholů.
>    Jedná se o **úplný graf na 8 vrcholech**:
>    $$G \cong K_8.$$
> 
> ### Výpočet počtu zaměstnanců:
> Počet zaměstnanců KTI přesně odpovídá počtu hran v grafu $K_8$:
> $$|E(K_8)| = \binom{8}{2} = \frac{8 \cdot 7}{2} = \mathbf{28}.$$
> 
> Katedra teoretické informatiky má právě **28 zaměstnanců**.


---

## Úloha 10.9: Cesty izomorfní se svým vlastním doplňkem
* **Zdroj:** FitWiki (Knop 2022 `knop_2022_12_01`)
* **Relevantní skripta:** [Definice 1.2 (Izomorfismus grafů)](https://truepeak78.github.io/ag1/01_Z%C3%A1klady_graf%C5%AF/1.2_Izomorfismus_graf%C5%AF/#def-1.2) · [Definice 1.5 (Doplněk grafu)](https://truepeak78.github.io/ag1/01_Z%C3%A1klady_graf%C5%AF/1.4_Grafov%C3%A9_operace/#def-1.5)

### Zadání
Rozhodněte a dokažte, které neorientované jednoduché cesty $P_n$ ($n \ge 1$, cesta na $n$ vrcholech) jsou **izomorfní se svým vlastním doplňkem** $\overline{P_n}$. Dokažte, že pro žádná jiná $n$ cesta izomorfní se svým doplňkem být nemůže.

> [!success]- Řešení
> ### 1. Nutná podmínka na počet hran
> Dva grafy mohou být izomorfní pouze tehdy, mají-li stejný počet vrcholů a stejný počet hran.
> - Cesta $P_n$ na $n$ vrcholech má přesně $|E(P_n)| = n - 1$ hran.
> - Úplný graf $K_n$ na $n$ vrcholech má celkem $\binom{n}{2} = \frac{n(n - 1)}{2}$ hran.
> - Počet hran v doplňku $\overline{P_n}$ je:
>   $$|E(\overline{P_n})| = |E(K_n)| - |E(P_n)| = \frac{n(n - 1)}{2} - (n - 1)$$
> 
> Má-li platit $P_n \cong \overline{P_n}$, musí se rovnat počty hran:
> $$|E(P_n)| = |E(\overline{P_n})| \implies n - 1 = \frac{n(n - 1)}{2} - (n - 1) \implies 2(n - 1) = \frac{n(n - 1)}{2}$$
> 
> Tuto rovnici vyřešíme:
> - **Pro $n = 1$:** Rovnice dává $2(0) = \frac{1 \cdot 0}{2} \implies 0 = 0$, což platí!
> - **Pro $n > 1$:** Můžeme obě strany rovnice vydělit nenulovým výrazem $(n - 1)$:
>   $$2 = \frac{n}{2} \implies \mathbf{n = 4}$$
> 
> Žádná jiná hodnota $n$ rovnici nevyhovuje. Cesta může být samodoplňková **pouze pro $n = 1$ a $n = 4$**.
>
> ---
>
> ### 2. Ověření nalezených případů
> 1. **Případ $n = 1$ ($P_1$):**
>    - $V(P_1) = \{v_1\}$, $E(P_1) = \emptyset$.
>    - Doplněk: $V(\overline{P_1}) = \{v_1\}$, $E(\overline{P_1}) = \emptyset$.
>    - Zjevně $P_1 \cong \overline{P_1}$ (triviální graf o 1 vrcholu).
>
> 2. **Případ $n = 4$ ($P_4$):**
>    - Nechť $V = \{1, 2, 3, 4\}$ a hrany cesty $P_4$ jsou:
>      $$E(P_4) = \{\{1, 2\}, \{2, 3\}, \{3, 4\}\}$$
>    - Úplný graf $K_4$ má $\binom{4}{2} = 6$ hran. Doplněk $\overline{P_4}$ obsahuje zbylé 3 hrany:
>      $$E(\overline{P_4}) = \{\{1, 3\}, \{1, 4\}, \{2, 4\}\}$$
>    - Prověříme strukturu $\overline{P_4}$:
>      - Stupně vrcholů v $\overline{P_4}$: $\deg(1) = 2$ (sousedi 3, 4), $\deg(2) = 1$ (soused 4), $\deg(3) = 1$ (soused 1), $\deg(4) = 2$ (sousedi 1, 2).
>      - Jedná se o cestu s koncovými vrcholy 2 a 3:
>        $$2 - 4 - 1 - 3$$
>      - Graf $\overline{P_4}$ je tedy jednoduchá cesta na 4 vrcholech!
>    - Zobrazení $f: V(P_4) \to V(\overline{P_4})$ definované jako $f(1)=2, f(2)=4, f(3)=1, f(4)=3$ je korektním izomorfismem. Platí $P_4 \cong \overline{P_4}$.
>
> ### Závěr
> Jediné cesty izomorfní se svým vlastním doplňkem jsou **$P_1$ a $P_4$**. Pro všechna ostatní $n \in \mathbb{N} \setminus \{1, 4\}$ nesouhlasí počet hran v $P_n$ a $\overline{P_n}$, proto žádné jiné samodoplňkové cesty neexistují.

---

## Úloha 10.10: Podgraf lichá kružnice implikuje indukovanou lichou kružnici
* **Zdroj:** FitWiki (Hušek 2024 `husek_2024_12_5`, Knop 2021 `knop2021`)
* **Relevantní skripta:** [Definice 1.3 (Podgraf a indukovaný podgraf)](https://truepeak78.github.io/ag1/01_Z%C3%A1klady_graf%C5%AF/1.1_Z%C3%A1kladn%C3%AD_pojmy/#def-1.3) · [Věta 3.1 (Bipartitní grafy a liché kružnice)](https://truepeak78.github.io/ag1/03_Bipartitn%C3%AD_grafy/3.1_Bipartitn%C3%AD_grafy/#theorem-3.1)

### Zadání
Dokažte následující tvrzení:
> Jestliže neorientovaný graf $G$ obsahuje jako (běžný) podgraf lichou kružnici, potom $G$ obsahuje jako **indukovaný podgraf** lichou kružnici.

> [!success]- Řešení
> ### Formální důkaz (princip minimálního protipříkladu / minimální délky)
> Mějme graf $G$, který obsahuje alespoň jednu lichou kružnici jako podgraf.
> Uvažujme množinu všech lichých kružnic, které jsou podgrafy v $G$:
> $$\mathcal{C}_{\text{odd}} = \{ C \subseteq G \mid C \text{ je kružnice a } |E(C)| \text{ je liché}\} \neq \emptyset$$
> 
> Protože délka kružnice je kladné celé číslo a množina je neprázdná, existuje v ní kružnice s **minimální délkou** (minimálním počtem hran). Označme tuto nejkratší lichou kružnici jako $C^*$.
> 
> Dokažme sporem, že $C^*$ je nutně **indukovaným** podgrafem grafu $G$:
> - Předpokládejme pro spor, že $C^*$ **není** indukovaný podgraf.
> - To z definice indukovaného podgrafu znamená, že v grafu $G$ existuje hrana $e = \{u, v\} \in E(G)$ spojující dva vrcholy $u, v \in V(C^*)$, která však **nepatří do kružnice** $C^*$ ($e \notin E(C^*)$). Takové hraně říkáme **tětiva** kružnice $C^*$.
> - Protože $e \notin E(C^*)$, vrcholy $u$ a $v$ nejsou na kružnici $C^*$ sousední (vzdálenost podél $C^*$ mezi nimi je alespoň 2 v obou směrech).
> - Vrcholy $u$ a $v$ rozdělují původní kružnici $C^*$ na dvě disjunktní cesty $P_1$ a $P_2$ mezi $u$ a $v$.
> - Délka původní kružnice je součet délek těchto cest:
>   $$|E(C^*)| = |E(P_1)| + |E(P_2)|$$
> - Přidáním tětivy $e$ k cestě $P_1$ vznikne kružnice $C_1 = P_1 + e$, a přidáním $e$ k cestě $P_2$ vznikne kružnice $C_2 = P_2 + e$.
> - Pro délky těchto dvou kružnic platí:
>   $$|E(C_1)| = |E(P_1)| + 1, \quad |E(C_2)| = |E(P_2)| + 1$$
> - Sečteme délky obou kružnic:
>   $$|E(C_1)| + |E(C_2)| = |E(P_1)| + |E(P_2)| + 2 = |E(C^*)| + 2$$
> - Protože $|E(C^*)|$ je **liché číslo**, výraz $|E(C^*)| + 2$ je rovněž **liché číslo**.
> - Součet dvou celých čísel $|E(C_1)| + |E(C_2)|$ je lichý právě tehdy, když **jedno z nich je liché a druhé je sudé**.
> - Bez újmy na obecnosti nechť $|E(C_1)|$ je liché číslo. Pak $C_1$ je lichá kružnice v grafu $G$, tedy $C_1 \in \mathcal{C}_{\text{odd}}$.
> - Porovnejme nyní délku $C_1$ s délkou $C^*$:
>   Protože $u$ a $v$ nejsou na $C^*$ sousední, cesta $P_2$ musí obsahovat alespoň 2 hrany ($|E(P_2)| \ge 2$). Odtud plyne:
>   $$|E(C_1)| = |E(C^*)| + 2 - |E(C_2)| = |E(C^*)| + 2 - (|E(P_2)| + 1) = |E(C^*)| + 1 - |E(P_2)| \le |E(C^*)| + 1 - 2 = |E(C^*)| - 1 < |E(C^*)|$$
> - Našli jsme lichou kružnici $C_1 \in \mathcal{C}_{\text{odd}}$, která má ostře menší délku než $C^*$ ($|E(C_1)| < |E(C^*)|$).
> - To je přímý **SPOR** s tím, že $C^*$ byla zvolena jako lichá kružnice minimální délky v $G$.
> 
> Proto kružnice $C^*$ nemůže mít v grafu $G$ žádnou tětivu. Každá hrana v $G$ mezi vrcholy z $V(C^*)$ tedy leží v $E(C^*)$, což znamená, že $C^*$ je **indukovaným podgrafem** grafu $G$.

---

## Úloha 10.11: Dva liché vrcholy leží v téže komponentě souvislosti
* **Zdroj:** FitWiki (Hušek 2024 `husek_2024_12_5`, Pondělí 2025 `pondeli_2025_11_03`)
* **Relevantní skripta:** [Věta 1.1 (Princip sudosti / Handshaking lemma)](https://truepeak78.github.io/ag1/01_Z%C3%A1klady_graf%C5%AF/1.1_Z%C3%A1kladn%C3%AD_pojmy/#theorem-1.1) · [Definice 2.1 (Komponenta souvislosti)](https://truepeak78.github.io/ag1/02_Souvislost_graf%C5%AF/2.1_Souvislost_graf%C5%AF/#def-2.1)

### Zadání
Nechť $G$ je neorientovaný graf, který má **právě dva vrcholy lichého stupně**, označme je $u$ a $v$.
Dokažte, že mezi vrcholy $u$ a $v$ existuje v grafu $G$ cesta (a tedy $u$ a $v$ nutně leží ve stejné komponentě souvislosti).

> [!success]- Řešení
> ### Důkaz přes Handshaking lemma v komponentách souvislosti
> 1. Nechť graf $G$ má komponenty souvislosti $C_1, C_2, \dots, C_k$ ($k \ge 1$), kde každá komponenta $C_i = (V_i, E_i)$ je souvislý podgraf a množiny vrcholů $V_1, \dots, V_k$ tvoří rozklad $V(G)$.
> 2. Pro každý vrchol $w \in V_i$ platí, že všechny jeho hrany vedou pouze do vrcholů téže komponenty $V_i$. Proto se stupeň vrcholu v komponentě rovná jeho stupni v celém grafu:
>    $$\deg_{C_i}(w) = \deg_G(w) \quad \forall w \in V_i$$
> 3. Podle **Handshaking lemmatu** ([Věta 1.1](https://truepeak78.github.io/ag1/01_Z%C3%A1klady_graf%C5%AF/1.1_Z%C3%A1kladn%C3%AD_pojmy/#theorem-1.1)) aplikovaného na samotnou komponentu $C_i$:
>    $$\sum_{w \in V_i} \deg_{C_i}(w) = 2 |E_i|$$
>    Protože pravá strana je sudé číslo, musí být i levá strana sudá. Z toho plyne základní vlastnost teorie grafů: **Každá komponenta souvislosti musí obsahovat SUDÝ počet vrcholů s lichým stupněm.**
> 4. Nyní předpokládejme pro spor, že vrcholy $u$ a $v$ **neleží ve stejné komponentě** souvislosti.
>    - Nechť bez újmy na obecnosti $u \in V_1$ a $v \in V_2$, kde $V_1 \cap V_2 = \emptyset$.
>    - Ze zadání víme, že $u$ a $v$ jsou *jediné dva* vrcholy lichého stupně v celém grafu $G$.
>    - To znamená, že v komponentě $C_1$ je $u$ jediným vrcholem s lichým stupněm a všechny ostatní vrcholy $w \in V_1 \setminus \{u\}$ mají sudý stupeň!
>    - Počet vrcholů lichého stupně v komponentě $C_1$ je tedy roven **1**.
>    - Číslo 1 je liché, což je přímý **SPOR** s tím, že každá komponenta musí mít sudý počet lichých vrcholů.
> 5. Vrcholy $u$ a $v$ proto nemohou ležet v různých komponentách souvislosti, nýbrž musí patřit do **téže komponenty** (ve které tvoří právě 2 liché vrcholy, což je sudé číslo).
> 6. Protože leží ve stejné komponentě souvislosti, z definice souvislého podgrafu mezi nimi **nutně existuje cesta** v grafu $G$.

---

## Úloha 10.12: Souvislost grafu o $2n$ vrcholech s minimálním stupněm $n$
* **Zdroj:** FitWiki (Knop 2023 `knop_2023_11_30`, Scholtzová 2020 `scholtzova2020`)
* **Relevantní skripta:** [Definice 2.1 (Souvislost grafu)](https://truepeak78.github.io/ag1/02_Souvislost_graf%C5%AF/2.1_Souvislost_graf%C5%AF/#def-2.1) · [Sekce 1.1 (Stupně vrcholů)](https://truepeak78.github.io/ag1/01_Z%C3%A1klady_graf%C5%AF/1.1_Z%C3%A1kladn%C3%AD_pojmy/#def-1.1)

### Zadání
Dokažte nebo vyvraťte:
> Každý jednoduchý neorientovaný graf $G$ o $2n$ vrcholech ($n \ge 1$), ve kterém má každý vrchol stupeň alespoň $n$ (tj. $\delta(G) \ge n$), je **souvislý**. Navíc ukažte, že průměr takového grafu je nejvýše 2 ($\text{diam}(G) \le 2$).

> [!success]- Řešení
> ### Výrok
> **Tvrzení PLATÍ.**
>
> ### 1. Důkaz souvislosti (sporem)
> Předpokládejme pro spor, že graf $G$ o $|V| = 2n$ vrcholech s minimálním stupněm $\delta(G) \ge n$ **není souvislý**.
> - Graf $G$ se tedy rozpadá na alespoň dvě komponenty souvislosti.
> - Nechť $C = (V_C, E_C)$ je komponenta souvislosti s **nejmenším počtem vrcholů**.
> - Protože komponent je alespoň 2 a celkový počet vrcholů je $2n$, pro velikost nejmenší komponenty nutně platí:
>   $$|V_C| \le \frac{|V(G)|}{2} = \frac{2n}{2} = n$$
> - Zvolme libovolný vrchol $x \in V_C$.
> - Protože $C$ je komponenta souvislosti, mezi vrcholy $V_C$ a zbytkem grafu $V(G) \setminus V_C$ nevede žádná hrana. Všichni sousedé vrcholu $x$ tedy musí ležet uvnitř $V_C$.
> - Množina vrcholů komponenty $C$ proto musí obsahovat samotný vrchol $x$ a všech jeho alespoň $n$ sousedů:
>   $$|V_C| \ge \deg_G(x) + 1 \ge \delta(G) + 1 \ge n + 1$$
> - Dostáváme nerovnost:
>   $$n + 1 \le |V_C| \le n \implies n + 1 \le n$$
> - To je triviální **SPOR** ($1 \le 0$).
> Graf $G$ tedy nemůže mít více než jednu komponentu a je **souvislý**.
>
> ---
>
> ### 2. Důkaz, že průměr grafu $\text{diam}(G) \le 2$
> Zvolme libovolnou dvojici různých vrcholů $u, v \in V(G)$.
> - **Pokud $\{u, v\} \in E(G)$:** Vzdálenost $d(u, v) = 1 \le 2$.
> - **Pokud $\{u, v\} \notin E(G)$:**
>   - Vrcholy $u$ a $v$ nejsou spojeny hranou.
>   - Otevřená sousedství $N(u)$ i $N(v)$ leží v množině zbývajících vrcholů:
>     $$N(u) \subseteq V(G) \setminus \{u, v\}, \quad N(v) \subseteq V(G) \setminus \{u, v\}$$
>   - Počet zbývajících vrcholů v grafu je $|V(G) \setminus \{u, v\}| = 2n - 2$.
>   - Ze zadání platí $|N(u)| = \deg(u) \ge n$ a $|N(v)| = \deg(v) \ge n$.
>   - Podle principu inkluze a exkluze:
>     $$|N(u) \cap N(v)| = |N(u)| + |N(v)| - |N(u) \cup N(v)|$$
>   - Protože $N(u) \cup N(v) \subseteq V(G) \setminus \{u, v\}$, je $|N(u) \cup N(v)| \le 2n - 2$.
>   - Dosazením dostáváme:
>     $$|N(u) \cap N(v)| \ge n + n - (2n - 2) = 2n - 2n + 2 = \mathbf{2}$$
>   - Vrcholy $u$ a $v$ mají dokonce **alespoň dva společné sousedy**!
>   - Nechť $w \in N(u) \cap N(v)$ je libovolný z nich. Pak sled $u - w - v$ je cestou délky 2.
>   - Vzdálenost $d(u, v) \le 2$.
>
> Protože pro každou dvojici vrcholů platí $d(u, v) \le 2$, průměr grafu $\text{diam}(G) = \max_{u,v} d(u, v) \le \mathbf{2}$.

---

## Úloha 10.13: Existence trojúhelníku v hustém grafu s $\deg(v) > n/2$
* **Zdroj:** FitWiki (Mitura 2017 `512171800mitura`)
* **Relevantní skripta:** [Definice 1.4 (Běžné grafy – Trojúhelník $K_3$)](https://truepeak78.github.io/ag1/01_Z%C3%A1klady_graf%C5%AF/1.3_B%C4%9B%C5%BEn%C3%A9_grafy/#def-1.4) · [Věta 3.1 (Bipartitnost a trojúhelníky)](https://truepeak78.github.io/ag1/03_Bipartitn%C3%AD_grafy/3.1_Bipartitn%C3%AD_grafy/)

### Zadání
Nechť $G = (V, E)$ je jednoduchý neorientovaný graf na $n \ge 3$ vrcholech takový, že pro každý vrchol $v \in V(G)$ platí:
$$\deg(v) > \frac{n}{2}$$
Dokažte, že graf $G$ obsahuje trojúhelník (tj. podgraf izomorfní s úplným grafem $K_3$).

> [!success]- Řešení
> ### Důkaz (Princip holubníku na sousedství libovolné hrany)
> 1. **Existence hrany:**
>    Protože $n \ge 3$, pro každý vrchol platí $\deg(v) > \frac{n}{2} \ge 1{,}5 \implies \deg(v) \ge 2$. Graf tedy není prázdný a obsahuje alespoň jednu hranu.
> 2. Zvolme libovolnou hranu $e = \{u, v\} \in E(G)$.
> 3. Vrcholy $u$ a $v$ mají v grafu $G$ své množiny sousedů $N(u)$ a $N(v)$.
>    - Protože $\{u, v\} \in E(G)$, vrchol $v$ je sousedem $u$ a vrchol $u$ je sousedem $v$.
>    - Množina sousedů vrcholu $u$ mimo vrchol $v$ má velikost:
>      $$|N(u) \setminus \{v\}| = \deg(u) - 1 > \frac{n}{2} - 1$$
>    - Množina sousedů vrcholu $v$ mimo vrchol $u$ má velikost:
>      $$|N(v) \setminus \{u\}| = \deg(v) - 1 > \frac{n}{2} - 1$$
> 4. Všechny tyto sousedy musí ležet v množině zbývajících $n - 2$ vrcholů grafu, tedy v univerzu $U = V(G) \setminus \{u, v\}$, jehož mohutnost je $|U| = n - 2$.
> 5. Sečteme mohutnosti obou množin sousedů:
>    $$|N(u) \setminus \{v\}| + |N(v) \setminus \{u\}| > \left(\frac{n}{2} - 1\right) + \left(\frac{n}{2} - 1\right) = n - 2 = |U|$$
> 6. Podle **principu inkluze a exkluze** (resp. Dirichletova principu holubníku):
>    $$|(N(u) \setminus \{v\}) \cap (N(v) \setminus \{u\})| = |N(u) \setminus \{v\}| + |N(v) \setminus \{u\}| - |(N(u) \setminus \{v\}) \cup (N(v) \setminus \{u\})|$$
>    Protože sjednocení je podmnožinou $U$, je jeho velikost nejvýše $|U| = n - 2$:
>    $$|(N(u) \setminus \{v\}) \cap (N(v) \setminus \{u\})| > (n - 2) - (n - 2) = 0$$
> 7. Průnik těchto dvou množin je tedy **ostře větší než 0**, což znamená, že v něm existuje alespoň jeden vrchol $w \in U$.
> 8. Pro tento vrchol $w$ platí:
>    - $w \in N(u) \implies \{u, w\} \in E(G)$,
>    - $w \in N(v) \implies \{v, w\} \in E(G)$,
>    - a ze zadání $\{u, v\} \in E(G)$.
> 9. Vrcholy $\{u, v, w\}$ jsou po dvou spojeny hranami, tvoří tedy podgraf $K_3$ (trojúhelník) v grafu $G$.

---

## Úloha 10.14: Hellyho vlastnost pro podstromy stromu
* **Zdroj:** FitWiki (Dvořák 2024 `dvorak_2024_12_2`)
* **Relevantní skripta:** [Definice 2.2 (Strom)](https://truepeak78.github.io/ag1/02_Souvislost_graf%C5%AF/2.2_Stromy/#def-2.2) · [Věta 2.1 (Vlastnosti stromů)](https://truepeak78.github.io/ag1/02_Souvislost_graf%C5%AF/2.2_Stromy/#theorem-2.1)

### Zadání
Mějme strom $T = (V, E)$ a tři jeho neprázdné souvislé podgrafy (podstromy) $T_1, T_2, T_3$.
Předpokládejme, že každé dva z těchto tří podstromů mají alespoň jeden společný vrchol:
$$V(T_1) \cap V(T_2) \neq \emptyset, \quad V(T_2) \cap V(T_3) \neq \emptyset, \quad V(T_1) \cap V(T_3) \neq \emptyset$$
Dokažte, že existuje vrchol, který je společný všem třem podstromům, tedy:
$$V(T_1) \cap V(T_2) \cap V(T_3) \neq \emptyset$$

> [!success]- Řešení
> ### Důkaz (Přes jednoznačnost cest ve stromě)
> 1. Ze zadání vyberme libovolné tři vrcholy reprezentující párové průniky:
>    - Zvolme $x \in V(T_1) \cap V(T_2)$,
>    - Zvolme $y \in V(T_2) \cap V(T_3)$,
>    - Zvolme $z \in V(T_1) \cap V(T_3)$.
> 
> 2. Pokud se některé dva z těchto vrcholů rovnají (např. $x = y$), pak $x \in V(T_1) \cap V(T_2)$ a zároveň $x \in V(T_3)$, takže $x \in V(T_1) \cap V(T_2) \cap V(T_3)$ a tvrzení triviálně platí.
>    Předpokládejme tedy dále, že $x, y, z$ jsou tři vzájemně různé vrcholy.
> 
> 3. Protože $T$ je strom, mezi libovolnými dvěma vrcholy existuje **právě jedna jednoduchá cesta** ([Věta 2.1](https://truepeak78.github.io/ag1/02_Souvislost_graf%C5%AF/2.2_Stromy/#theorem-2.1)).
>    Označme $P(x, y)$, $P(y, z)$ a $P(x, z)$ tyto jednoznačné jednoduché cesty v $T$.
> 
> 4. **Příslušnost cest k podstromům:**
>    - Vrcholy $x$ i $y$ leží v $V(T_2)$. Protože $T_2$ je souvislý podgraf stromu $T$, celá jednoznačná cesta mezi nimi musí ležet uvnitř $T_2$:
>      $$P(x, y) \subseteq T_2$$
>    - Analogicky:
>      - $y, z \in V(T_3) \implies P(y, z) \subseteq T_3$
>      - $x, z \in V(T_1) \implies P(x, z) \subseteq T_1$
> 
> 5. **Průnik tří cest ve stromě (Medián trojice vrcholů):**
>    Ve stromě pro libovolné tři vrcholy $x, y, z$ existuje právě jeden centrální vrchol $m$ (tzv. *medián*), ve kterém se cesty $P(x, y), P(y, z), P(x, z)$ protínají.
>    Konstrukce $m$: Začněme ve vrcholu $x$ a postupujme po cestě $P(x, y)$ směrem k $y$. Nechť $m$ je poslední vrchol na této cestě, který zároveň leží na cestě $P(x, z)$.
>    Z definice stromu (absence kružnic) se cesta $P(x, z)$ od cesty $P(x, y)$ odpojuje právě v tomto vrcholu $m$, a od vrcholu $m$ do $z$ a z $m$ do $y$ vedou disjunktní cesty. Tedy jednoznačná cesta mezi $y$ a $z$ vede právě přes vrchol $m$, což znamená:
>    $$m \in P(x, y) \cap P(y, z) \cap P(x, z)$$
> 
> 6. **Společný vrchol všech tří podstromů:**
>    Jelikož $m \in P(x, y) \subseteq T_2$, platí $m \in V(T_2)$.
>    Jelikož $m \in P(y, z) \subseteq T_3$, platí $m \in V(T_3)$.
>    Jelikož $m \in P(x, z) \subseteq T_1$, platí $m \in V(T_1)$.
> 
> Odtud přímo plyne:
> $$m \in V(T_1) \cap V(T_2) \cap V(T_3) \neq \emptyset$$
> Vrchol $m$ leží ve všech třech podstromech současně, čímž je Hellyho vlastnost dokázána.

---

## Úloha 10.15: Červený list v bipartitně obarveném stromu
* **Zdroj:** FitWiki (Šestáková 2019 `sest2019`)
* **Relevantní skripta:** [Definice 2.2 (Strom a listy)](https://truepeak78.github.io/ag1/02_Souvislost_graf%C5%AF/2.2_Stromy/#def-2.2) · [Věta 3.1 (Bipartitní grafy)](https://truepeak78.github.io/ag1/03_Bipartitn%C3%AD_grafy/3.1_Bipartitn%C3%AD_grafy/#theorem-3.1)

### Zadání
Mějme neorientovaný graf $T = (V, E)$, který je stromem na alespoň dvou vrcholech ($|V| \ge 2$).
Vrcholy $T$ jsou obarveny červenou barvou ($R$) nebo modrou barvou ($B$) tak, že žádné dva sousední vrcholy nemají stejnou barvu (korektní 2-obarvení / bipartitní rozklad $V = R \cup B$).
Dokažte následující tvrzení:
> Pokud strom $T$ obsahuje alespoň tolik červených vrcholů jako modrých (tj. $|R| \ge |B|$), potom v $T$ **existuje alespoň jeden červený list** (vrchol stupně 1 s červenou barvou).

> [!success]- Řešení
> ### Důkaz (Sporem přes součet stupňů v bipartitním grafu)
> 1. Nechť $T = (V, E)$ je strom s bipartitním obarvením $V = R \cup B$, kde $R \cap B = \emptyset$ a $|R| \ge |B|$.
> 2. Protože $T$ je strom s $|V| \ge 2$, celkový počet hran je $|E| = |V| - 1 = |R| + |B| - 1$.
> 3. Z vlastnosti bipartitního grafu každá hrana spojuje právě jeden červený a jeden modrý vrchol. Součet stupňů v každé z obou partit se tedy přesně rovná počtu hran:
>    $$\sum_{r \in R} \deg(r) = |E| = \sum_{b \in B} \deg(b)$$
> 4. Předpokládejme pro spor, že v $T$ **neexistuje žádný červený list**.
>    - To znamená, že žádný vrchol z $R$ nemá stupeň 1.
>    - Protože strom je souvislý a $|V| \ge 2$, neobsahuje žádné izolované vrcholy ($\deg(v) \ge 1$ pro všechna $v$).
>    - Všechny vrcholy v $R$ tedy musí mít stupeň alespoň 2:
>      $$\deg(r) \ge 2 \quad \forall r \in R$$
> 5. Odhadneme součet stupňů v $R$:
>    $$|E| = \sum_{r \in R} \deg(r) \ge 2 |R|$$
> 6. Nyní využijeme předpoklad zadání $|R| \ge |B|$:
>    $$|E| \ge 2 |R| \ge |R| + |B|$$
> 7. Porovnáme to se vzorcem pro počet hran ve stromě $|E| = |R| + |B| - 1$:
>    $$|R| + |B| - 1 = |E| \ge |R| + |B| \implies -1 \ge 0$$
> 8. Dostáváme přímý aritmetický **SPOR**!
> 
> Předpoklad byl tedy chybný a v množině $R$ musí existovat alespoň jeden vrchol se stupněm menším než 2, tedy vrchol stupně 1 — **červený list**.

---

## Úloha 10.16: Komplement podgrafu bez vrcholu: $\overline{G - v} = \overline{G} - v$
* **Zdroj:** FitWiki (Šestáková 2019, scan testu `test_ag1_sestakova_29_11_2019_18_00_4.jpg`)
* **Relevantní skripta:** [Definice 1.5 (Operace s grafy – podgrafy a doplňky)](https://truepeak78.github.io/ag1/01_Z%C3%A1klady_graf%C5%AF/1.4_Grafov%C3%A9_operace/#def-1.5)

### Zadání
Mějme jednoduchý neorientovaný graf $G = (V, E)$, libovolný vrchol $v \in V$ a podgraf $H = G - v$ vzniklý odstraněním vrcholu $v$ a všech hran do něj incidentních.
Dokažte, že platí:
$$\overline{H} = \overline{G} - v$$
kde $\overline{K}$ značí doplněk grafu $K$ do úplného grafu na jeho množině vrcholů.

> [!success]- Řešení
> ### Důkaz (Množinová rovnost grafů)
> Dva grafy $G_1 = (V_1, E_1)$ a $G_2 = (V_2, E_2)$ jsou totožné ($G_1 = G_2$), právě když se rovnají jejich množiny vrcholů ($V_1 = V_2$) a množiny hran ($E_1 = E_2$).
>
> #### 1. Rovnost množin vrcholů:
> - Z definice odstranění vrcholu $v$ z grafu $G$:
>   $$V(H) = V(G - v) = V(G) \setminus \{v\}$$
> - Doplněk grafu $\overline{H}$ má podle definice doplňku stejnou množinu vrcholů jako výchozí graf $H$:
>   $$V(\overline{H}) = V(H) = V(G) \setminus \{v\}$$
> - Na pravé straně rovnosti:
>   - Doplněk $\overline{G}$ má množinu vrcholů $V(\overline{G}) = V(G)$.
>   - Odstraněním vrcholu $v$ z $\overline{G}$ dostáváme:
>     $$V(\overline{G} - v) = V(\overline{G}) \setminus \{v\} = V(G) \setminus \{v\}$$
> - Vidíme, že množiny vrcholů se rovnají:
>   $$V(\overline{H}) = V(\overline{G} - v) = V(G) \setminus \{v\}$$
>
> #### 2. Rovnost množin hran:
> Zvolme libovolnou neuspořádanou dvojici různých vrcholů $\{x, y\}$, kde $x, y \in V(G) \setminus \{v\}$ ($x \neq y$).
> Postupnými ekvivalencemi prověříme podmínku pro příslušnost hrany $\{x, y\}$ k oběma grafům:
> 
> $$\begin{aligned}
> \{x, y\} \in E(\overline{H}) &\iff \{x, y\} \notin E(H) \quad &&\text{(z definice doplňku } \overline{H} \text{ vzhledem k } V(H)) \\
> &\iff \{x, y\} \notin E(G) \quad &&\text{(protože } x, y \neq v \text{ a } H \text{ je indukovaný na } V \setminus \{v\}) \\
> &\iff \{x, y\} \in E(\overline{G}) \quad &&\text{(z definice doplňku } \overline{G}) \\
> &\iff \{x, y\} \in E(\overline{G} - v) \quad &&\text{(protože } x, y \in V(\overline{G}) \setminus \{v\})
> \end{aligned}$$
> 
> Protože ekvivalence platí pro libovolnou dvojici $\{x, y\}$, množiny hran se přesně shodují:
> $$E(\overline{H}) = E(\overline{G} - v)$$
> 
> Protože se rovnají množiny vrcholů i množiny hran, grafy jsou identické:
> $$\mathbf{\overline{H} = \overline{G} - v}$$

---

## Úloha 10.17: Cesta délky $k$ v $k$-regulárním grafu
* **Zdroj:** FitWiki (Malík 2018 `061218-1100malik`, Opravný 2021 `opravny_2021`)
* **Relevantní skripta:** [Definice 1.1 (Regulární graf)](https://truepeak78.github.io/ag1/01_Z%C3%A1klady_graf%C5%AF/1.1_Z%C3%A1kladn%C3%AD_pojmy/#def-1.1) · [Definice 2.1 (Cesta v grafu)](https://truepeak78.github.io/ag1/02_Souvislost_graf%C5%AF/2.1_Souvislost_graf%C5%AF/#def-2.1)

### Zadání
Dokažte, že v každém $k$-regulárním grafu $G$ ($k \ge 1$) existuje jednoduchá cesta délky alespoň $k$ (tj. cesta obsahující alespoň $k$ hran a $k + 1$ různých vrcholů).

> [!success]- Řešení
> ### Důkaz (Přes nejdelší jednoduchou cestu v grafu)
> 1. Protože graf $G$ je konečný, množina všech jednoduchých cest v $G$ je neprázdná (každá hrana tvoří cestu délky 1) a konečná.
> 2. V grafu $G$ tedy nutně existuje **nejdelší jednoduchá cesta** (cesta s maximálním počtem hran). Označme ji jako:
>    $$P = (v_0, v_1, v_2, \dots, v_m)$$
>    kde $v_0, \dots, v_m$ jsou vzájemně různé vrcholy a její délka je $m$ hran ($m \ge 1$).
> 3. Uvažujme koncový vrchol cesty $v_m$:
>    - Graf $G$ je $k$-regulární, proto stupeň vrcholu $v_m$ je přesně:
>      $$\deg_G(v_m) = k$$
>    - To znamená, že z vrcholu $v_m$ vychází $k$ různých hran do $k$ různých sousedů.
> 4. **Klíčové pozorování (všichni sousedé leží na cestě $P$):**
>    - Žádný soused vrcholu $v_m$ nemůže ležet mimo cestu $P$!
>    - Kdyby totiž existoval soused $w \in N(v_m)$ takový, že $w \notin \{v_0, v_1, \dots, v_m\}$, pak bychom mohli cestu $P$ prodloužit o hranu $\{v_m, w\}$:
>      $$P' = (v_0, v_1, \dots, v_m, w)$$
>    - Cesta $P'$ by byla jednoduchou cestou délky $m + 1$, což je přímý **SPOR** s tím, že $P$ byla zvolena jako nejdelší jednoduchá cesta v celém grafu!
> 5. Proto všech $k$ sousedů vrcholu $v_m$ musí patřit mezi vrcholy cesty $P$:
>    $$N(v_m) \subseteq \{v_0, v_1, \dots, v_{m-1}\}$$
> 6. Protože $v_m$ má $k$ různých sousedů a všechny leží v množině $\{v_0, \dots, v_{m-1}\}$, mohutnost této množiny musí být alespoň $k$:
>    $$m = |\{v_0, v_1, \dots, v_{m-1}\}| \ge |N(v_m)| = k$$
> 7. Z nerovnosti $m \ge k$ přímo vyplývá, že délka nejdelší cesty $P$ je **alespoň $k$**, a cesta $P$ tedy obsahuje alespoň $k+1$ různých vrcholů.
