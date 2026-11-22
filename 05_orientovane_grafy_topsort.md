# 05. Orientované grafy, DAGy a Topologické uspořádání – Zadání z písemek a řešení

Tento dokument obsahuje úlohy z písemek zaměřené na orientované acyklické grafy (DAG), turnaje, relaci uspořádání a (ne)jednoznačnost topologického uspořádání.

---

## Úloha 5.1: Topologické uspořádání v orientaci úplného grafu (turnaji)

* **Zdroj:** FitWiki (Baier 2016, Sedláček HW)
* **Relevantní skripta:** [Definice 3.4 (Topologické uspořádání)](https://truepeak78.github.io/ag1/03_Stromy/3.4_Topsort/#def-3.4) · [Definice 1.1 a orientované grafy](https://truepeak78.github.io/ag1/01_Z%C3%A1klady_graf%C5%AF/1.1_Z%C3%A1kladn%C3%AD_pojmy/#def-1.1)

### Zadání
Nechť $G = (V, E)$ je orientace úplného grafu $K_n$ (tzv. **turnaj**, tj. pro každou dvojici různých vrcholů $u \neq v$ existuje buď hrana $(u, v) \in E$, nebo $(v, u) \in E$, ale nikdy obě).
Dokažte, že graf $G$ má **nejvýše jedno** topologické uspořádání.

> [!success]- Řešení
> **Důkaz:**
> Předpokládejme, že $G$ má alespoň jedno topologické uspořádání (pokud $G$ obsahuje orientovaný cyklus, nemá žádné topologické uspořádání a tvrzení platí triviálně).
> 
> Nechť $\pi = (v_1, v_2, \dots, v_n)$ je topologické uspořádání vrcholů grafu $G$.
> Podle definice topologického uspořádání ([Definice 3.4](https://truepeak78.github.io/ag1/03_Stromy/3.4_Topsort/#def-3.4)) platí:
> $$\forall (u, v) \in E \implies \pi(u) < \pi(v)$$
> Všechny hrany tedy směřují zleva doprava (od menších indexů k větším).
>
> Uvažujme libovolné dva vrcholy $v_i$ a $v_j$ s $i < j$:
> - Protože $G$ je turnaj, musí mezi $v_i$ a $v_j$ existovat právě jedna orientovaná hrana.
> - Hrana $(v_j, v_i)$ nemůže existovat, protože by vedla zprava doleva ($j > i$), což by porušilo předpoklad, že $\pi$ je topologické uspořádání.
> - Mezi $v_i$ a $v_j$ tedy **nutně existuje hrana $(v_i, v_j) \in E$** pro všechny indexy $1 \le i < j \le n$.
> 
> Z toho plyne, že pro každý vrchol $v_i$ je jeho výstupní stupeň roven:
> $$\deg^+(v_i) = n - i$$
> Vrcholy mají navzájem ostře různé výstupní stupně:
> $$\deg^+(v_1) = n - 1, \quad \deg^+(v_2) = n - 2, \quad \dots, \quad \deg^+(v_n) = 0$$
> Jelikož pořadí v topologickém uspořádání je jednoznačně určeno výstupním stupněm vrcholu ($v_i$ je jednoznačný vrchol s $\deg^+(v_i) = n - i$), nemůže existovat žádné jiné topologické uspořádání. Graf $G$ má tedy nejvýše jedno topologické uspořádání. $\blacksquare$

---

## Úloha 5.2: Orientace bipartitního grafu $K_{n,n}$ s právě jedním uspořádáním

* **Zdroj:** FitWiki & Courses (Vzorový opravný test, Mitura 2016)
* **Relevantní skripta:** [Definice 3.4 (Topologické uspořádání)](https://truepeak78.github.io/ag1/03_Stromy/3.4_Topsort/#def-3.4) · [Definice 1.4 (Bipartitní graf $K_{n,n}$)](https://truepeak78.github.io/ag1/01_Z%C3%A1klady_graf%C5%AF/1.3_B%C4%9B%C5%BEn%C3%A9_grafy/#def-1.4)

### Zadání
Dokažte matematickou indukcí podle $n \ge 1$, že úplný bipartitní graf $K_{n,n}$ lze zorientovat tak, že výsledný orientovaný graf má **právě jedno** topologické uspořádání.

> [!success]- Řešení
> Označme dvě partity grafu $K_{n,n}$ jako $A_n = \{a_1, \dots, a_n\}$ a $B_n = \{b_1, \dots, b_n\}$.
> Graf má $2n$ vrcholů. Chceme zkonstruovat orientaci hran tak, aby jediné topologické uspořádání bylo střídavé pořadí:
> $$a_1, b_1, a_2, b_2, \dots, a_n, b_n$$
>
> **Důkaz matematickou indukcí podle $n$:**
> - **Základní krok ($n = 1$):**
>   Graf $K_{1,1}$ má vrcholy $a_1, b_1$ a jedinou hranu $\{a_1, b_1\}$.
>   Zorientujeme ji jako $(a_1, b_1)$.
>   Jediné topologické uspořádání je $(a_1, b_1)$ (má právě jedno uspořádání). Základní krok platí.
>
> - **Indukční krok ($n \to n + 1$):**
>   Předpokládejme (IP), že pro $K_{n,n}$ s partitami $\{a_1, \dots, a_n\}$ a $\{b_1, \dots, b_n\}$ máme orientaci $E_n$, jejímž jediným topologickým uspořádáním je $\pi_n = (a_1, b_1, \dots, a_n, b_n)$.
>   Přidejme dva nové vrcholy $a_{n+1}$ (do partity $A$) a $b_{n+1}$ (do partity $B$).
>   Všechny nové hrany incidenční s $a_{n+1}$ a $b_{n+1}$ zorientujeme následovně:
>   1. Pro všechny $b_i \in B_n$ zavedeme hranu $(b_i, a_{n+1})$. Tím je $a_{n+1}$ umístěn v topologickém uspořádání **za** všechny dosavadní vrcholy $B_n$ (a tedy i za celou dosavadní posloupnost).
>   2. Zavedeme hranu $(a_{n+1}, b_{n+1})$. Tím je $b_{n+1}$ nucen být až **za** $a_{n+1}$.
>   3. Pro všechny $a_i \in A_n$ zavedeme hranu $(a_i, b_{n+1})$.
>   
>   Ověření acykličnosti a jednoznačnosti v $K_{n+1, n+1}$:
>   - Všechny nově přidané hrany vedou z nižších indexů posloupnosti na vyšší (z $B_n$ do $a_{n+1}$, z $a_{n+1}$ do $b_{n+1}$ a z $A_n$ do $b_{n+1}$), graf tedy neobsahuje žádný zpětný cyklus a je DAG.
>   - Konstrukce přímo obsahuje **orientovanou Hamiltonovskou cestu**:
>     $$a_1 \to b_1 \to a_2 \to b_2 \to \dots \to a_n \to b_n \to a_{n+1} \to b_{n+1}$$
>   - Protože mezi každou dvojicí po sobě jdoucích vrcholů této posloupnosti vede orientovaná hrana, musí v každém platném topologickém uspořádání předcházet předchozí vrchol vrcholu následujícímu.
>   Posloupnost $\pi_{n+1} = (a_1, b_1, \dots, a_{n+1}, b_{n+1})$ je tedy jediným možným topologickým uspořádáním. $\blacksquare$

---

## Úloha 5.3: Kdy má DAG alespoň dvě topologická uspořádání?

* **Zdroj:** FitWiki (Scholtzová 2020)
* **Relevantní skripta:** [Věta 3.7 (Existence topologického uspořádání)](https://truepeak78.github.io/ag1/03_Stromy/3.4_Topsort/#theorem-3.7) · [Algoritmus 3.1 (Topsort)](https://truepeak78.github.io/ag1/03_Stromy/3.4_Topsort/#algo-3.1)

### Zadání
Dokažte, že pokud orientovaný acyklický graf (DAG) $G$ neobsahuje orientovanou Hamiltonovskou cestu, pak existují **alespoň dvě** různá topologická uspořádání grafu $G$.

> [!success]- Řešení
> **Důkaz:**
> DAG má vždy alespoň jedno topologické uspořádání ([Věta 3.7](https://truepeak78.github.io/ag1/03_Stromy/3.4_Topsort/#theorem-3.7)).
> Zvolme libovolné topologické uspořádání $\pi = (v_1, v_2, \dots, v_n)$.
>
> Předpokládejme, že $G$ neobsahuje orientovanou Hamiltonovskou cestu:
> 1. Kdyby pro každý index $i \in \{1, \dots, n - 1\}$ existovala v $G$ orientovaná hrana $(v_i, v_{i+1})$, pak by posloupnost hran $(v_1, v_2), (v_2, v_3), \dots, (v_{n-1}, v_n)$ tvořila orientovanou cestu procházející všemi vrcholy grafu, což je definice orientované Hamiltonovské cesty.
> 2. Jelikož $G$ takovou cestu neobsahuje, musí existovat **alespoň jeden index $i \in \{1, \dots, n - 1\}$** takový, že:
>    $$(v_i, v_{i+1}) \notin E(G).$$
> 3. Mezi vrcholy $v_i$ a $v_{i+1}$ navíc nemůže vést ani opačná hrana $(v_{i+1}, v_i)$, protože $\pi$ je topologické uspořádání a hrany nemohou vést z vyššího indexu na nižší.
> 4. Mezi sousedními vrcholy $v_i$ a $v_{i+1}$ v uspořádání $\pi$ tedy nevede **vůbec žádná hrana**.
> 5. Prohodíme-li v uspořádání $\pi$ tyto dva sousední vrcholy:
>    $$\pi' = (v_1, \dots, v_{i-1}, \mathbf{v_{i+1}}, \mathbf{v_i}, v_{i+2}, \dots, v_n)$$
>    - Všechny hrany mezi vrcholy mimo tuto dvojici mají zachovanou vzájemnou orientaci zleva doprava.
>    - Mezi $v_i$ a $v_{i+1}$ žádná hrana nevede, takže prohozením nemohlo dojít k porušení směru žádné hrany.
> 6. Posloupnost $\pi'$ je tedy dalším platným topologickým uspořádáním a platí $\pi' \neq \pi$.
> Existují tudíž alespoň dvě různá topologická uspořádání. $\blacksquare$

---

## Úloha 5.4: Král v turnaji (cesta délky nejvýše 2)

* **Zdroj:** FitWiki (Knop 2021)
* **Relevantní skripta:** [Definice 1.1 (Orientovaný graf a stupně)](https://truepeak78.github.io/ag1/01_Z%C3%A1klady_graf%C5%AF/1.1_Z%C3%A1kladn%C3%AD_pojmy/#def-1.1) · [Definice 1.3 (Orientovaná cesta)](https://truepeak78.github.io/ag1/01_Z%C3%A1klady_graf%C5%AF/1.1_Z%C3%A1kladn%C3%AD_pojmy/#def-1.3)

### Zadání
Turnaj je orientovaný graf $T = (V, E)$, kde mezi každou dvojicí vrcholů $u \neq v$ vede právě jedna orientovaná hrana.
Dokažte, že v každém turnaji existuje vrchol $r \in V$ (tzv. král), ze kterého vede do každého jiného vrcholu $w \in V \setminus \{r\}$ orientovaná cesta délky **nejvýše 2**.

> [!success]- Řešení
> **Důkaz:**
> Vyberme za vrchol $r$ vrchol s **maximálním výstupním stupněm** v celém turnaji:
> $$\deg^+(r) = \max_{v \in V} \deg^+(v)$$
> Označme:
> - $N^+(r) = \{u \in V \mid (r, u) \in E\}$ množinu přímých následníků vrcholu $r$. Do každého vrcholu $u \in N^+(r)$ vede z $r$ cesta délky 1 (hrana $(r, u)$).
> - $N^-(r) = \{w \in V \mid (w, r) \in E\}$ množinu vrcholů, ze kterých vede hrana do $r$.
>
> Chceme dokázat, že do každého vrcholu $w \in N^-(r)$ vede z $r$ cesta délky 2:
> - Sporem: předpokládejme, že existuje vrchol $w \in N^-(r)$, do kterého nevede z $r$ žádná cesta délky 2.
> - Cesta délky 2 z $r$ do $w$ by znamenala existenci vrcholu $u \in N^+(r)$ takového, že $(u, w) \in E$.
> - Neexistence takové cesty znamená, že pro **žádný** vrchol $u \in N^+(r)$ nevede hrana $(u, w)$.
> - Jelikož $T$ je turnaj, musí mezi každým $u \in N^+(r)$ a $w$ vést hrana opačným směrem, tedy:
>   $$\forall u \in N^+(r): (w, u) \in E.$$
> - Vrchol $w$ má tedy hrany do všech vrcholů z $N^+(r)$, a navíc má ze zadání hranu $(w, r) \in E$.
> - Výstupní stupeň vrcholu $w$ je proto alespoň:
>   $$\deg^+(w) \ge |N^+(r)| + 1 = \deg^+(r) + 1 > \deg^+(r).$$
> - To je však v přímém **sporu** s volbou vrcholu $r$ jakožto vrcholu s maximálním výstupním stupněm!
> Každý vrchol z $N^-(r)$ je tedy dosažitelný z $r$ cestou délky 2. Tvrzení platí. $\blacksquare$


---

## Úloha 5.5: Každá orientace úplného grafu $K_n$ má nejvýše jedno topologické uspořádání
* **Zdroj:** FitWiki (Baier 2016, scan testu `test-ag1-13-12-2016-baier.jpg`)
* **Relevantní skripta:** [Definice 5.3 a Věta 5.1 (Topologické uspořádání)](https://truepeak78.github.io/ag1/05_Orientovan%C3%A9_grafy/5.1_Z%C3%A1kladn%C3%AD_pojmy/#def-5.3) · [Definice 1.4 (Úplný graf)](https://truepeak78.github.io/ag1/01_Z%C3%A1klady_graf%C5%AF/1.3_B%C4%9B%C5%BEn%C3%A9_grafy/#def-1.4)

### Zadání
Dokažte, že každá orientace úplného grafu $K_n$ (tzv. **turnaj** na $n$ vrcholech) má **nejvýše jedno** topologické uspořádání.

> [!success]- Řešení
> ### Důkaz
> Nechť $T = (V, E)$ je libovolná orientace úplného grafu $K_n$. Mezi každou dvojicí různých vrcholů $u \neq v$ tedy vede právě jedna orientovaná hrana: buď $(u, v) \in E$, nebo $(v, u) \in E$.
> 
> Mohou nastat dva případy:
> 1. **Graf $T$ obsahuje orientovaný cyklus:**
>    Podle [Věty 5.1](https://truepeak78.github.io/ag1/05_Orientovan%C3%A9_grafy/5.1_Z%C3%A1kladn%C3%AD_pojmy/#theorem-5.1) orientovaný graf má topologické uspořádání právě tehdy, když je acyklický (DAG). Pokud $T$ obsahuje cyklus, má přesně **0** topologických uspořádání, což splňuje podmínku „nejvýše jedno“.
> 
> 2. **Graf $T$ je acyklický (DAG):**
>    Předpokládejme, že existuje alespoň jedno platné topologické uspořádání $\pi = (v_1, v_2, \dots, v_n)$.
>    - Z definice topologického uspořádání musí každá orientovaná hrana vést z vrcholu s menším indexem do vrcholu s větším indexem.
>    - Protože mezi každou dvojicí $v_i, v_j$ (pro $i < j$) vede hrana a nemůže vést zprava doleva ($(v_j, v_i) \notin E$), musí nutně vést zleva doprava:
>      $$\forall 1 \le i < j \le n: (v_i, v_j) \in E$$
>    - To znamená, že vrchol na $i$-té pozici v uspořádání $\pi$ má orientované hrany do všech $n - i$ vrcholů, které jsou v uspořádání za ním:
>      $$v_i \to v_{i+1}, \; v_i \to v_{i+2}, \; \dots, \; v_i \to v_n$$
>    - Do vrcholu $v_i$ naopak vedou hrany ze všech $i - 1$ vrcholů před ním.
>    - Výstupní stupeň vrcholu $v_i$ je tedy roven přesně:
>      $$\deg^+(v_i) = n - i$$
>    - Hodnoty výstupních stupňů jsou:
>      $$\deg^+(v_1) = n - 1, \quad \deg^+(v_2) = n - 2, \quad \dots, \quad \deg^+(v_n) = 0$$
>    - Všech $n$ vrcholů má **vzájemně různé výstupní stupně** z množiny $\{0, 1, \dots, n-1\}$.
>    - V jakémkoliv platném topologickém uspořádání musí vrchol s větším výstupním stupněm předcházet vrchol s menším výstupním stupněm (protože mezi nimi vede hrana ze stromu s větším do menšího).
>    - Pozice každého vrcholu $v$ v uspořádání je tedy **jednoznačně fixována** jeho výstupním stupněm:
>      $$\text{pozice}(v) = n - \deg^+(v)$$
>    - Neexistuje žádný stupeň volnosti a nemůže existovat žádné jiné topologické uspořádání.
> 
> Každá orientace úplného grafu má tedy buď 0 (pokud obsahuje cyklus), nebo právě 1 topologické uspořádání (pokud je acyklická, tzv. tranzitivní turnaj). Počet topologických uspořádání je tedy **vždy nejvýše 1**.

---

## Úloha 5.6: Počet topologických uspořádání dvou disjunktních uspořádaných řetězců
* **Zdroj:** FitWiki (Suchý 2017, scan testu `ag1_zapoctak.jpeg`)
* **Relevantní skripta:** [Definice 5.3 (Topologické uspořádání)](https://truepeak78.github.io/ag1/05_Orientovan%C3%A9_grafy/5.1_Z%C3%A1kladn%C3%AD_pojmy/#def-5.3)

### Zadání
Mějme orientovaný graf $G = (V, E)$ o 14 vrcholech, který se skládá ze dvou disjunktních komponent $C_1$ a $C_2$:
- **Komponenta $C_1$** obsahuje 7 vrcholů $V_1 = \{1, 2, 3, 4, 5, 6, 7\}$ a obsahuje hrany $(i, j)$ pro všechna $1 \le i < j \le 7$ (tranzitivní turnaj na 7 vrcholech).
- **Komponenta $C_2$** obsahuje 7 vrcholů $V_2 = \{8, 9, 10, 11, 12, 13, 14\}$ a obsahuje hrany $(i, j)$ pro všechna $8 \le i < j \le 14$ (tranzitivní turnaj na dalších 7 vrcholech).
- Mezi komponentami $C_1$ a $C_2$ nevede **žádná hrana**.

Určete přesný celkový počet všech platných topologických uspořádání grafu $G$. Kombinační čísla vyčíslete.

> [!success]- Řešení
> ### 1. Analýza jednotlivých komponent
> - Uvnitř komponenty $C_1$ vede hrana $(i, j)$ pro každé $i < j$. Podle Úlohy 5.5 má tato komponenta **právě jedno (1)** platné vnitřní topologické uspořádání:
>   $$\pi_1 = (1, 2, 3, 4, 5, 6, 7)$$
> - Uvnitř komponenty $C_2$ vede hrana $(i, j)$ pro každé $i < j$. Komponenta $C_2$ má rovněž **právě jedno (1)** platné vnitřní topologické uspořádání:
>   $$\pi_2 = (8, 9, 10, 11, 12, 13, 14)$$
>
> ### 2. Kombinatorické sloučení (prolínání)
> Topologické uspořádání celého grafu $G$ je libovolná permutace všech $7 + 7 = 14$ vrcholů, která:
> - zachovává relativní pořadí prvků z $V_1$: $1 \prec 2 \prec 3 \prec 4 \prec 5 \prec 6 \prec 7$,
> - zachovává relativní pořadí prvků z $V_2$: $8 \prec 9 \prec 10 \prec 11 \prec 12 \prec 13 \prec 14$.
>
> Protože mezi $V_1$ a $V_2$ neexistují žádné hrany, libovolné proložení (interleaving) těchto dvou posloupností je platným topologickým uspořádáním.
> 
> Ze 14 celkových pozic stačí vybrat 7 pozic, na které umístíme prvky první komponenty (v jejich pevném pořadí $\pi_1$). Zbývajících 7 pozic bude automaticky obsazeno prvky druhé komponenty (v jejich pevném pořadí $\pi_2$).
> 
> ### Výpočet
> Celkový počet topologických uspořádání je dán kombinačním číslem:
> $$\text{Počet} = \binom{14}{7} = \frac{14 \cdot 13 \cdot 12 \cdot 11 \cdot 10 \cdot 9 \cdot 8}{7 \cdot 6 \cdot 5 \cdot 4 \cdot 3 \cdot 2 \cdot 1} = \mathbf{3\,432}.$$
> 
> Graf $G$ má přesně **3 432** různých topologických uspořádání.

---

## Úloha 5.7: Zdroje a soutoky v orientacích úplného bipartitního grafu $K_{n, m}$
* **Zdroj:** FitWiki (Chludil 2017 `61217holub`)
* **Relevantní skripta:** [Definice 5.2 (Zdroje a soutoky v orientovaném grafu)](https://truepeak78.github.io/ag1/05_Orientovan%C3%A9_grafy/5.1_Z%C3%A1kladn%C3%AD_pojmy/#def-5.2) · [Definice 1.4 (Úplný bipartitní graf)](https://truepeak78.github.io/ag1/01_Z%C3%A1klady_graf%C5%AF/1.3_B%C4%9B%C5%BEn%C3%A9_grafy/#def-1.4)

### Zadání
Mějme úplný bipartitní graf $G = K_{n, m}$ s partitami $A$ a $B$, kde $|A| = n \ge 2$ a $|B| = m \ge 2$.
Orientujeme každou hranu grafu $G$ jedním směrem (vznikne orientovaný graf bez smyček a násobných hran).
**Zdroj** je vrchol s nulovým vstupním stupněm ($\deg_{\text{in}}(v) = 0$).
1. Jaký je **největší možný počet zdrojů**, které mohou v takové orientaci vzniknout?
2. Jaký je **nejmenší možný počet zdrojů**, které mohou v takové orientaci vzniknout?
Své odpovědi zdůvodněte a popište orientace dosahující těchto extrémů.

> [!success]- Řešení
> ### 1. Největší možný počet zdrojů
> **Maximální počet zdrojů je $\max(n, m)$.**
> 
> - **Zdůvodnění horního odhadu:**
>   Předpokládejme, že v partitě $A$ existuje alespoň jeden zdroj $u \in A$, a zároveň v partitě $B$ existuje alespoň jeden zdroj $v \in B$.
>   Protože graf $G$ je úplný bipartitní graf, existuje mezi libovolným vrcholem z $A$ a libovolným vrcholem z $B$ hrana. Tedy mezi $u$ a $v$ musí vést hrana:
>   - Je-li orientována $u \to v$, pak $\deg_{\text{in}}(v) \ge 1 \implies v$ nemůže být zdroj!
>   - Je-li orientována $v \to u$, pak $\deg_{\text{in}}(u) \ge 1 \implies u$ nemůže být zdroj!
>   Vzniká přímý spor: **Zdroje nemohou ležet současně v obou partitách.** Všechny zdroje musí nutně ležet buď výhradně v partitě $A$, nebo výhradně v partitě $B$.
>   Maximální počet zdrojů je tedy omezen velikostí větší partity: $\max(|A|, |B|) = \max(n, m)$.
> 
> - **Dosažitelnost maxima:**
>   Bez újmy na obecnosti nechť $n \ge m$. Orientujme **všechny hrany z $A$ do $B$** ($(a, b) \in E$ pro všechna $a \in A, b \in B$).
>   - Pro každý vrchol $a \in A$ platí $\deg_{\text{in}}(a) = 0 \implies$ všech $n$ vrcholů z $A$ jsou zdroje.
>   - Pro každý vrchol $b \in B$ platí $\deg_{\text{in}}(b) = n \ge 2 \implies$ žádný není zdroj.
>   Počet zdrojů je přesně **$n = \max(n, m)$**.
>
> ---
>
> ### 2. Nejmenší možný počet zdrojů
> **Minimální počet zdrojů je $0$.**
> 
> - **Zdůvodnění a konstrukce orientace s 0 zdroji:**
>   Zdroj má $\deg_{\text{in}}(v) = 0$. Aby graf neměl žádný zdroj, musí pro každý vrchol platit $\deg_{\text{in}}(v) \ge 1$.
>   Toho lze snadno dosáhnout vytvořením orientovaného cyklu nebo střídáním orientací:
>   - Zvolme $u_1, u_2 \in A$ a $v_1, v_2 \in B$ (protože $n, m \ge 2$).
>   - Zorientujeme hrany:
>     - $u_1 \to v_1$
>     - $v_1 \to u_2$
>     - $u_2 \to v_2$
>     - $v_2 \to u_1$
>     Tato 4 hrany tvoří orientovaný cyklus $u_1 \to v_1 \to u_2 \to v_2 \to u_1$. Všechny tyto 4 vrcholy mají $\deg_{\text{in}} \ge 1$.
>   - Pro libovolný další vrchol $a \in A \setminus \{u_1, u_2\}$ zavedeme hranu $v_1 \to a$ $\implies \deg_{\text{in}}(a) \ge 1$.
>   - Pro libovolný další vrchol $b \in B \setminus \{v_1, v_2\}$ zavedeme hranu $u_1 \to b$ $\implies \deg_{\text{in}}(b) \ge 1$.
>   - Všechny zbývající hrany zorientujeme libovolně.
>   V této orientaci má každý vrchol grafu vstupní stupeň alespoň 1.
>   Graf neobsahuje **žádný zdroj** (počet zdrojů je $0$).
