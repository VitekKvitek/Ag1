# BI-AG1: Příprava na 1. a 2. zápočtový test (FitWiki & Sylabus)

Tento repozitář obsahuje komplexní a vyčerpávající sbírku **104 autentických i typových zadání** z 1. a 2. zápočtového testu předmětu **BI-AG1 (Algoritmy a grafy 1)** vytěžených z archivu **FitWiki** (včetně reálných zadání z let 2016–2024 a scanů písemek Pokorného, Suchého, Šestákové, Baiera, Knopa, Dvořáka, Huška, Mitury a Cataye). Každé zadání je doplněno o formální, precizní řešení vycházející ze zápisků a sylabu [truepeak78/ag1](https://github.com/truepeak78/ag1).

---

## 📌 Parametry zápočtových testů

Student během semestru píše **dva písemné testy** (celkem za 20 bodů, minimum pro zápočet je 10 bodů):

### 1. Zápočtový test (polovina semestru)
* **Časový limit:** **30 minut**
* **Bodování:** **7 bodů**
* **Rozsah:** Látka přednášek 1–5.
* **Formát:** 2 úlohy:
  1. **Datová struktura (3 body):** **binární halda** v poli (`HeapBuild` odspoda v $O(n)$, `HeapExtractMin`, `HeapInsert`, operace `DecreaseMax(k)`) nebo **binomiální halda** (`BHMerge`, vlastnosti $B_k$, `BHExtractMax`, `IncreaseKey`).
  2. **Důkaz / tvrzení / algoritmus (4 body):** grafové vlastnosti (stupně, Handshaking lemma, acykličnost, komponenty, kostry, TopSort v turnajích) nebo algoritmus v čase $O(|V| + |E|)$ / $O(|V|)$ se zdůvodněním korektnosti a konečnosti.

### 2. Zápočtový test (konec listopadu / začátek prosince)
* **Časový limit:** **50 minut**
* **Bodování:** **13 bodů**
* **Rozsah:** Látka přednášek 1–7 (klade důraz na nová témata 6–7 a pokročilé grafové modely).
* **Formát:** 3 úlohy:
  1. **Datová struktura (4 body):** **AVL strom** (vkládání, mazání se dvěma syny a kaskádováním rotací, Fibonacciho stromy $N(h)$, pravděpodobnost rotací) nebo **Hešovací tabulka** (otevřené adresování, lineární probing s náhrobky $\dagger$, dvojité hešování, modulární řešení kolizí, multiplikativní hešování s rehašováním, řetízky). Zadáno i řešeno v **autentických horizontálních tabulkách**.
  2. **Důkaz / teoretická úvaha (4 body):** pokročilé grafové důkazy ($P_3, P_4, C_4$, samodoplňkové cesty $P_n \cong \overline{P_n}$, liché kružnice, $2n$ vrcholů stupně $n$, Hellyho vlastnost podstromů, červený list v bipartitním stromu, komplement podgrafu bez vrcholu) nebo amortizovaná analýza (penízková a potenciálová metoda pro $2n$ nafukování i smršťování při $\le 1/4$, Eliščino pole).
  3. **Rozšiřující algoritmická úloha (5 bodů):** návrh algoritmu nad stavovým prostorem (Pizzerie s omezením odbočování, líná sytá kočička, křeček ve víceúrovňovém bludišti, sběr klíčů v $V \times \{0,1\}^k$, Dialův algoritmus pro váhy $\{0, 300, 500\}$, jednoznačnost TopSortu).

---

## 📂 Přehled kapitol a úloh (Celkem 104 úloh)

Všechny úlohy jsou roztříděny do tematických `.md` souborů s podrobným krokováním, diagramy, horizontálními tabulkami a rigorózními důkazy:

### 🟢 1. Zápočtový test (Kapitoly 01–06, celkem 47 úloh)

| Soubor | Úloh | Téma | Klíčové pojmy a operace |
| :--- | :---: | :--- | :--- |
| [**`01_binarni_haldy.md`**](./01_binarni_haldy.md) | **6** | **Binární haldy** | `HeapBuild` (in-place $O(n)$ odspoda), `HeapExtractMin`, `HeapInsert`, indexace pole ($2i, 2i+1$), seřazené pole vs. halda, implementace `DecreaseMax(k)` v $O(n)$ a $O(\log n)$. |
| [**`02_binomialni_haldy.md`**](./02_binomialni_haldy.md) | **6** | **Binomiální haldy** | Binomiální strom $B_k$ (stupně, hladiny, počet vrcholů $\binom{k}{d}$, rozbor $B_{36}$), `BHMerge`, `BHExtractMin`, `BHExtractMax`, `IncreaseKey` v $O(\log n)$. |
| [**`03_stupne_grafy_indukce.md`**](./03_stupne_grafy_indukce.md) | **11** | **Stupně, grafy a indukce** | Princip sudosti, existence $K_3$, izomorfismus cesty a doplňku ($P_1, P_4$), automorfismy úplných bloků, extrémy hran při zadaných komponentách (11 vrcholů, 5 komponent), graf s méně hranami než vrcholy, klasifikace 11 neizomorfních grafů s 10 vrcholy a 41 hranami. |
| [**`04_souvislost_komponenty_stromy.md`**](./04_souvislost_komponenty_stromy.md) | **10** | **Souvislost a stromy** | Extrémy hran v komponentách, acyklické grafy, přidání hrany do stromu, kostry s hranami, listy stromu (min 2, max $n-1$), Hellyho vlastnost podstromů, červený list v bipartitním stromu, rozdělení stromu o 50 hranách, počet koster v grafu s kružnicemi a mosty ($3 \times 5 = 15$). |
| [**`05_orientovane_grafy_topsort.md`**](./05_orientovane_grafy_topsort.md) | **7** | **DAGy a Topologické uspořádání** | Jednoznačnost TopSort v turnaji (každá orientace $K_n$ má $\le 1$ uspořádání), $\ge 2$ uspořádání v DAGu bez Hamiltonovské cesty, král v turnaji (vzdálenost $\le 2$), prolínání dvou disjunktních turnajů ($\binom{14}{7} = 3432$), extrémy zdrojů v $K_{n,m}$. |
| [**`06_algoritmy_na_grafech_a_stromech.md`**](./06_algoritmy_na_grafech_a_stromech.md) | **7** | **Algoritmické úlohy $O(V+E)$ / $O(n)$** | Detekce kružnice v čase $O(|V|)$ (rychlý test $|E| \ge |V|$), rozpoznání unicyklického grafu („strom + 1 hrana“) v $O(|V|+|E|)$, prefixové uspořádání stromu s $\le 1$ sousedem v $O(n)$, existence souvislého indukovaného podgrafu velikosti $k$, jednoznačný střed stromu. |

### 🔵 2. Zápočtový test (Kapitoly 07–11, celkem 57 úloh)

| Soubor | Úloh | Téma | Klíčové pojmy a operace |
| :--- | :---: | :--- | :--- |
| [**`07_bvs_a_avl_stromy.md`**](./07_bvs_a_avl_stromy.md) | **10** | **BVS a AVL stromy** | Rotace LL, RR, LR, RL, `AVLInsert`, `AVLDelete` s kaskádováním vyvážení a mazáním uzlu se dvěma syny, minimální počet uzlů $N(h)$ (Fibonacciho stromy), pravděpodobnost a střední hodnota počtu rotací (Catay 2024 `grafff.png`), důkaz návštěvy předchůdce i následníka při vkládání do BVS, konstrukce dokonale vyváženého BVS ze seřazeného pole v $O(n)$, rozdělení BVS v $O(n)$. |
| [**`08_hesovani_a_hesovaci_tabulky.md`**](./08_hesovani_a_hesovaci_tabulky.md) | **13** | **Hešování a hešovací tabulky** | Otevřené adresování, lineární probing a náhrobky $\dagger$ (Pokorný 2021, Suchý 2017, Catay 2024), dvojité hešování a počítání kolizí, vkládání duplikátů přes náhrobky (Marast 2022), modulární řešení kolizí ($f(k, 2) = 5$), multiplikativní hešování metodou vyšších bitů součinu s dynamickým rehašováním, separátní řetězení (řetízky), vkládání řetězců (Malík 2018), počet platných posloupností vkládání přes DAG a prolínání shluků ($\binom{7}{3} \times 2 = 70$), teorie shlukování, faktor $\alpha$, narozeninový paradox. Zadáno i řešeno v **horizontálních tabulkách**. |
| [**`09_amortizovana_analyza_a_dynamicke_pole.md`**](./09_amortizovana_analyza_a_dynamicke_pole.md) | **7** | **Amortizovaná analýza** | Nafukovací pole s realokací $2n$, účetní (penízková) metoda, aditivní zvětšování ($n+c$) vs. geometrické ($\beta n$), zmenšování pole a prevence thrashingu (hystereze při $\le 1/4$), binární čítač, střední hodnota posunů v „Eliščině poli“ ($\Theta(n^2)$ celkem, $\Theta(n)$ na operaci), analýza cyklů s exponenciálním tiskem hvězdiček $\sum 2^i = 2^{n+1}-2$, potenciálová metoda $\Phi = 2k - s$ pro obousměrné dynamické pole (Push & Pop v $O(1)$). |
| [**`10_pokrocile_grafove_dukazy.md`**](./10_pokrocile_grafove_dukazy.md) | **17** | **Pokročilé grafové důkazy** | Grafy bez indukovaného $P_4, C_4$ a existence univerzálního vrcholu, indukovaná $P_3$ v neúplném souvislém grafu, souvislost přes podgrafy $\{G - v\}$, cesty v bipartitním grafu $K_{n, 5}$, existence kostry obsahující libovolnou hranu $e \in E(G)$, paritní spor turnaje 26 týmů (Handshaking lemma), komise KTI jako úplný graf $K_8$, samodoplňkové cesty $P_n \cong \overline{P_n}$ ($n=1, 4$), podgraf lichá kružnice $\implies$ indukovaná lichá kružnice, 2 liché vrcholy leží v téže komponentě, graf o $2n$ vrcholech s $\delta(G) \ge n$ je souvislý s průměrem $\le 2$, existence trojúhelníku při $\deg(v) > n/2$, Hellyho vlastnost podstromů, červený list v bipartitním stromu, množinová identita $\overline{G - v} = \overline{G} - v$, cesta délky $k$ v $k$-regulárním grafu. |
| [**`11_stavove_grafy_a_pokrocile_algoritmy.md`**](./11_stavove_grafy_a_pokrocile_algoritmy.md) | **10** | **Stavové grafy a pokročilé algoritmy** | Stavový prostor $V \times \{0,1\}^3$ (sběr 3 klíčů a odemykání hran), váhy $\{0, 300, 500\}$ s limitem $\le 1000$ (Dialův algoritmus / 0-1 BFS), cesta s $\le 5$ po sobě jdoucími vnějšími vrcholy, počet cest v DAGu v $O(V+E)$, post-order součet na binárním stromu v $O(n)$, pizzerie se zákazem odbočení vlevo v $V \times E$, líná sytá kočička v $V \times \{0, 1\}$, křeček ve víceúrovňovém bludišti se skluzavkami dolů, nejkratší sled přes 8 vrcholů v $V \times \mathcal{P}(A)$ o 256 vrstvách, jednoznačnost topologického uspořádání a míchání chemikálií. |

---

## 🔗 Rychlé odkazy na klíčové definice a věty sylabu

Názvosloví a značení v řešeních striktně odpovídá přednáškám BI-AG1 a zápiskům na [truepeak78.github.io/ag1](https://truepeak78.github.io/ag1/):

- **Základy grafů a souvislost:**
  - [Definice 1.1 (Neorientovaný graf)](https://truepeak78.github.io/ag1/01_Z%C3%A1klady_graf%C5%AF/1.1_Z%C3%A1kladn%C3%AD_pojmy/#def-1.1)
  - [Věta 1.1 (Princip sudosti – Handshaking Lemma)](https://truepeak78.github.io/ag1/01_Z%C3%A1klady_graf%C5%AF/1.1_Z%C3%A1kladn%C3%AD_pojmy/#theorem-1.1)
  - [Definice 1.3 (Sled, tah, cesta)](https://truepeak78.github.io/ag1/01_Z%C3%A1klady_graf%C5%AF/1.1_Z%C3%A1kladn%C3%AD_pojmy/#def-1.3)
  - [Algoritmus 1.1 a Věta 1.2 (BFS a jeho korektnost)](https://truepeak78.github.io/ag1/01_Z%C3%A1klady_graf%C5%AF/1.2_BFS/#algo-1.1)
  - [Definice 2.1 a 2.2 (Souvislost a komponenty)](https://truepeak78.github.io/ag1/02_Souvislost_graf%C5%AF/2.1_Souvislost_graf%C5%AF/#def-2.1)
  - [Definice 2.5 (Izomorfismus a automorfismus)](https://truepeak78.github.io/ag1/02_Souvislost_graf%C5%AF/2.3_Izomorfismus/#def-2.5)
- **Stromy a TopSort:**
  - [Definice 3.1 a Věta 3.2 (Stromy a existence listů)](https://truepeak78.github.io/ag1/03_Stromy/3.1_Z%C3%A1kladn%C3%AD_pojmy/#def-3.1)
  - [Věta 3.3 (Ekvivalentní charakterizace stromů)](https://truepeak78.github.io/ag1/03_Stromy/3.2_Charakterizace_strom%C5%AF/#theorem-3.3)
  - [Definice 3.3 a Věta 3.4 (Kostra grafu)](https://truepeak78.github.io/ag1/03_Stromy/3.3_Kostra_grafu/#def-3.3)
  - [Definice 3.4 a Věta 3.7 (Topologické uspořádání a acykličnost)](https://truepeak78.github.io/ag1/03_Stromy/3.4_Topsort/#def-3.4)
- **Haldy a amortizace:**
  - [Definice 4.1 a 4.4 (Binární halda a reprezentace v poli)](https://truepeak78.github.io/ag1/04_Bin%C3%A1rn%C3%AD_haldy/4.1_Halda_jako_struktura/#def-4.1)
  - [Algoritmus 4.3 a Věta 4.5 (HeapBuild v lineárním čase)](https://truepeak78.github.io/ag1/04_Bin%C3%A1rn%C3%AD_haldy/4.5_Heap_Build_a_Sort/#algo-4.3)
  - [Definice 4.4 a Věta 4.7 (Amortizovaná analýza nafukovacího pole)](https://truepeak78.github.io/ag1/04_Bin%C3%A1rn%C3%AD_haldy/4.6_Amortizovan%C3%A1_%C4%8Dasov%C3%A1_slo%C5%BEitost/#def-4.7)
  - [Definice 5.1 a Věta 5.1 (Binomiální stromy $B_k$)](https://truepeak78.github.io/ag1/05_Binomi%C3%A1ln%C3%AD_haldy/5.1_Binomi%C3%A1ln%C3%AD_stromy/#def-5.1)
  - [Definice 5.2 a Algoritmus 5.1 (Binomiální halda a sloučení BHMerge)](https://truepeak78.github.io/ag1/05_Binomi%C3%A1ln%C3%AD_haldy/5.2_Binomi%C3%A1ln%C3%AD_halda/#def-5.2)
- **BVS a AVL stromy:**
  - [Definice 6.1 (Binární vyhledávací strom)](https://truepeak78.github.io/ag1/06_BVS_a_vyva%C5%BEov%C3%A1n%C3%AD/6.1_%C3%9Avod_do_BVS/#def-6.1)
  - [Algoritmus 6.3 a 6.6 (BVSPred, BVSSucc a BVSDelete)](https://truepeak78.github.io/ag1/06_BVS_a_vyva%C5%BEov%C3%A1n%C3%AD/6.2_Operace_BVS/#algo-6.3)
  - [Definice 6.3 (Hloubkově vyvážený BVS – AVL)](https://truepeak78.github.io/ag1/06_BVS_a_vyva%C5%BEov%C3%A1n%C3%AD/6.4_AVL_rotace/#def-6.3)
  - [Věta 6.2 (O hloubce AVL stromů – Fibonacciho stromy)](https://truepeak78.github.io/ag1/06_BVS_a_vyva%C5%BEov%C3%A1n%C3%AD/6.4_AVL_rotace/#theorem-6.2)
  - [Algoritmus 6.7 a Kapitola 6.5 (Rotace, AVLInsert a AVLDelete)](https://truepeak78.github.io/ag1/06_BVS_a_vyva%C5%BEov%C3%A1n%C3%AD/6.4_AVL_rotace/#algo-6.7)
- **Hešování a pravděpodobnost:**
  - [Definice 7.1 (Slovník a hešovací tabulka)](https://truepeak78.github.io/ag1/07_Randomizace_a_he%C5%A1ov%C3%A1n%C3%AD/7.1_Slovn%C3%ADky_a_He%C5%A1ovac%C3%AD_tabulky/#def-7.1)
  - [Algoritmy 7.2 a 7.3 (OpenDelete, OpenInsert a náhrobky)](https://truepeak78.github.io/ag1/07_Randomizace_a_he%C5%A1ov%C3%A1n%C3%AD/7.3_%C5%98e%C5%A1en%C3%AD_koliz%C3%AD/#algo-7.2)
  - [Věta 7.1 (O efektivitě otevřené adresace – vzorce pro $\alpha$)](https://truepeak78.github.io/ag1/07_Randomizace_a_he%C5%A1ov%C3%A1n%C3%AD/7.4_Volba_vyhled%C3%A1vac%C3%AD_posloupnosti/#theorem-7.1)
  - [Definice 7.7 a Věta 7.2 (Střední hodnota a linearita)](https://truepeak78.github.io/ag1/07_Randomizace_a_he%C5%A1ov%C3%A1n%C3%AD/7.5_Pravd%C4%9Bpodobnostn%C3%AD_prostor/#def-7.7)

---

## 💡 Důležité zásady a edge-cases na písemku

1. **AVL rotace:**
   - Při vkládání (`AVLInsert`) stačí k obnovení vyvážení v celém stromu **nejvýše jedna rotace** (jednoduchá nebo dvojitá). Jakmile se provede, výška podstromu klesne na původní a kaskádování se zastaví.
   - Při mazání (`AVLDelete`) se naopak výška podstromu zmenší, což může způsobit nevyváženost u předka! Rotace proto **mohou kaskádovat až do kořene** (až $O(\log n)$ rotací).
2. **Hešování a náhrobky:**
   - Při `Delete` v otevřené adresaci se buňka **nikdy nepřepisuje na `EMPTY`**! Musí se označit jako `TOMBSTONE`. Jinak by vyhledávání dalších prvků skončilo předčasně na `EMPTY`.
   - Při `Insert` si ukládáme první nalezený `TOMBSTONE`, ale pokračujeme ve vyhledávání až k `EMPTY`, abychom ověřili, že vkládaný klíč už v tabulce neleží.
   - U dvojitého hešování $h_2(k)$ nesmí vracet 0 a musí platit $\gcd(h_2(k), m) = 1$.
3. **Amortizace a dynamické pole:**
   - Zdvojnásobení kapacity ($2n$) dává $O(1)$ amortizovanou cenu, protože intervaly mezi realokacemi rostou geometricky a střádají dostatek mincí (kredit 3 mince na prvek).
   - Zmenšování pole se musí provádět až při poklesu na $1/4$ kapacity (hystereze). Zmenšování při $1/2$ vede k patologickému kmitání (thrashing) s amortizovanou cenou $\Theta(n)$.
4. **Stavové grafy v úlohách s BFS:**
   - Kdykoliv pohyb v grafu závisí na doplňkovém stavu (např. sebrané klíče, zbývající peníze, počet vnějších vrcholů za sebou, stav nasycení), **neupravujte vnitřek algoritmu BFS**, ale **zkonstruujte stavový graf $G' = V \times S$**!
   - Na hotový stavový graf $G'$ pak aplikujte standardní přednáškový BFS, čímž máte automaticky garantován důkaz konečnosti a optimality nejkratší cesty.
