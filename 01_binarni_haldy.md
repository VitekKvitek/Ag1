# 01. Binární haldy – Zadání z písemek a řešení

Tento dokument obsahuje typové i archivní úlohy z 1. zápočtového testu BI-AG1 zaměřené na **binární minimové haldy**.

---

## Úloha 1.1: HeapBuild a HeapExtractMin na zadaném poli

* **Zdroj:** FitWiki (4.11.2025, 3.11.2025, 2.12.2024, Scholtzová 2020)
* **Relevantní skripta:** [Algoritmus 4.3 a Věta 4.5 (HeapBuild v čase O(n))](https://truepeak78.github.io/ag1/04_Bin%C3%A1rn%C3%AD_haldy/4.5_Heap_Build_a_Sort/#algo-4.3) · [Algoritmus 4.2 (HeapExtractMin a BubbleDown)](https://truepeak78.github.io/ag1/04_Bin%C3%A1rn%C3%AD_haldy/4.3_Operace_na_Hald%C4%9B/#algo-4.1) · [Definice 4.4 (Reprezentace haldy v poli)](https://truepeak78.github.io/ag1/04_Bin%C3%A1rn%C3%AD_haldy/4.4_Reprezentace_haldy_v_pam%C4%9Bti/#def-4.4)

### Zadání
Mějme pole $A$ o 9 prvcích (indexováno od 1 do 9):
$$A = [29, 15, 8, 14, 9, 21, 14, 22, 8]$$

1. Proveďte nad tímto polem algoritmus **HeapBuild** pro minimovou haldu. Uveďte stav pole a stromové struktury po dokončení HeapBuild.
2. Následně proveďte operaci **HeapExtractMin**. Uveďte výsledné pole a strom.

> [!success]- Řešení
> ### 1. HeapBuild (in-place stavba haldy v čase $O(n)$)
> V binární haldě o $n=9$ prvcích indexované od 1 jsou synové prvku na indexu $i$ na pozicích $2i$ (levý) a $2i+1$ (pravý).
> Listy jsou na indexech $\lfloor n/2 \rfloor + 1 = 5$ až $9$. Listy již tvoří triviální 1-prvkové haldy.
> Iterujeme od $i = \lfloor 9/2 \rfloor = 4$ dolů k $1$ a voláme `BubbleDown(i)`:
>
> - **Krok $i = 4$:** Prvek $A[4] = 14$. Synové jsou na indexech $2 \cdot 4 = 8$ ($A[8] = 22$) a $2 \cdot 4 + 1 = 9$ ($A[9] = 8$).
>   - Menší syn je $A[9] = 8 < 14$.
>   - Prohodíme $A[4]$ a $A[9]$.
>   - Stav pole: $[29, 15, 8, \mathbf{8}, 9, 21, 14, 22, \mathbf{14}]$
>
> - **Krok $i = 3$:** Prvek $A[3] = 8$. Synové jsou na indexech $6$ ($A[6] = 21$) a $7$ ($A[7] = 14$).
>   - Minimální ze synů je $14 > 8$. Haldová podmínka pro podstrom splněna, žádný swap.
>   - Stav pole: beze změny.
>
> - **Krok $i = 2$:** Prvek $A[2] = 15$. Synové jsou na indexech $4$ ($A[4] = 8$) a $5$ ($A[5] = 9$).
>   - Menší syn je $A[4] = 8 < 15$.
>   - Prohodíme $A[2]$ a $A[4]$: pole $[29, \mathbf{8}, 8, \mathbf{15}, 9, 21, 14, 22, 14]$.
>   - Pokračujeme `BubbleDown` z pozice $4$: synové jsou indexy $8$ ($A[8]=22$) a $9$ ($A[9]=14$).
>   - Menší syn je $A[9] = 14 < 15$. Prohodíme $A[4]$ a $A[9]$.
>   - Stav pole: $[29, 8, 8, \mathbf{14}, 9, 21, 14, 22, \mathbf{15}]$
>
> - **Krok $i = 1$:** Prvek $A[1] = 29$. Synové jsou na indexech $2$ ($A[2]=8$) a $3$ ($A[3]=8$).
>   - Oba synové mají hodnotu 8. Zvolíme levého syna (index 2): prohodíme $A[1]$ a $A[2]$.
>   - Pole: $[\mathbf{8}, \mathbf{29}, 8, 14, 9, 21, 14, 22, 15]$.
>   - Bubláme z indexu 2: synové jsou indexy 4 ($A[4]=14$) a 5 ($A[5]=9$).
>   - Menší syn je $A[5] = 9 < 29$. Prohodíme $A[2]$ a $A[5]$.
>   - Pole: $[8, \mathbf{9}, 8, 14, \mathbf{29}, 21, 14, 22, 15]$.
>   - Index 5 nemá žádné syny ($2 \cdot 5 = 10 > 9$). Konec `BubbleDown`.
>
> **Výsledek HeapBuild:**
> - Pole: `[8, 9, 8, 14, 29, 21, 14, 22, 15]`
> - Stromová reprezentace:
>   - Kořen: $8$ (index 1)
>   - Hladina 1: levý syn $9$ (index 2), pravý syn $8$ (index 3)
>   - Hladina 2: synové 9 jsou $14$ (index 4) a $29$ (index 5); synové 8 jsou $21$ (index 6) a $14$ (index 7)
>   - Hladina 3: synové 14 (index 4) jsou $22$ (index 8) a $15$ (index 9)
>
> ---
>
> ### 2. HeapExtractMin
> 1. Kořen $A[1] = 8$ je minimální prvek. Uložíme si ho jako výstup.
> 2. Poslední prvek haldy $A[9] = 15$ přesuneme do kořene $A[1]$, velikost haldy zmenšíme na $n=8$.
>    - Dočasné pole: $[15, 9, 8, 14, 29, 21, 14, 22]$
> 3. Zavoláme `BubbleDown(1)`:
>    - Pozice 1 ($15$): synové $A[2] = 9$, $A[3] = 8$. Menší syn je $A[3] = 8 < 15$.
>    - Prohodíme $A[1]$ a $A[3]$: $[\mathbf{8}, 9, \mathbf{15}, 14, 29, 21, 14, 22]$.
>    - Pozice 3 ($15$): synové jsou $A[6] = 21$ a $A[7] = 14$. Menší syn je $A[7] = 14 < 15$.
>    - Prohodíme $A[3]$ a $A[7]$: $[8, 9, \mathbf{14}, 14, 29, 21, \mathbf{15}, 22]$.
>    - Pozice 7 nemá syny ($2 \cdot 7 = 14 > 8$). Hotovo.
>
> **Výsledné pole po HeapExtractMin:**
> `[8, 9, 14, 14, 29, 21, 15, 22]`

---

## Úloha 1.2: Vkládání prvku (HeapInsert) do existující haldy

* **Zdroj:** FitWiki (Šestáková 2018)
* **Relevantní skripta:** [Algoritmus 4.1 a Věta 4.3 (HeapInsert a BubbleUp)](https://truepeak78.github.io/ag1/04_Bin%C3%A1rn%C3%AD_haldy/4.3_Operace_na_Hald%C4%9B/#algo-4.1) · [Definice 4.4 (Reprezentace haldy v paměti)](https://truepeak78.github.io/ag1/04_Bin%C3%A1rn%C3%AD_haldy/4.4_Reprezentace_haldy_v_pam%C4%9Bti/#def-4.4)

### Zadání
Mějme pole $A = [4, 33, 6, 90, 33, 32, 31, 91, 90, 89, 50, 33]$ reprezentující minimovou binární haldu ($n=12$).
Vložte do haldy prvek s hodnotou $5$. Výsledek zapište formou pole.

> [!success]- Řešení
> 1. Prvek $x = 5$ vložíme na konec pole na novou pozici $n = 13$:
>    $$A[13] = 5$$
> 2. Provedeme operaci `BubbleUp(13)`:
>    - Index $13$: rodič je na pozici $\lfloor 13/2 \rfloor = 6$.
>      - $A[6] = 32$. Protože $5 < 32$, prohodíme $A[13]$ a $A[6]$.
>      - Pole: $[4, 33, 6, 90, 33, \mathbf{5}, 31, 91, 90, 89, 50, 33, \mathbf{32}]$
>    - Index $6$: rodič je na pozici $\lfloor 6/2 \rfloor = 3$.
>      - $A[3] = 6$. Protože $5 < 6$, prohodíme $A[6]$ a $A[3]$.
>      - Pole: $[4, 33, \mathbf{5}, 90, 33, \mathbf{6}, 31, 91, 90, 89, 50, 33, 32]$
>    - Index $3$: rodič je na pozici $\lfloor 3/2 \rfloor = 1$.
>      - $A[1] = 4$. Protože $5 \ge 4$, haldová podmínka $A[\text{parent}(i)] \le A[i]$ je splněna.
>    - Konec operace.
>
> **Výsledné pole:**
> `[4, 33, 5, 90, 33, 6, 31, 91, 90, 89, 50, 33, 32]`

---

## Úloha 1.3: Seřazené pole vs. minimová binární halda

* **Zdroj:** FitWiki (Šestáková 2018, Sbírka 2021)
* **Relevantní skripta:** [Definice 4.1 (Halda jako struktura – tvarová a haldová podmínka)](https://truepeak78.github.io/ag1/04_Bin%C3%A1rn%C3%AD_haldy/4.1_Halda_jako_struktura/#def-4.1) · [Definice 4.4 (Reprezentace haldy v paměti)](https://truepeak78.github.io/ag1/04_Bin%C3%A1rn%C3%AD_haldy/4.4_Reprezentace_haldy_v_pam%C4%9Bti/#def-4.4)

### Zadání
Rozhodněte a dokažte nebo vyvraťte:
*„Máme-li na vstupu vzestupně seřazené pole $A[1 \dots n]$ (tedy $A[1] \le A[2] \le \dots \le A[n]$), pak toto pole vždy reprezentuje binární minimovou haldu.“*

> [!success]- Řešení
> **Tvrzení PLATÍ.**
>
> **Důkaz:**
> Pro to, aby pole $A[1 \dots n]$ reprezentovalo binární minimovou haldu, musí splňovat dvě podmínky:
> 1. **Tvarová podmínka:** Je splněna automaticky, protože prvky jsou uloženy na indexech $1, \dots, n$ v poli, což odpovídá téměř úplnému binárnímu stromu plněnému po hladinách zleva doprava ([Definice 4.4](https://truepeak78.github.io/ag1/04_Bin%C3%A1rn%C3%AD_haldy/4.4_Reprezentace_haldy_v_pam%C4%9Bti/#def-4.4)).
> 2. **Haldová podmínka (pro min-haldu):** Pro každý vrchol na indexu $i$ a jeho existující potomky na indexech $2i$ a $2i+1$ musí platit:
>    $$A[i] \le A[2i] \quad \text{a} \quad A[i] \le A[2i+1]$$
>
> Jelikož pro každé $i \ge 1$ platí $i < 2i < 2i+1$, a pole $A$ je vzestupně seřazené (tzn. $j < k \implies A[j] \le A[k]$), nutně platí:
> $$A[i] \le A[2i] \quad \text{a zároveň} \quad A[i] \le A[2i+1].$$
> Haldová podmínka je tedy splněna pro všechny uzly. Pole $A$ je validní binární minimová halda. $\blacksquare$

---

## Úloha 1.4: Obrácené tvrzení – Je každá binární halda seřazeným polem?

* **Zdroj:** FitWiki (doplňující častý chyták z ústních/písemek)
* **Relevantní skripta:** [Definice 4.1 (Halda jako struktura)](https://truepeak78.github.io/ag1/04_Bin%C3%A1rn%C3%AD_haldy/4.1_Halda_jako_struktura/#def-4.1) · [Vlastnosti haldy – uspořádání na cestách](https://truepeak78.github.io/ag1/04_Bin%C3%A1rn%C3%AD_haldy/4.2_Vlastnosti_haldy/)

### Zadání
Rozhodněte a dokažte nebo vyvraťte:
*„Každé pole reprezentující binární minimovou haldu je vzestupně seřazené.“*

> [!success]- Řešení
> **Tvrzení NEPLATÍ (vyvrácení protipříkladem).**
>
> **Protipříklad:**
> Uvažujme pole $A = [1, 5, 2]$ pro $n=3$:
> - Kořen je $A[1] = 1$.
> - Levý syn je $A[2] = 5 \ge A[1]$.
> - Pravý syn je $A[3] = 2 \ge A[1]$.
> Haldová podmínka platí ($1 \le 5$ i $1 \le 2$). Pole reprezentuje korektní binární minimovou haldu.
> Pole však není seřazené, protože $A[2] = 5 > A[3] = 2$.
> *(Halda garantuje pouze uspořádání podél větví od kořene k listům, nikoliv mezi sourozenci či nesouvisejícími podstromy).*


---

## Úloha 1.5: Operace DecreaseMax(k) a HeapExtractMin v binární minimové haldě
* **Zdroj:** FitWiki (Šestáková 2019, scan testu `test_ag1_sestakova_29_11_2019_18_00_2.jpg`)
* **Relevantní skripta:** [Sekce 4.4 (Reprezentace haldy v poli)](https://truepeak78.github.io/ag1/04_Bin%C3%A1rn%C3%AD_haldy/4.4_Reprezentace_haldy_v_pam%C4%9Bti/#def-4.4) · [Sekce 4.5 (Operace nad binární haldou)](https://truepeak78.github.io/ag1/04_Bin%C3%A1rn%C3%AD_haldy/4.5_Operace_nad_haldou/)

### Zadání
Uvažujte následující 8prvkové pole reprezentující binární minimovou haldu (indexováno od 1):
$$A = [3, 4, 6, 5, 6, 40, 31, 7]$$

1. Proveďte nad touto haldou operaci `HeapExtractMin`. Výsledek zapište formou pole.
2. Popište, jak byste implementovali operaci `DecreaseMax(k)`, která sníží hodnotu **maxima** v binární minimové haldě na zadanou novou hodnotu $k$ tak, aby halda zůstala po provedení validní binární minimovou haldou.
   - Jaká bude asymptotická časová složitost této operace?
   - Ukažte průběh volání operace `DecreaseMax(1)` nad původním polem $A$ (nezávisle na kroku 1) a zapište výsledné pole.

> [!success]- Řešení
> ### 1. Operace `HeapExtractMin` nad polem $A = [3, 4, 6, 5, 6, 40, 31, 7]$
> 1. Minimum je v kořeni: $A[1] = 3$.
> 2. Poslední prvek haldy $A[8] = 7$ přesuneme do kořene a zmenšíme velikost haldy na 7:
>    $$A = [7, 4, 6, 5, 6, 40, 31]$$
> 3. Spustíme `BubbleDown(1)` (probublávání dolů):
>    - Kořen na indexu 1 (hodnota 7) má syny na indexu 2 ($A[2]=4$) a indexu 3 ($A[3]=6$).
>    - Menší ze synů je $A[2] = 4$. Protože $4 < 7$, prohodíme $A[1]$ a $A[2]$:
>      $$A = [4, 7, 6, 5, 6, 40, 31]$$
>    - Nyní jsme na indexu 2 (hodnota 7). Jeho synové jsou na indexu 4 ($A[4]=5$) a indexu 5 ($A[5]=6$).
>    - Menší ze synů je $A[4] = 5$. Protože $5 < 7$, prohodíme $A[2]$ a $A[4]$:
>      $$A = [4, 5, 6, 7, 6, 40, 31]$$
>    - Nyní jsme na indexu 4 (hodnota 7). Jeho potenciální synové by byli na indexech 8 a 9, ale velikost haldy je 7. Uzel je tedy listem, probublávání končí.
> 
> **Výsledné pole po `HeapExtractMin`:**
> $$A = \mathbf{[4, 5, 6, 7, 6, 40, 31]}$$
>
> ---
>
> ### 2. Implementace operace `DecreaseMax(k)`
> #### Kde se nachází maximum v minimové haldě?
> V minimové binární haldě platí, že hodnota v každém rodiči je menší nebo rovna hodnotám jeho potomků. Z toho plyne, že **maximum se nutně musí nacházet v některém z listů haldy**.
> V poli velikosti $n$ (při 1-based indexaci) mají listy indexy:
> $$\text{Listy} = \left\{ \lfloor n/2 \rfloor + 1, \, \lfloor n/2 \rfloor + 2, \, \dots, \, n \right\}$$
> Počet listů je $\lceil n/2 \rceil = \Theta(n)$.
>
> #### Algoritmus:
> 1. **Nalezení maxima:** Lineárním průchodem projdeme všechny indexy listů $i \in \{ \lfloor n/2 \rfloor + 1, \dots, n \}$ a najdeme index $i_{\max}$, na kterém je uložena maximální hodnota $A[i_{\max}]$.
> 2. **Snížení hodnoty:** Nastavíme $A[i_{\max}] = k$.
> 3. **Obnova haldové podmínky:**
>    - Protože se hodnota prvku **zmenšila**, nemůže porušit haldovou podmínku vůči svým potomkům (je to list, navíc menší hodnota je vůči potomkům bezpečná).
>    - Může však být nově **menší než její rodič** na indexu $\lfloor i_{\max} / 2 \rfloor$.
>    - Proto zavoláme operaci **`BubbleUp`** (probublávání nahoru) od indexu $i_{\max}$ směrem ke kořeni.
>
> #### Asymptotická složitost:
> - Nalezení maxima mezi listy vyžaduje $\lceil n/2 \rceil$ porovnání $\implies \mathbf{O(n)}$.
> - Provolání `BubbleUp` stoupá po větvi stromu z listu ke kořeni, hloubka stromu je $\lfloor \log_2 n \rfloor \implies \mathbf{O(\log n)}$.
> - Celková časová složitost je tedy:
>   $$T(n) = O(n) + O(\log n) = \mathbf{O(n)}.$$
>
> ---
>
> ### 3. Běh operace `DecreaseMax(1)` nad původním polem $A$
> Původní pole: $A = [3, 4, 6, 5, 6, 40, 31, 7]$, velikost $n = 8$.
> 1. Listy mají indexy $\lfloor 8/2 \rfloor + 1 = 5$ až $8$, tedy indexy $\{5, 6, 7, 8\}$:
>    - $A[5] = 6$
>    - $A[6] = 40$
>    - $A[7] = 31$
>    - $A[8] = 7$
> 2. Maximum je hodnota $40$ na indexu **$i = 6$**.
> 3. Snížíme hodnotu na 1: $A[6] \leftarrow 1$.
>    Stav pole: $[3, 4, 6, 5, 6, \mathbf{1}, 31, 7]$.
> 4. Voláme `BubbleUp(6)`:
>    - Index 6: rodič je na indexu $\lfloor 6/2 \rfloor = 3$, kde leží $A[3] = 6$.
>    - Protože $1 < 6$, prohodíme $A[6]$ a $A[3]$:
>      $$A = [3, 4, \mathbf{1}, 5, 6, 6, 31, 7]$$
>    - Nyní jsme na indexu 3: rodič je na indexu $\lfloor 3/2 \rfloor = 1$, kde leží $A[1] = 3$.
>    - Protože $1 < 3$, prohodíme $A[3]$ a $A[1]$:
>      $$A = [\mathbf{1}, 4, 3, 5, 6, 6, 31, 7]$$
>    - Jsme v kořeni (index 1), probublávání končí.
> 
> **Výsledné pole po `DecreaseMax(1)`:**
> $$A = \mathbf{[1, 4, 3, 5, 6, 6, 31, 7]}$$

---

## Úloha 1.6: HeapBuild odspoda, vkládání a extrakce z písemky Suchý 2017
* **Zdroj:** FitWiki (Suchý 2017, scan testu `ag1_zapoctak.jpeg`, Knop 2023)
* **Relevantní skripta:** [Sekce 4.5 (Operace HeapBuild a Heapify)](https://truepeak78.github.io/ag1/04_Bin%C3%A1rn%C3%AD_haldy/4.5_Operace_nad_haldou/#heapbuild)

### Zadání
V poli je uloženo 10 prvků:
$$A = [13, 9, 19, 21, 10, 10, 14, 6, 26, 27]$$
1. Pomocí operace `HeapBuild` (vybudování haldy v lineárním čase odspoda nahoru pomocí `BubbleDown`) zkonstruujte minimovou binární haldu. Uveďte postup a výsledné pole.
2. Do takto vybudované haldy vložte pomocí `HeapInsert` prvek $8$.
3. Následně z haldy odeberte minimum pomocí `HeapExtractMin`.
4. Vysvětlete, proč `HeapBuild` běží v čase $O(n)$, zatímco postupné vkládání prvků do prázdné haldy pomocí $n$-krát volaného `HeapInsert` běží v čase $O(n \log n)$.

> [!success]- Řešení
> ### 1. Operace `HeapBuild` odspoda
> Velikost pole $n = 10$.
> Listy jsou uzly na indexech $\lfloor 10/2 \rfloor + 1 = 6$ až $10$. Tyto jednoprvkové podstromy již triviálně splňují haldovou podmínku.
> Procházíme vnitřní uzly od indexu $i = \lfloor 10/2 \rfloor = 5$ dolů k $1$ a voláme `BubbleDown(i)`:
> 
> - **Krok $i = 5$ ($A[5] = 10$):**
>   - Synové: $2i = 10 \implies A[10] = 27$.
>   - $10 \le 27$, haldová podmínka v uzlu 5 platí. Beze změny.
> 
> - **Krok $i = 4$ ($A[4] = 21$):**
>   - Synové: $2i = 8 \implies A[8] = 6$, $2i+1 = 9 \implies A[9] = 26$.
>   - Menší syn je $A[8] = 6$. Protože $6 < 21$, prohodíme $A[4]$ a $A[8]$:
>     $$A = [13, 9, 19, \mathbf{6}, 10, 10, 14, \mathbf{21}, 26, 27]$$
> 
> - **Krok $i = 3$ ($A[3] = 19$):**
>   - Synové: $2i = 6 \implies A[6] = 10$, $2i+1 = 7 \implies A[7] = 14$.
>   - Menší syn je $A[6] = 10$. Protože $10 < 19$, prohodíme $A[3]$ a $A[6]$:
>     $$A = [13, 9, \mathbf{10}, 6, 10, \mathbf{19}, 14, 21, 26, 27]$$
> 
> - **Krok $i = 2$ ($A[2] = 9$):**
>   - Synové: $2i = 4 \implies A[4] = 6$, $2i+1 = 5 \implies A[5] = 10$.
>   - Menší syn je $A[4] = 6$. Protože $6 < 9$, prohodíme $A[2]$ a $A[4]$:
>     $$A = [13, \mathbf{6}, 10, \mathbf{9}, 10, 19, 14, 21, 26, 27]$$
>   - Nyní jsme na indexu 4 (hodnota 9). Jeho synové jsou $A[8] = 21$ a $A[9] = 26$. Menší syn je 21. Protože $9 \le 21$, probublávání končí.
> 
> - **Krok $i = 1$ ($A[1] = 13$):**
>   - Synové: $A[2] = 6$, $A[3] = 10$. Menší syn je $A[2] = 6$.
>   - Protože $6 < 13$, prohodíme $A[1]$ a $A[2]$:
>     $$A = [\mathbf{6}, \mathbf{13}, 10, 9, 10, 19, 14, 21, 26, 27]$$
>   - Nyní jsme na indexu 2 (hodnota 13). Synové jsou $A[4] = 9$ a $A[5] = 10$. Menší syn je $A[4] = 9$.
>   - Protože $9 < 13$, prohodíme $A[2]$ a $A[4]$:
>     $$A = [6, \mathbf{9}, 10, \mathbf{13}, 10, 19, 14, 21, 26, 27]$$
>   - Nyní jsme na indexu 4 (hodnota 13). Synové jsou $A[8] = 21$ a $A[9] = 26$. Protože $13 \le 21$, probublávání končí.
> 
> **Výsledné pole po `HeapBuild`:**
> $$A = \mathbf{[6, 9, 10, 13, 10, 19, 14, 21, 26, 27]}$$
>
> ---
>
> ### 2. Vložení prvku 8 (`HeapInsert(8)`)
> 1. Prvek 8 přidáme na konec pole na novou pozici $A[11] = 8$.
> 2. Voláme `BubbleUp(11)`:
>    - Rodič indexu 11 je $\lfloor 11/2 \rfloor = 5$, kde leží $A[5] = 10$.
>    - Protože $8 < 10$, prohodíme $A[11]$ a $A[5]$:
>      $$A = [6, 9, 10, 13, \mathbf{8}, 19, 14, 21, 26, 27, \mathbf{10}]$$
>    - Nyní jsme na indexu 5: rodič je $\lfloor 5/2 \rfloor = 2$, kde leží $A[2] = 9$.
>    - Protože $8 < 9$, prohodíme $A[5]$ a $A[2]$:
>      $$A = [6, \mathbf{8}, 10, 13, \mathbf{9}, 19, 14, 21, 26, 27, 10]$$
>    - Nyní jsme na indexu 2: rodič je index 1, kde leží $A[1] = 6$.
>    - Protože $8 \ge 6$, podmínka platí, probublávání končí.
> 
> **Výsledné pole po `HeapInsert(8)`:**
> $$A = \mathbf{[6, 8, 10, 13, 9, 19, 14, 21, 26, 27, 10]}$$
>
> ---
>
> ### 3. Operace `HeapExtractMin()`
> 1. Minimum je v kořeni: $A[1] = 6$.
> 2. Poslední prvek $A[11] = 10$ přesuneme do kořene a zmenšíme pole na 10 prvků:
>    $$A = [10, 8, 10, 13, 9, 19, 14, 21, 26, 27]$$
> 3. Voláme `BubbleDown(1)`:
>    - Kořen 10: synové jsou $A[2] = 8$ a $A[3] = 10$. Menší je $A[2] = 8$.
>    - Prohodíme $A[1]$ a $A[2]$:
>      $$A = [\mathbf{8}, \mathbf{10}, 10, 13, 9, 19, 14, 21, 26, 27]$$
>    - Index 2 (hodnota 10): synové jsou $A[4] = 13$ a $A[5] = 9$. Menší syn je $A[5] = 9$.
>    - Prohodíme $A[2]$ a $A[5]$:
>      $$A = [8, \mathbf{9}, 10, 13, \mathbf{10}, 19, 14, 21, 26, 27]$$
>    - Index 5 (hodnota 10): syn je $A[10] = 27$. Protože $10 \le 27$, probublávání končí.
> 
> **Výsledné pole po `HeapExtractMin`:**
> $$A = \mathbf{[8, 9, 10, 13, 10, 19, 14, 21, 26, 27]}$$
>
> ---
>
> ### 4. Porovnání složitostí: HeapBuild $O(n)$ vs $n \times$ HeapInsert $O(n \log n)$
> - **HeapBuild odspoda:**
>   - Probublává směrem **dolů**. Většina vrcholů haldy se nachází na nejnižších hladinách:
>     - $\approx n/2$ listů probublává o $0$ pater.
>     - $\approx n/4$ uzlů probublává nanejvýš o $1$ patro.
>     - $\approx n/8$ uzlů probublává nanejvýš o $2$ patra.
>   - Celkový počet kroků je dán sumou:
>     $$\sum_{h=0}^{\lfloor \log_2 n \rfloor} \frac{n}{2^{h+1}} \cdot h = n \sum_{h=0}^{\infty} \frac{h}{2^{h+1}} = n \cdot 1 = \mathbf{\Theta(n)}.$$
> - **Postupné vkládání ($n \times$ HeapInsert):**
>   - Každý prvek se vkládá na konec haldy (do listu) a probublává **nahoru**.
>   - Nejméně polovina všech vkládaných prvků se vkládá do hloubky $\Theta(\log n)$ a může probublat až do kořene.
>   - Celková složitost je $\sum_{i=1}^n \log i = \log(n!) = \mathbf{\Theta(n \log n)}$.
