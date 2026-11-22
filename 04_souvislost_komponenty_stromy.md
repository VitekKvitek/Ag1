# 04. Souvislost, komponenty, stromy a kostry – Zadání z písemek a řešení

Tento dokument obsahuje úlohy zaměřené na souvislost, extrémní počty hran v komponentách, charakterizaci stromů, vlastnosti listů, kostry grafu a bipartitní barvení.

---

## Úloha 4.1: Extrémy počtu hran pro daný počet komponent

* **Zdroj:** FitWiki (Opravný 2021, Baier 2016, Plachý 2017)
* **Relevantní skripta:** [Definice 2.1 a 2.2 (Souvislost a komponenty)](https://truepeak78.github.io/ag1/02_Souvislost_graf%C5%AF/2.1_Souvislost_graf%C5%AF/#def-2.1) · [Věta 3.3 (Charakterizace stromů)](https://truepeak78.github.io/ag1/03_Stromy/3.2_Charakterizace_strom%C5%AF/#theorem-3.3)

### Zadání
1. Kolik nejméně a nejvíce hran může mít neorientovaný graf se $14$ vrcholy a $6$ souvislými komponentami? Uveďte obecný vzorec pro $n$ vrcholů a $c$ komponent ($1 \le c \le n$).
2. Kolik nejméně a nejvíce hran má neorientovaný **acyklický** graf se $154$ vrcholy a $4$ komponentami souvislosti?

> [!success]- Řešení
> ### 1. Graf s $n=14$ vrcholy a $c=6$ komponentami
> - **Minimální počet hran:**
>   Minima dosáhneme, pokud je každá komponenta stromem (neobsahuje žádné cykly, graf je lesem).
>   Strom s $n_i$ vrcholy má právě $n_i - 1$ hran.
>   Součet přes všech $c$ komponent:
>   $$|E|_{\min} = \sum_{i=1}^c (n_i - 1) = \sum_{i=1}^c n_i - \sum_{i=1}^c 1 = n - c$$
>   Pro $n=14, c=6$:
>   $$|E|_{\min} = 14 - 6 = \mathbf{8 \text{ hran}}.$$
>
> - **Maximální počet hran:**
>   Maxima dosáhneme, pokud jsou komponenty co nejvíce nerovnoměrné: $c - 1$ komponent tvoří izolované vrcholy ($K_1$, tj. 0 hran) a veškeré zbylé vrcholy jsou soustředěny v jedné jediné komponentě, která tvoří úplný graf $K_{n - (c - 1)}$.
>   Počet vrcholů této velké komponenty je $n - c + 1$.
>   $$|E|_{\max} = \binom{n - c + 1}{2} = \frac{(n - c + 1)(n - c)}{2}$$
>   Pro $n=14, c=6$:
>   Počet vrcholů v $K$ je $14 - 6 + 1 = 9$.
>   $$|E|_{\max} = \binom{9}{2} = \frac{9 \cdot 8}{2} = \mathbf{36 \text{ hran}}.$$
>
> ---
>
> ### 2. Acyklický graf se $154$ vrcholy a $4$ komponentami
> Acyklický graf je z definice **les**. Každá jeho souvislá komponenta je stromem bez cyklů.
> V lese s $n$ vrcholy a $c$ komponentami je počet hran **vždy pevně dán**:
> $$|E| = n - c = 154 - 4 = \mathbf{150}$$
> Nejméně i nejvíce hran je tedy **právě 150**. (Acykličnost nepřipouští žádné hrany navíc).

---

## Úloha 4.2: Přidání hrany do stromu a vznik kružnice

* **Zdroj:** FitWiki (7.11.2025, Opravný 2021)
* **Relevantní skripta:** [Věta 3.3 (Charakterizace stromů)](https://truepeak78.github.io/ag1/03_Stromy/3.2_Charakterizace_strom%C5%AF/#theorem-3.3) · [Definice 3.1 (Definice stromu a lesa)](https://truepeak78.github.io/ag1/03_Stromy/3.1_Z%C3%A1kladn%C3%AD_pojmy/#def-3.1)

### Zadání
Nechť $T = (V, E)$ je strom a $e = \{u, v\} \notin E$ je nová hrana mezi vrcholy stromu ($u \neq v$).
Dokažte, že graf $T + e = (V, E \cup \{e\})$ obsahuje **právě jednu** kružnici.

> [!success]- Řešení
> **Důkaz:**
> Podle věty o charakterizaci stromů ([Věta 3.3](https://truepeak78.github.io/ag1/03_Stromy/3.2_Charakterizace_strom%C5%AF/#theorem-3.3)) existuje mezi libovolnými dvěma vrcholy stromu $u, v$ **právě jedna cesta** $P_{uv} = (u = x_0, x_1, \dots, x_k = v)$ v $T$.
>
> 1. **Existence alespoň jedné kružnice:**
>    Přidáním hrany $e = \{u, v\}$ ke grafu $T$ spojí hrana $e$ konce cesty $P_{uv}$.
>    Posloupnost $(u = x_0, x_1, \dots, x_k = v, u)$ tvoří kružnici $C$ o délce $k+1$. Graf $T + e$ tedy obsahuje alespoň jednu kružnici.
>
> 2. **Jednoznačnost (neexistence jiné kružnice):**
>    Předpokládejme pro spor, že v $T + e$ existují dvě různé kružnice $C_1 \neq C_2$.
>    - Strom $T$ je acyklický, takže žádná kružnice nemůže sestávat pouze z hran původního stromu $E$. Každá kružnice v $T + e$ musí nutně obsahovat novou hranu $e = \{u, v\}$.
>    - Odebráním hrany $e$ z kružnice $C_1$ získáme $u$-$v$-cestu $P_1$ tvořenou výhradně hranami z $E(T)$.
>    - Odebráním hrany $e$ z kružnice $C_2$ získáme $u$-$v$-cestu $P_2$ tvořenou výhradně hranami z $E(T)$.
>    - Protože $C_1 \neq C_2$, musí být cesty $P_1$ a $P_2$ různé ($P_1 \neq P_2$).
>    - To je však v přímém **sporu** s vlastností stromu $T$, že mezi $u$ a $v$ existuje v $T$ právě jedna cesta.
> Graf $T + e$ proto obsahuje právě jednu kružnici. $\blacksquare$

---

## Úloha 4.3: Hrana v kostře a hrana mimo kostru

* **Zdroj:** FitWiki (Dvořák 2022, Knop 2020, Hušek 2022, 2023)
* **Relevantní skripta:** [Definice 3.3 a Věta 3.4 (Kostra grafu a její existence)](https://truepeak78.github.io/ag1/03_Stromy/3.3_Kostra_grafu/#def-3.3) · [Věta 3.3 (Charakterizace stromů)](https://truepeak78.github.io/ag1/03_Stromy/3.2_Charakterizace_strom%C5%AF/#theorem-3.3)

### Zadání
Nechť $G = (V, E)$ je souvislý neorientovaný graf s alespoň dvěma vrcholy a $e \in E$ je jeho libovolná hrana.
1. Dokažte, že existuje kostra grafu $G$, která hranu $e$ **obsahuje**.
2. Rozhodněte, zda pro každou hranu $e \in E$ nutně existuje kostra grafu $G$, která hranu $e$ **neobsahuje**.

> [!success]- Řešení
> ### 1. Důkaz existence kostry obsahující hranu $e$
> Jednoprvková množina hran $T_0 = \{e\}$ tvoří acyklický podgraf grafu $G$ (jedna hrana netvoří cyklus).
> Protože $G$ je souvislý, existuje algoritmus postupného doplňování hran do kostry (např. Kruskalův / Jarníkův přístup nebo doplňování z lesa):
> - Dokud podgraf $(V, T_i)$ není souvislý, existuje v $G$ hrana $e' \in E \setminus T_i$ spojující dvě různé komponenty podgrafu $(V, T_i)$.
> - Přidáním $e'$ nevznikne cyklus a sníží se počet komponent.
> - Po $|V| - 1$ krocích získáme acyklický souvislý podgraf obsahující všechny vrcholy grafu $G$, který obsahuje původní hranu $e$.
> Tento podgraf je hledanou kostrou. $\blacksquare$
>
> ---
>
> ### 2. Existence kostry neobsahující $e$
> **Tvrzení NEPLATÍ (vyvrácení protipříkladem).**
> 
> **Protipříklad:**
> Nechť $G$ je libovolný strom (např. cesta $P_3$ s hranami $\{1,2\}$ a $\{2,3\}$).
> Protože $G$ je sám stromem, má **právě jednu jedinou kostru**, kterou je graf $G$ sám ([Definice 3.3](https://truepeak78.github.io/ag1/03_Stromy/3.3_Kostra_grafu/#def-3.3)).
> Tato jediná kostra obsahuje všechny hrany $E(G)$. Neexistuje tedy žádná kostra, která by neobsahovala hranu $e = \{1,2\}$.
> *(Obecně: je-li hrana $e$ **mostem** grafu $G$, pak vynecháním $e$ vznikne nesouvislý graf, tudíž $e$ musí ležet v každé kostře grafu $G$).*

---

## Úloha 4.4: Počet listů ve stromu

* **Zdroj:** FitWiki (Knop 2021)
* **Relevantní skripta:** [Věta 3.2 (Existence listů ve stromu)](https://truepeak78.github.io/ag1/03_Stromy/3.1_Z%C3%A1kladn%C3%AD_pojmy/#theorem-3.2) · [Věta 1.1 (Princip sudosti)](https://truepeak78.github.io/ag1/01_Z%C3%A1klady_graf%C5%AF/1.1_Z%C3%A1kladn%C3%AD_pojmy/#theorem-1.1)

### Zadání
Nechť $T = (V, E)$ je strom s $n \ge 2$ vrcholy.
Určete minimální a maximální možný počet listů (vrcholů stupně 1) v $T$. Své tvrzení dokažte.

> [!success]- Řešení
> - **Minimální počet listů je 2 (pro každé $n \ge 2$):**
>   - V každém stromu s $n \ge 2$ vrcholy existují **alespoň 2 listy** ([Věta 3.2](https://truepeak78.github.io/ag1/03_Stromy/3.1_Z%C3%A1kladn%C3%AD_pojmy/#theorem-3.2)): vezmeme-li nejdelší cestu v $T$, její dva koncové vrcholy musí mít stupeň 1 (jinak by se cesta dala prodloužit do dosud nenavštíveného vrcholu, nebo by vznikl cyklus, což je spor s acykličností).
>   - Minima 2 je dosaženo např. pro **cestu $P_n$**, kde mají pouze 2 koncové vrcholy stupeň 1 a všechny vnitřní vrcholy mají stupeň 2.
>
> - **Maximální počet listů:**
>   - **Případ $n = 2$:** Jediným stromem o 2 vrcholech je $K_2$ (jediná hrana $\{u, v\}$). Oba vrcholy mají stupeň 1, tedy počet listů je **právě 2** (což odpovídá $n$).
>   - **Případ $n \ge 3$:** Maximální počet listů je **$n - 1$**.
>     - **Důkaz:** Součet stupňů ve stromu je $\sum_{v \in V} \deg(v) = 2|E| = 2(n - 1) = 2n - 2$.
>     - Kdyby měl strom $n$ listů, platilo by $\sum \deg(v) = n \cdot 1 = n$. Z rovnosti $n = 2n - 2$ plyne $n = 2$.
>     - Pro $n \ge 3$ tedy strom nemůže mít všechny vrcholy stupně 1 (musí existovat alespoň jeden vrchol se stupněm $\ge 2$).
>     - Má-li alespoň jeden vrchol stupeň $\ge 2$, může mít strom nejvýše $n - 1$ listů.
>     - Maxima $n - 1$ listů je skutečně dosaženo pro **hvězdu $K_{1, n-1}$**, kde centrální vrchol má stupeň $n-1$ a všech zbývajících $n-1$ vrcholů má stupeň 1 (jsou to listy).
>   - **Shrnutí maxima:** Pro $n=2$ je maximum **2 listy**; pro $n \ge 3$ je maximum **$n - 1$ listů**.

---

## Úloha 4.5: Souvislost doplňku po odebrání artikulace

* **Zdroj:** FitWiki (4.11.2025 Varianta C, Catay 2024)
* **Relevantní skripta:** [Definice 2.1 a 2.2 (Souvislost a komponenty)](https://truepeak78.github.io/ag1/02_Souvislost_graf%C5%AF/2.1_Souvislost_graf%C5%AF/#def-2.1) · [Definice 1.4 (Doplněk grafu)](https://truepeak78.github.io/ag1/01_Z%C3%A1klady_graf%C5%AF/1.3_B%C4%9B%C5%BEn%C3%A9_grafy/#def-1.4)

### Zadání
Nechť $G = (V, E)$ je souvislý neorientovaný graf a $v \in V$ je vrchol takový, že podgraf $G - v$ je **nesouvislý**.
Dokažte, že graf $\overline{G} - v$ (doplněk grafu $G$ bez vrcholu $v$) je **souvislý**.

> [!success]- Řešení
> **Důkaz:**
> Označme $H = G - v$. Podle předpokladu je $H$ nesouvislý, má tedy alespoň dvě souvislé komponenty $C_1, C_2, \dots, C_k$ ($k \ge 2$).
> Množina vrcholů grafu $\overline{G} - v$ je rovna $V(H) = V(G) \setminus \{v\}$.
> V grafu $H$ nevede **žádná hrana** mezi vrcholy ležícími v různých komponentách (tj. pro $x \in V(C_i)$ a $y \in V(C_j)$ s $i \neq j$ platí $\{x, y\} \notin E(G)$).
> 
> Z definice doplňku grafu $\overline{G}$ to znamená, že:
> $$\forall x \in V(C_i), \forall y \in V(C_j), i \neq j \implies \{x, y\} \in E(\overline{G} - v).$$
> V doplňku $\overline{G} - v$ tedy existuje hrana mezi **každou dvojicí vrcholů z různých komponent** $C_i, C_j$.
>
> Chceme dokázat, že mezi libovolnými dvěma vrcholy $a, b \in V(H)$ existuje cesta v $\overline{G} - v$:
> - **Případ 1 ($a$ a $b$ jsou z různých komponent, $a \in C_i, b \in C_j, i \neq j$):**
>   Pak přímo existuje hrana $\{a, b\} \in E(\overline{G} - v)$, což je cesta délky 1.
> - **Případ 2 ($a$ a $b$ jsou ze stejné komponenty, $a, b \in C_i$):**
>   Protože $k \ge 2$, existuje jiná komponenta $C_j$ ($j \neq i$).
>   Zvolme libovolný vrchol $z \in V(C_j)$.
>   Podle Případu 1 existují v $\overline{G} - v$ hrany $\{a, z\}$ i $\{z, b\}$.
>   Posloupnost $(a, z, b)$ tvoří cestu délky 2 mezi $a$ a $b$ v $\overline{G} - v$.
>
> V obou případech existuje mezi libovolnými dvěma vrcholy cesta. Graf $\overline{G} - v$ je tedy souvislý. $\blacksquare$

---

## Úloha 4.6: Dvoubarevné stromy a existence listu dané barvy

* **Zdroj:** FitWiki (Šestáková 2019)
* **Relevantní skripta:** [Věta 3.3 (Charakterizace stromů)](https://truepeak78.github.io/ag1/03_Stromy/3.2_Charakterizace_strom%C5%AF/#theorem-3.3) · [Definice 1.4 a Věta 1.3 (Bipartitní grafy)](https://truepeak78.github.io/ag1/01_Z%C3%A1klady_graf%C5%AF/1.3_B%C4%9B%C5%BEn%C3%A9_grafy/#def-1.4)

### Zadání
Mějme neorientovaný graf $T = (V, E)$, který je stromem o $|V| \ge 2$ vrcholech.
Vrcholy $T$ jsou obarveny červenou nebo modrou barvou tak, že žádné dva sousední vrcholy nemají stejnou barvu (bipartitní obarvení).
Dokažte tvrzení:
*„Pokud $T$ obsahuje alespoň tolik červených vrcholů jako modrých (tj. $|V_{\text{červená}}| \ge |V_{\text{modrá}}|$), potom v $T$ existuje alespoň jeden červený list.“*

> [!success]- Řešení
> **Důkaz (sporem):**
> Označme $R = V_{\text{červená}}$ a $B = V_{\text{modrá}}$. Platí $|R| \ge |B|$ a $V = R \cup B$, přičemž každá hrana stromu spojuje vrchol z $R$ s vrcholem z $B$.
> Předpokládejme pro spor, že v $T$ **neexistuje žádný červený list**.
> - Každý list stromu (vrchol stupně 1) proto musí být modrý.
> - Každý červený vrchol $r \in R$ tedy musí mít stupeň alespoň 2:
>   $$\forall r \in R: \deg(r) \ge 2$$
> - Sečteme stupně všech červených vrcholů:
>   $$\sum_{r \in R} \deg(r) \ge 2 |R|$$
> - Protože je graf bipartitní a každá hrana má právě jeden koncový vrchol v $R$, je součet stupňů v $R$ roven celkovému počtu hran v $T$:
>   $$|E| = \sum_{r \in R} \deg(r) \ge 2 |R|$$
> - Ze zadání víme, že $|R| \ge |B|$, a tedy $|V| = |R| + |B| \le |R| + |R| = 2 |R|$, z čehož plyne $|R| \ge \frac{|V|}{2}$.
> - Dosazením do předchozí nerovnosti dostáváme:
>   $$|E| \ge 2 |R| \ge 2 \cdot \frac{|V|}{2} = |V|$$
> - Pro každý strom však podle Věty o charakterizaci stromů ([Věta 3.3](https://truepeak78.github.io/ag1/03_Stromy/3.2_Charakterizace_strom%C5%AF/#theorem-3.3)) nutně platí:
>   $$|E| = |V| - 1$$
> - Dostáváme $|V| - 1 \ge |V|$, tedy $-1 \ge 0$, což je **spor**.
> Předpoklad byl chybný, v $T$ musí existovat alespoň jeden červený list. $\blacksquare$

---

## Úloha 4.7: Hellyho vlastnost pro podstromy

* **Zdroj:** FitWiki (Dvořák 2024)
* **Relevantní skripta:** [Definice 3.1 a Věta 3.3 (Vlastnosti stromů a jednoznačnost cest)](https://truepeak78.github.io/ag1/03_Stromy/3.2_Charakterizace_strom%C5%AF/#theorem-3.3)

### Zadání
Nechť $T = (V, E)$ je strom a $T_1, T_2, T_3$ jsou tři podstromy stromu $T$ (souvislé podgrafy).
Předpokládejme, že každé dva z těchto podstromů mají neprázdný průnik (tj. $V(T_1) \cap V(T_2) \neq \emptyset$, $V(T_2) \cap V(T_3) \neq \emptyset$ a $V(T_1) \cap V(T_3) \neq \emptyset$).
Dokažte, že všechny tři podstromy mají alespoň jeden společný vrchol, tedy:
$$V(T_1) \cap V(T_2) \cap V(T_3) \neq \emptyset$$

> [!success]- Řešení
> **Důkaz:**
> 1. Zvolme libovolné vrcholy v průnicích dvojic:
>    - $x \in V(T_1) \cap V(T_2)$
>    - $y \in V(T_2) \cap V(T_3)$
>    - $z \in V(T_1) \cap V(T_3)$
>    Pokud se libovolné dva z těchto tří vrcholů rovnají (např. $x = y$), pak $x \in V(T_1) \cap V(T_2) \cap V(T_3)$ a tvrzení triviálně platí.
> 2. Nechť jsou $x, y, z$ navzájem různé.
>    Ve stromu $T$ existuje mezi každými dvěma vrcholy právě jedna cesta: označme $P_{xy}$ jedinou cestu mezi $x$ a $y$, $P_{yz}$ cestu mezi $y$ a $z$ a $P_{xz}$ cestu mezi $x$ a $z$.
> 3. Protože jsou podstromy $T_1, T_2, T_3$ souvislé:
>    - $x \in T_2$ a $y \in T_2 \implies$ celá cesta $P_{xy}$ leží v $T_2$.
>    - $y \in T_3$ a $z \in T_3 \implies$ celá cesta $P_{yz}$ leží v $T_3$.
>    - $x \in T_1$ a $z \in T_1 \implies$ celá cesta $P_{xz}$ leží v $T_1$.
> 4. Ve stromu mají libovolné tři cesty spojující trojici vrcholů $x, y, z$ právě jeden společný vrchol $m$ (tzv. medián trojice):
>    - Uvažujme cestu z vrcholu $z$ k cestě $P_{xy}$. Nechť $m$ je první vrchol cesty $P_{xy}$, na který tato cesta narazí ($m$ je projekce $z$ na $P_{xy}$).
>    - Vrchol $m$ tedy leží na $P_{xy}$.
>    - Z jednoznačnosti cest ve stromu se cesta $P_{xz}$ skládá z úseku z $x$ do $m$ (na $P_{xy}$) a úseku z $m$ do $z$, proto $m \in V(P_{xz})$.
>    - Analogicky se cesta $P_{yz}$ skládá z úseku z $y$ do $m$ (na $P_{xy}$) a úseku z $m$ do $z$, proto $m \in V(P_{yz})$.
>    - Vrchol $m$ je tedy společný všem třem cestám: $m \in V(P_{xy}) \cap V(P_{yz}) \cap V(P_{xz})$.
> 5. Jelikož $m \in P_{xz} \subseteq T_1$, $m \in P_{xy} \subseteq T_2$ a $m \in P_{yz} \subseteq T_3$, vrchol $m$ leží ve všech třech podstromech současně:
>    $$m \in V(T_1) \cap V(T_2) \cap V(T_3) \neq \emptyset. \quad \blacksquare$$


---

## Úloha 4.8: Počet koster grafu s bloky kružnic a mosty
* **Zdroj:** FitWiki (Scholtzová 2020, scan testu `202011301336301000.jpg`)
* **Relevantní skripta:** [Definice 2.3 a Věta 2.2 (Kostra grafu a mosty)](https://truepeak78.github.io/ag1/02_Souvislost_graf%C5%AF/2.2_Stromy/#def-2.3)

### Zadání
1. Definujte **kostru** souvislého neorientovaného grafu $G = (V, E)$.
2. Mějme souvislý graf $G$ zadaný následující strukturou (odpovídající testu Scholtzová 2020):
   - Graf obsahuje dva listy (vrcholy stupně 1), které jsou hranami připojeny k jednomu vrcholu trojúhelníku $C_3$ (kružnice délky 3).
   - Z jiného vrcholu tohoto trojúhelníku vede spojovací hrana (most) do jednoho vrcholu pětiúhelníku $C_5$ (kružnice délky 5).
   - Žádné další hrany v grafu nejsou.
   Určete přesný počet všech různých koster grafu $G$ a své tvrzení precizně zdůvodněte.

> [!success]- Řešení
> ### 1. Definice kostry
> **Kostra** souvislého neorientovaného grafu $G = (V, E)$ je podgraf $T = (V, E_T)$ takový, že $T$ je strom a obsahuje všechny vrcholy grafu $G$ ($V(T) = V(G)$).
> Ekvivalentně: kostra je minimální souvislý podgraf obsahující všechny vrcholy (nebo maximální acyklický podgraf na $V$).
>
> ---
>
> ### 2. Analýza struktury grafu
> Spočtěme vrcholy a hrany grafu $G$:
> - Vrcholy: 2 listy + 3 vrcholy na trojúhelníku + 5 vrcholů na pětiúhelníku = **$10$ vrcholů**.
> - Hrany: 2 hrany k listům + 3 hrany trojúhelníku + 1 spojovací hrana + 5 hran pětiúhelníku = $2 + 3 + 1 + 5 = \mathbf{11}$ hran.
> - Každá kostra grafu s 10 vrcholy musí být stromem, a proto musí obsahovat přesně $|V| - 1 = 10 - 1 = \mathbf{10}$ hran.
> - K vytvoření kostry je tedy nutné z původních 11 hran odebrat právě:
>   $$|E(G)| - (|V(G)| - 1) = 11 - 10 = \mathbf{1 \text{ hranu}} \quad \text{... počkat!}$$
>   *Pozor na přepočet cyklů:*
>   Graf obsahuje **dvě nezávislé kružnice**: trojúhelník $C_3$ a pětiúhelník $C_5$.
>   Kružnice $C_3$ a $C_5$ jsou hranově disjunktní.
>   Počet hran je ve skutečnosti:
>   - 2 hrany incidentní s listy
>   - 3 hrany v $C_3$
>   - 1 most spojující $C_3$ a $C_5$
>   - 5 hran v $C_5$
>   Celkem hran: $2 + 3 + 1 + 5 = 11$.
>   Počet vrcholů: 2 listy + 3 ($C_3$) + 5 ($C_5$) = 10 vrcholů.
>   Kostra na 10 vrcholech musí mít $10 - 1 = 9$ hran!
>   Musíme tedy odebrat přesně $11 - 9 = \mathbf{2 \text{ hrany}}$.
>
> ---
>
> ### 3. Odstranění hran a pravidlo součinu
> 1. **Mosty grafu (hrany, které nesmí být odebrány):**
>    - Dvě hrany vedoucí do listů jsou mosty (jejich odebráním by se list stal izolovaným vrcholem, což poruší souvislost kostry).
>    - Spojovací hrana mezi trojúhelníkem a pětiúhelníkem je rovněž mostem (její odebrání rozdělí graf na 2 komponenty).
>    - Všechny tyto 3 mosty **musí být obsaženy v každé kostře**.
> 
> 2. **Kružnice v grafu:**
>    - Aby byl výsledný podgraf stromem (acyklický), nesmí obsahovat žádnou kružnici:
>      - Z kružnice $C_3$ (3 hrany) musíme přerušit cyklus odebráním alespoň jedné hrany. Pokud odebereme právě 1 hranu z $C_3$, zůstane z $C_3$ souvislá cesta na 3 vrcholech. To dává přesně $\binom{3}{1} = \mathbf{3 \text{ možnosti}}$.
>      - Z kružnice $C_5$ (5 hran) musíme obdobně přerušit cyklus odebráním alespoň jedné hrany. Odebráním právě 1 hrany z $C_5$ vznikne souvislá cesta na 5 vrcholech. To dává $\binom{5}{1} = \mathbf{5 \text{ možností}}$.
> 
> 3. Protože kružnice $C_3$ a $C_5$ nesdílejí žádnou hranu, volba odebrané hrany v trojúhelníku nijak neovlivňuje volbu odebrané hrany v pětiúhelníku.
>    Odebráním 1 hrany z $C_3$ a 1 hrany z $C_5$ odebereme celkem $1 + 1 = 2$ hrany, zbude $11 - 2 = 9$ hran, podgraf zůstane souvislý a bude acyklický (tedy strom na všech 10 vrcholech).
> 
> Podle kombinatorického pravidla součinu je celkový počet koster:
> $$\text{Počet koster} = 3 \times 5 = \mathbf{15}.$$

---

## Úloha 4.9: Rozdělení stromu o 50 hranách
* **Zdroj:** FitWiki (Pokorný 2021 `sbirka-2021`, scan testu `20211125_091802.jpg`)
* **Relevantní skripta:** [Definice 2.2 a Věta 2.1 (Základní vlastnosti stromů)](https://truepeak78.github.io/ag1/02_Souvislost_graf%C5%AF/2.2_Stromy/#theorem-2.1)

### Zadání
Mějme strom $T = (V, E)$, který má $|E(T)| = 50$ hran.
Odstraněním jedné libovolné hrany $e \in E(T)$ se strom $T$ rozpadne na dva disjunktní podstromy $T_1 = (V_1, E_1)$ a $T_2 = (V_2, E_2)$.
Předpokládejme, že o těchto podstromech platí, že **počet hran v $T_1$ se přesně rovná počtu vrcholů v $T_2$**, tedy:
$$|E(T_1)| = |V(T_2)|$$
Určete přesné počty vrcholů $|V(T_1)|$ a $|V(T_2)|$ obou vzniklých podstromů.

> [!success]- Řešení
> 1. **Počet vrcholů původního stromu $T$:**
>    Ve stromu je počet vrcholů vždy o 1 větší než počet hran ([Věta 2.1](https://truepeak78.github.io/ag1/02_Souvislost_graf%C5%AF/2.2_Stromy/#theorem-2.1)):
>    $$|V(T)| = |E(T)| + 1 = 50 + 1 = 51.$$
> 
> 2. **Vztah mezi vrcholy a hranami podstromů:**
>    Odstraněním jedné hrany vzniknou dvě komponenty souvislosti $T_1$ a $T_2$, přičemž každá z nich je rovněž stromem:
>    - $V(T) = V(T_1) \cup V(T_2)$ a $V(T_1) \cap V(T_2) = \emptyset \implies |V(T_1)| + |V(T_2)| = 51$.
>    - Protože $T_1$ je strom, platí pro něj:
>      $$|E(T_1)| = |V(T_1)| - 1.$$
> 
> 3. **Sestavení a vyřešení rovnice:**
>    Ze zadání dosadíme podmínku $|E(T_1)| = |V(T_2)|$:
>    $$|V(T_1)| - 1 = |V(T_2)|$$
>    Zároveň víme, že $|V(T_2)| = 51 - |V(T_1)|$:
>    $$|V(T_1)| - 1 = 51 - |V(T_1)|$$
>    $$2 |V(T_1)| = 52 \implies |V(T_1)| = \mathbf{26}.$$
> 
> 4. **Dopočtení $|V(T_2)|$ a kontrola:**
>    $$|V(T_2)| = 51 - 26 = \mathbf{25}.$$
>    - Počet hran v $T_1$: $|E(T_1)| = 26 - 1 = 25$.
>    - Počet vrcholů v $T_2$: $|V(T_2)| = 25$.
>    - Podmínka $|E(T_1)| = |V(T_2)| = 25$ je přesně splněna.
> 
> Podstrom $T_1$ má **26 vrcholů** a podstrom $T_2$ má **25 vrcholů**.

---

## Úloha 4.10: Minimální a maximální počet listů ve stromu
* **Zdroj:** FitWiki (Knop 2021 `knop2021`)
* **Relevantní skripta:** [Definice 2.2 (Strom a listy)](https://truepeak78.github.io/ag1/02_Souvislost_graf%C5%AF/2.2_Stromy/#def-2.2) · [Věta 2.1 (Vlastnosti stromů)](https://truepeak78.github.io/ag1/02_Souvislost_graf%C5%AF/2.2_Stromy/#theorem-2.1)

### Zadání
Určete minimální a maximální možný počet listů (vrcholů stupně 1) ve stromu $T$ na $n$ vrcholech ($n \ge 2$). Uveďte přesné hodnoty a popište extremální grafy, které těchto mezí dosahují.

> [!success]- Řešení
> ### 1. Minimální počet listů
> **Minimální počet listů je roven $2$.**
> - **Důkaz:** 
>   Podle základní věty teorie stromů má každý strom na $n \ge 2$ vrcholech **alespoň 2 listy**:
>   Nechť $P = (v_1, v_2, \dots, v_k)$ je nejdelší jednoduchá cesta v $T$. Protože $n \ge 2$, má cesta délku alespoň 1.
>   Koncové vrcholy $v_1$ a $v_k$ nemohou mít žádné jiné sousedy mimo cestu $P$ (jinak by se cesta dala prodloužit), ani žádné jiné sousedy na cestě $P$ kromě $v_2$ a $v_{k-1}$ (jinak by vznikla kružnice, což je ve stromu vyloučeno). Proto $\deg(v_1) = 1$ a $\deg(v_k) = 1$.
>   Strom tedy vždy obsahuje alespoň 2 listy.
> - **Extremální graf:** Cesta $P_n$.
>   V cestě $P_n$ mají dva koncové vrcholy stupeň 1 a všech $n - 2$ vnitřních vrcholů má stupeň 2. Počet listů je přesně 2.
>
> ### 2. Maximální počet listů
> **Maximální počet listů je roven $n - 1$.**
> - **Důkaz:**
>   Předpokládejme, že by strom měl $n$ listů. Pak by všechny vrcholy měly stupeň 1, graf by byl 1-regulární, což je nesouvislý graf tvořený nezávislými hranami (pro $n \ge 3$), nebo by měl $n=2$. Pro $n \ge 3$ však strom musí být souvislý a mít alespoň jeden vrchol stupně $\ge 2$ (aby propojil ostatní).
>   Tedy počet vnitřních vrcholů (vrcholů stupně $\ge 2$) musí být alespoň 1.
>   Počet listů tedy nemůže překročit $n - 1$.
> - **Extremální graf:** Hvězda $K_{1, n-1}$.
>   Hvězda má jeden centrální vrchol spojený se všemi ostatními $n - 1$ vrcholy. Centrální vrchol má stupeň $n - 1$, zatímco všech ostatních $n - 1$ vrcholů má stupeň 1 (jsou to listy).
