# 07. Binární vyhledávací stromy (BVS) a AVL stromy – Zadání z písemek a řešení

Tento dokument obsahuje typové i archivní úlohy z 2. zápočtového testu BI-AG1 zaměřené na **binární vyhledávací stromy (BVS)**, **AVL stromy**, jejich rotace, operace vkládání a mazání, extrémy počtu vrcholů a pravděpodobnostní analýzu rotací.

---

## Úloha 7.1: Stavba AVL stromu postupným vkládáním (jednoduché i dvojité rotace)

* **Zdroj:** FitWiki (Knop 2022, 2023, Suchý 2017, Chludil 2017, Marast 2024)
* **Relevantní skripta:** [Definice 6.3 (Hloubkově vyvážený BVS – AVL)](https://truepeak78.github.io/ag1/06_BVS_a_vyva%C5%BEov%C3%A1n%C3%AD/6.4_AVL_rotace/#def-6.3) · [Algoritmus 6.7 (Rotace v AVL stromu)](https://truepeak78.github.io/ag1/06_BVS_a_vyva%C5%BEov%C3%A1n%C3%AD/6.4_AVL_rotace/#algo-6.7) · [Kapitola 6.5 (AVLInsert a oprava vyvážení)](https://truepeak78.github.io/ag1/06_BVS_a_vyva%C5%BEov%C3%A1n%C3%AD/6.5_AVL_operace/)

### Zadání
Do prázdného AVL stromu postupně vložte prvky v následujícím pořadí:
$$10, 20, 30, 25, 27, 5, 4$$
1. Zakreslete stav stromu po každé operaci, při které dojde k porušení AVL podmínky a následné vyvažovací rotaci.
2. U každé rotace uveďte její přesný typ (LL, RR, LR, RL), vrchol, v němž došlo k porušení vyvážení, a kořeny podstromů, které se rotace účastní.
3. Zakreslete výsledný strom včetně vyvažovacích koeficientů (faktorů vyvážení $\text{bf}(v) = h(v.\text{right}) - h(v.\text{left})$) ve všech uzlech.

> [!success]- Řešení
> **Definice faktoru vyvážení:** Pro každý uzel $v$ definujeme $\text{bf}(v) = h(v.\text{right}) - h(v.\text{left})$. V AVL stromu musí platit $\text{bf}(v) \in \{-1, 0, +1\}$.
>
> 1. **Vložení 10, 20:**
>    - Strom: $10 \to$ pravý syn $20$. Vyvážení v pořádku ($\text{bf}(10) = +1, \text{bf}(20) = 0$).
>
> 2. **Vložení 30 $\to$ Porušení v uzlu 10:**
>    - Strom před rotací: $10$ s pravým synem $20$ a pravým vnukem $30$.
>    - Faktory vyvážení: $\text{bf}(10) = +2$, $\text{bf}(20) = +1$.
>    - **Rotace RR (jednoduchá levá rotace v uzlu 10):**
>      - Uzel $20$ se stává novým kořenem.
>      - Uzel $10$ se stává levým synem $20$, uzel $30$ zůstává pravým synem $20$.
>      - Faktory po rotaci: $\text{bf}(20) = 0, \text{bf}(10) = 0, \text{bf}(30) = 0$.
>
> 3. **Vložení 25:**
>    - Vložen jako levý syn uzlu $30$.
>    - Faktory vyvážení: $\text{bf}(30) = -1$, $\text{bf}(20) = 2 - 1 = +1$, $\text{bf}(10) = 0$. Vše v mezích $\{-1, 0, +1\}$.
>
> 4. **Vložení 27 $\to$ Porušení v uzlu 30:**
>    - Cesta vkládání: $20 \to 30 \to 25 \to 27$ (pravý syn, pravý vnuk $25$, jeho pravý syn $27$).
>    - Faktory vyvážení odspodu: $\text{bf}(25) = +1$, $\text{bf}(30) = h(30.\text{right}) - h(30.\text{left}) = 0 - 2 = -2$.
>    - V uzlu $30$ došlo k porušení vyvážení ($\text{bf}(30) = -2, \text{bf}(25) = +1$).
>    - **Rotace LR (dvojitá levo-pravá rotace v uzlu 30):**
>      - Krok 1 (levá rotace levého syna 25): uzel $27$ stoupne nad $25$ ($25$ je levý syn $27$).
>      - Krok 2 (pravá rotace uzlu 30): uzel $27$ nahradí uzel $30$. Levý syn $27$ je $25$, pravý syn $27$ je $30$.
>      - Tento podstrom s kořenem $27$ je připojen jako pravý syn uzlu $20$.
>      - Hloubka pravého podstromu uzlu $20$ klesla zpět na 2.
>      - Faktory po rotaci: $\text{bf}(25) = 0, \text{bf}(30) = 0, \text{bf}(27) = 0, \text{bf}(20) = 2 - 1 = +1$.
>
> 5. **Vložení 5:**
>    - Vložen jako levý syn uzlu $10$.
>    - Faktory vyvážení: $\text{bf}(10) = -1$, $\text{bf}(20) = 2 - 2 = 0$.
>
> 6. **Vložení 4 $\to$ Porušení v uzlu 10:**
>    - Uzel $4$ vložen jako levý syn uzlu $5$.
>    - Faktory vyvážení: $\text{bf}(5) = -1$, $\text{bf}(10) = -2$.
>    - **Rotace LL (jednoduchá pravá rotace v uzlu 10):**
>      - Uzel $5$ se stává kořenem levého podstromu uzlu $20$.
>      - Uzel $4$ je levým synem $5$, uzel $10$ je pravým synem $5$.
>      - Faktory po rotaci: $\text{bf}(4) = 0, \text{bf}(10) = 0, \text{bf}(5) = 0$.
>
> **Výsledná struktura AVL stromu:**
> - **Kořen:** $20$ ($\text{bf} = 0$)
>   - **Levý podstrom (kořen 5, $\text{bf} = 0$):**
>     - Levý syn: $4$ ($\text{bf} = 0$)
>     - Pravý syn: $10$ ($\text{bf} = 0$)
>   - **Pravý podstrom (kořen 27, $\text{bf} = 0$):**
>     - Levý syn: $25$ ($\text{bf} = 0$)
>     - Pravý syn: $30$ ($\text{bf} = 0$)
> Strom je dokonale vyvážený s hloubkou 2 a všemi faktory vyvážení rovnými 0.

---

## Úloha 7.2: Mazání z AVL stromu (AVLDelete s kaskádováním vyvážení)

* **Zdroj:** FitWiki (Knop 2022, Suchý 2017, Chludil 2017)
* **Relevantní skripta:** [Algoritmus 6.6 (BVSDelete)](https://truepeak78.github.io/ag1/06_BVS_a_vyva%C5%BEov%C3%A1n%C3%AD/6.2_Operace_BVS/#algo-6.6) · [Kapitola 6.5 (AVLDelete a oprava vyvážení)](https://truepeak78.github.io/ag1/06_BVS_a_vyva%C5%BEov%C3%A1n%C3%AD/6.5_AVL_operace/) · [Věta 6.3 (Složitost AVLInsert a AVLDelete)](https://truepeak78.github.io/ag1/06_BVS_a_vyva%C5%BEov%C3%A1n%C3%AD/6.5_AVL_operace/#theorem-6.3)

### Zadání
Uvažujte AVL strom z předchozí úlohy s kořenem $20$:
$$V = \{4, 5, 10, 20, 25, 27, 30\}$$
Proveďte operaci **smazání kořene** (`AVLDelete(20)`).
1. Popište standardní postup náhrady mazaného vrcholu se dvěma syny (inorder následník).
2. Detailně popište, jak probíhá kontrola a případná oprava vyvážení na cestě od místa fyzického odstranění vrcholu směrem nahoru ke kořeni.
3. Zakreslete výsledný strom a určete jeho hloubku.

> [!success]- Řešení
> ### 1. Fyzické odstranění a náhrada vrcholu se dvěma syny
> Uzel $20$ má dva syny (levý $5$, pravý $27$).
> Podle standardního algoritmu `BVSDelete` ([Algoritmus 6.6](https://truepeak78.github.io/ag1/06_BVS_a_vyva%C5%BEov%C3%A1n%C3%AD/6.2_Operace_BVS/#algo-6.6)):
> 1. Najdeme **inorder následníka** uzlu $20$: jdeme jednou doprava (do uzlu $27$) a pak co nejvíce doleva. Zde je levým synem $27$ uzel $25$, který nemá žádného levého syna. Inorder následník je tedy $25$.
> 2. Hodnotu klíče $25$ zkopírujeme do kořene (přepíšeme $20 \leftarrow 25$).
> 3. Fyzicky odstraníme původní uzel $25$ z jeho původní pozice: uzel $25$ byl listem (neměl žádné syny), jednoduše tedy nastavíme levý ukazatel uzlu $27$ na `null`.
>
> ### 2. Vyvažování směrem vzhůru od bodu fyzického smazání
> Fyzické smazání proběhlo v levém podstromu uzlu $27$. Postupujeme po cestě předků nahoru k novému kořeni:
>
> - **Krok 1: Kontrola v uzlu 27**
>   - Původní stav podstromu uzlu $27$: levý syn $25$ (hloubka 0), pravý syn $30$ (hloubka 0). $\text{bf}(27) = 0$.
>   - Po smazání $25$: levý podstrom je prázdný ($h = -1$), pravý podstrom má uzel $30$ ($h = 0$).
>   - Nový faktor vyvážení:
>     $$\text{bf}(27) = h(27.\text{right}) - h(27.\text{left}) = 0 - (-1) = +1$$
>   - Hodnota $+1 \in \{-1, 0, +1\}$ je v mezích AVL stromu. Žádná rotace v uzlu $27$ není potřeba.
>   - Výška podstromu s kořenem $27$ byla původně $1$ (obsahoval $27$ a listy $25, 30$). Nová výška podstromu je stále $1$ (obsahuje $27$ a syna $30$).
>   - Protože výška podstromu uzlu $27$ se **nezměnila**, nedošlo ke změně výšky na vyšších hladinách!
>
> - **Krok 2: Kontrola v kořeni (nyní s hodnotou 25)**
>   - Levý podstrom (kořen 5) má hloubku 1 (uzly 4, 5, 10).
>   - Pravý podstrom (kořen 27) má hloubku 1 (uzly 27, 30).
>   - Faktor vyvážení kořene: $\text{bf}(25) = 1 - 1 = 0$.
>
> ### 3. Výsledný strom
> - **Kořen:** $25$ ($\text{bf} = 0$)
>   - **Levý syn:** $5$ ($\text{bf} = 0$)
>     - Levý syn: $4$ ($\text{bf} = 0$)
>     - Pravý syn: $10$ ($\text{bf} = 0$)
>   - **Pravý syn:** $27$ ($\text{bf} = +1$)
>     - Levý syn: `null`
>     - Pravý syn: $30$ ($\text{bf} = 0$)
> - Celková hloubka stromu je **$h = 2$** (kořen na hladině 0, listy 4, 10, 30 na hladině 2).

---

## Úloha 7.3: Extrémní počty vrcholů v AVL stromu hloubky $h$ (Fibonacciho stromy)

* **Zdroj:** FitWiki (Malík 2018, Marast 2024, Scholtzová 2020)
* **Relevantní skripta:** [Věta 6.2 (O hloubce hloubkově vyvážených stromů)](https://truepeak78.github.io/ag1/06_BVS_a_vyva%C5%BEov%C3%A1n%C3%AD/6.4_AVL_rotace/#theorem-6.2) · [Definice 6.3 (AVL strom)](https://truepeak78.github.io/ag1/06_BVS_a_vyva%C5%BEov%C3%A1n%C3%AD/6.4_AVL_rotace/#def-6.3)

### Zadání
1. Odvoďte rekurentní vztah pro **minimální počet vrcholů** $N(h)$ v AVL stromu hloubky $h$ (pro $h \ge 0$).
2. Vypočtěte přesnou hodnotu $N(h)$ pro $h \in \{0, 1, 2, 3, 4, 5\}$.
3. Kolik nejméně a kolik nejvíce vrcholů může mít AVL strom s hloubkou $h = 4$?
4. Jaká je maximální možná hloubka AVL stromu o $n = 20$ vrcholech?

> [!success]- Řešení
> ### 1. Odvození rekurentního vztahu
> V AVL stromu se hloubky levého a pravého podstromu kořene smí lišit nejvýše o 1.
> Aby měl AVL strom hloubky $h$ minimální možný počet vrcholů, musí mít jeden podstrom hloubku $h-1$ a druhý podstrom minimální povolenou hloubku $h-2$, přičemž oba podstromy musí být samy minimálními AVL stromy daných hloubek.
> K tomu přičteme 1 za samotný kořen:
> $$N(h) = N(h - 1) + N(h - 2) + 1$$
> **Bázové případy:**
> - $h = 0$ (pouze kořen): $N(0) = \mathbf{1}$
> - $h = 1$ (kořen a jeden syn): $N(1) = \mathbf{2}$
>
> *(Poznámka: Posloupnost $N(h)$ úzce souvisí s Fibonacciho čísly vztahem $N(h) = F_{h+3} - 1$, kde $F_1=1, F_2=1, F_3=2, F_4=3, F_5=5, \dots$)*
>
> ### 2. Výpočet hodnot pro $h \in \{0, \dots, 5\}$
> - $N(0) = 1$
> - $N(1) = 2$
> - $N(2) = N(1) + N(0) + 1 = 2 + 1 + 1 = \mathbf{4}$
> - $N(3) = N(2) + N(1) + 1 = 4 + 2 + 1 = \mathbf{7}$
> - $N(4) = N(3) + N(2) + 1 = 7 + 4 + 1 = \mathbf{12}$
> - $N(5) = N(4) + N(3) + 1 = 12 + 7 + 1 = \mathbf{20}$
>
> ### 3. Extrémy pro hloubku $h = 4$
> - **Minimální počet vrcholů:** $N(4) = \mathbf{12}$.
> - **Maximální počet vrcholů:** Nastává pro úplný binární strom hloubky $h = 4$, kde jsou zaplněny všechny hladiny od $0$ do $4$:
>   $$N_{\max}(4) = 2^{h+1} - 1 = 2^5 - 1 = 32 - 1 = \mathbf{31}.$$
>
> ### 4. Maximální možná hloubka pro $n = 20$ vrcholů
> Hledáme největší celé číslo $h$, pro které platí $N(h) \le 20$.
> Z hodnot výše vidíme:
> - $N(4) = 12 \le 20$
> - $N(5) = 20 \le 20$
> - $N(6) = N(5) + N(4) + 1 = 20 + 12 + 1 = 33 > 20$
> Maximální možná hloubka AVL stromu o 20 vrcholech je tedy **$h = 5$** (tento strom má přesně minimální možný počet vrcholů pro hloubku 5, je to tzv. Fibonacciho strom).

---

## Úloha 7.4: Pravděpodobnost a střední hodnota počtu rotací při náhodném vložení

* **Zdroj:** FitWiki (Catay 2024, Dvořák 2024)
* **Relevantní skripta:** [Definice 7.7 (Střední hodnota náhodné veličiny)](https://truepeak78.github.io/ag1/07_Randomizace_a_he%C5%A1ov%C3%A1n%C3%AD/7.5_Pravd%C4%9Bpodobnostn%C3%AD_prostor/#def-7.7) · [Kapitola 6.5 (AVL rotace a vyvážení)](https://truepeak78.github.io/ag1/06_BVS_a_vyva%C5%BEov%C3%A1n%C3%AD/6.5_AVL_operace/)

### Zadání
Mějme AVL strom $T$ s 5 vrcholy:
- Kořen je $10$.
- Levý syn je $5$ (který má levého syna $2$).
- Pravý syn je $15$ (který má pravého syna $18$).
- Uzly $2$ a $18$ jsou listy.

Do tohoto stromu vložíme náhodný celočíselný klíč $x$ rovnoměrně vylosovaný z množiny $\{1, 3, 4, 6, 7, 8, 9, 11, 12, 13, 14, 16, 17, 19, 20\}$ (celkem 15 možných hodnot).
1. Pro každý interval možných hodnot určete, zda po vložení dojde k rotaci, a pokud ano, jakého bude typu (žádná, jednoduchá, dvojitá).
2. Určete pravděpodobnost, že vložení vyvolá **dvojitou rotaci** (LR nebo RL).
3. Určete **střední hodnotu počtu provedených jednoduchých rotací** (dvojitá rotace se počítá jako dvě jednoduché).

> [!success]- Řešení
> **Analýza výchozího stromu:**
> - Kořen $10$: levý podstrom má uzly $\{5, 2\}$ s hloubkou 1; pravý podstrom má uzly $\{15, 18\}$ s hloubkou 1.
> - Faktory vyvážení:
>   - $\text{bf}(10) = 1 - 1 = 0$
>   - $\text{bf}(5) = h(5.\text{right}) - h(5.\text{left}) = -1 - 0 = -1$ (uzel 5 má pouze levého syna 2)
>   - $\text{bf}(15) = h(15.\text{right}) - h(15.\text{left}) = 0 - (-1) = +1$ (uzel 15 má pouze pravého syna 18)
>   - $\text{bf}(2) = 0$, $\text{bf}(18) = 0$.
>
> Prozkoumáme pozici vložení klíče $x$ podle jeho velikosti:
>
> 1. **Případ $x \in \{1\}$ (vložení pod uzel 2 vlevo):**
>    - $x=1$ se stane levým synem uzlu 2.
>    - $\text{bf}(2)$ se stane $-1$.
>    - $\text{bf}(5)$ se stane $-2$ (levá větev má hloubku 1, pravá $-1$).
>    - V uzlu 5 došlo k porušení vyvážení. Jelikož $\text{bf}(2) = -1$, jedná se o případ LL $\implies$ **jednoduchá rotace R v uzlu 5** (1 rotace).
>
> 2. **Případ $x \in \{3, 4\}$ (vložení pod uzel 2 vpravo):**
>    - $x$ se stane pravým synem uzlu 2.
>    - $\text{bf}(2)$ se stane $+1$.
>    - $\text{bf}(5)$ se stane $-2$.
>    - V uzlu 5 došlo k porušení. Jelikož $\text{bf}(2) = +1$, jedná se o případ LR $\implies$ **dvojitá rotace LR v uzlu 5** (2 rotace).
>
> 3. **Případ $x \in \{6, 7, 8, 9\}$ (vložení pod uzel 5 vpravo):**
>    - $x$ se stane pravým synem uzlu 5.
>    - Uzel 5 má nyní levého syna 2 i pravého syna $x$. Nový $\text{bf}(5) = 0 - 0 = 0$.
>    - Výška podstromu uzlu 5 se nezměnila (stále 1), v kořeni 10 ke změně nedojde.
>    - $\implies$ **0 rotací** (vyvážení zachováno bez rotace).
>
> 4. **Případ $x \in \{11, 12, 13, 14\}$ (vložení pod uzel 15 vlevo):**
>    - $x$ se stane levým synem uzlu 15.
>    - Uzel 15 má nyní levého syna $x$ i pravého syna 18. Nový $\text{bf}(15) = 0 - 0 = 0$.
>    - Výška podstromu uzlu 15 se nezměnila.
>    - $\implies$ **0 rotací**.
>
> 5. **Případ $x \in \{16, 17\}$ (vložení pod uzel 18 vlevo):**
>    - $x$ se stane levým synem uzlu 18.
>    - $\text{bf}(18)$ se stane $-1$.
>    - $\text{bf}(15)$ se stane $+2$.
>    - V uzlu 15 došlo k porušení s $\text{bf}(18) = -1$, jedná se o případ RL $\implies$ **dvojitá rotace RL v uzlu 15** (2 rotace).
>
> 6. **Případ $x \in \{19, 20\}$ (vložení pod uzel 18 vpravo):**
>    - $x$ se stane pravým synem uzlu 18.
>    - $\text{bf}(18)$ se stane $+1$.
>    - $\text{bf}(15)$ se stane $+2$.
>    - V uzlu 15 došlo k porušení s $\text{bf}(18) = +1$, jedná se o případ RR $\implies$ **jednoduchá rotace L v uzlu 15** (1 rotace).
>
> ---
>
> ### Souhrnné vyhodnocení
> Množina obsahuje celkem $|\Omega| = 1 + 2 + 4 + 4 + 2 + 2 = \mathbf{15}$ možných klíčů:
> - **0 rotací:** klíče $\{6, 7, 8, 9, 11, 12, 13, 14\}$ $\implies$ $4 + 4 = \mathbf{8}$ hodnot.
> - **1 rotace (jednoduchá):** klíče $\{1, 19, 20\}$ $\implies$ $1 + 2 = \mathbf{3}$ hodnoty.
> - **2 rotace (dvojitá):** klíče $\{3, 4, 16, 17\}$ $\implies$ $2 + 2 = \mathbf{4}$ hodnoty.
>
> 1. **Pravděpodobnost vyvolání dvojité rotace:**
>    $$P(\text{dvojitá rotace}) = \frac{4}{15} \approx \mathbf{26{,}67\,\%}$$
> 2. **Střední hodnota počtu rotací $E[R]$:**
>    $$E[R] = 0 \cdot \frac{8}{15} + 1 \cdot \frac{3}{15} + 2 \cdot \frac{4}{15} = \frac{0 + 3 + 8}{15} = \mathbf{\frac{11}{15}} \approx \mathbf{0{,}733}$$

---

## Úloha 7.5: Algoritmus pro nalezení Inorder následníka v BVS

* **Zdroj:** FitWiki (Mitura 2017, Chludil 2017, Dvořák 2024)
* **Relevantní skripta:** [Algoritmus 6.3 (BVSPred a BVSSucc)](https://truepeak78.github.io/ag1/06_BVS_a_vyva%C5%BEov%C3%A1n%C3%AD/6.2_Operace_BVS/#algo-6.3) · [Definice 6.1 (BVS)](https://truepeak78.github.io/ag1/06_BVS_a_vyva%C5%BEov%C3%A1n%C3%AD/6.1_%C3%9Avod_do_BVS/#def-6.1)

### Zadání
Navrhněte algoritmus `InorderSuccessor(v)`, který pro zadaný uzel $v$ v binárním vyhledávacím stromě $T$ nalezne uzel s nejmenším klíčem větším než $\text{key}(v)$ (inorder následníka).
Každý uzel má ukazatele `left`, `right` a `parent`.
1. Zapište algoritmus formou pseudokódu.
2. Dokažte jeho korektnost a konečnost.
3. Odvoďte jeho asymptotickou časovou a paměťovou složitost vzhledem k hloubce stromu $h$.

> [!success]- Řešení
> ### 1. Algoritmus (Pseudokód)
> ```text
> InorderSuccessor(v):
>     // Případ 1: Uzel v má pravého syna
>     if v.right != null:
>         curr = v.right
>         while curr.left != null:
>             curr = curr.left
>         return curr
>     
>     // Případ 2: Uzel v nemá pravého syna -> stoupáme nahoru k předkům
>     curr = v
>     p = v.parent
>     while p != null and curr == p.right:
>         curr = p
>         p = p.parent
>     
>     return p  // vrátí buď hledaného předka, nebo null (pokud byl v maximem stromu)
> ```
>
> ### 2. Důkaz korektnosti
> Podle definice BVS ([Definice 6.1](https://truepeak78.github.io/ag1/06_BVS_a_vyva%C5%BEov%C3%A1n%C3%AD/6.1_%C3%9Avod_do_BVS/#def-6.1)) platí: pro každý uzel $u$ jsou všechny uzly v $u.\text{left}$ menší než $\text{key}(u)$ a všechny uzly v $u.\text{right}$ jsou větší než $\text{key}(u)$.
> - **Případ 1 ($v.\text{right} \neq \text{null}$):**
>   Všechny uzly s klíčem větším než $\text{key}(v)$ v podstromu uzlu $v$ leží v pravém podstromu $v.\text{right}$. Nejmenší z nich je nejlevější uzel tohoto pravého podstromu. První cyklus `while curr.left != null` nalezne právě tento uzel s minimálním klíčem v $v.\text{right}$. Korektnost v tomto případě platí.
> - **Případ 2 ($v.\text{right} == \text{null}$):**
>   Žádný potomek $v$ nemůže být jeho následníkem. Následník musí ležet mezi předky. Uzel $v$ leží v pravém podstromu svých předků, dokud stoupáme zprava nahoru. První předek $p$, pro kterého je aktuální uzel `curr` **levým synem**, má vlastnost $\text{key}(p) > \text{key}(\text{curr}) \ge \text{key}(v)$. Protože jsme z $v$ stoupali pouze přes pravé hrany, žádný menší klíč větší než $\text{key}(v)$ v celém stromu neexistuje. Pokud takový předek neexistuje ($p == \text{null}$), byl uzel $v$ maximálním prvkem celého stromu a algoritmus korektně vrátí `null`.
>
> ### 3. Důkaz konečnosti a složitost
> - **Konečnost:** V obou případech se pohybujeme podél jednoduché cesty ve stromu: v Případu 1 klesáme po hranách dolů, v Případu 2 stoupáme po hranách k rodiči nahoru. Jelikož strom je konečný a acyklický, délka cesty je omezena hloubkou stromu $h$. Cykly se provedou nejvýše $h$-krát a skončí.
> - **Časová složitost:** V nejhorším případě projdeme cestu od listu ke kořeni nebo od kořene k listu, což trvá $O(h)$ kroků. Pro vyvážený AVL strom je hloubka $h = O(\log n)$, operace tedy trvá **$O(\log n)$**; pro obecný BVS $O(h) \subseteq O(n)$.
> - **Paměťová složitost:** Algoritmus používá pouze dva pomocné ukazatele `curr` a `p`, přídavná paměť je striktně **$O(1)$**.

---

## Úloha 7.6: Rozdělení BVS na dva vyvážené AVL stromy v lineárním čase

* **Zdroj:** FitWiki (Scholtzová 2020)
* **Relevantní skripta:** [Algoritmus 6.1 (BVSShow – Inorder průchod)](https://truepeak78.github.io/ag1/06_BVS_a_vyva%C5%BEov%C3%A1n%C3%AD/6.2_Operace_BVS/#algo-6.1) · [Věta 6.1 (Vyvážený strom ze seřazeného pole)](https://truepeak78.github.io/ag1/06_BVS_a_vyva%C5%BEov%C3%A1n%C3%AD/6.3_Vyv%C3%A1%C5%BEenost_BVS/#theorem-6.1)

### Zadání
Mějme binární vyhledávací strom $T$ s $n = n_1 + n_2$ vrcholy (kde $n_1, n_2 \ge 1$ jsou zadaná přirozená čísla).
Navrhněte algoritmus, který v čase $O(n)$ rozdělí strom $T$ na dva nezávislé **hloubkově vyvážené AVL stromy** $T_1$ a $T_2$ takové, že:
- $T_1$ obsahuje $n_1$ nejmenších prvků z $T$,
- $T_2$ obsahuje zbývajících $n_2$ prvků z $T$.
Dokažte korektnost a odvoďte časovou složitost navrženého řešení.

> [!success]- Řešení
> ### Algoritmus
> 1. **Fáze 1 (Inorder linearizace stromu do pole):**
>    - Provedeme standardní inorder průchod stromem $T$ a všechny jeho vrcholy uložíme do pole $A[1 \dots n]$.
>    - Díky vlastnosti BVS je pole $A$ ostře rostoucí: $A[1] < A[2] < \dots < A[n]$.
> 2. **Fáze 2 (Rozdělení pole):**
>    - Pole $A$ rozdělíme na dva disjunktní úseky:
>      - $A_1 = A[1 \dots n_1]$ (obsahuje $n_1$ nejmenších prvků),
>      - $A_2 = A[n_1 + 1 \dots n_1 + n_2]$ (obsahuje zbylých $n_2$ větších prvků).
> 3. **Fáze 3 (Rekurzivní stavba dokonale vyváženého AVL stromu ze seřazeného pole):**
>    - Definujeme pomocnou funkci `BuildBalanced(A, L, R)`:
>      ```text
>      BuildBalanced(A, L, R):
>          if L > R: return null
>          mid = L + (R - L) / 2
>          root = NovýUzel(A[mid])
>          root.left = BuildBalanced(A, L, mid - 1)
>          root.right = BuildBalanced(A, mid + 1, R)
>          root.height = 1 + max(Height(root.left), Height(root.right))
>          return root
>      ```
>    - Zkonstruujeme $T_1 \leftarrow \text{BuildBalanced}(A_1, 1, n_1)$ a $T_2 \leftarrow \text{BuildBalanced}(A_2, n_1 + 1, n)$.
>
> ### Důkaz korektnosti
> 1. **BVS vlastnost:** V každém rekurzivním volání je za kořen zvolen prvek na indexu `mid`. Všechny prvky v levém úseku $A[L \dots mid-1]$ jsou ostře menší než $A[mid]$ a všechny v pravém úseku $A[mid+1 \dots R]$ jsou ostře větší. Výsledné stromy $T_1$ i $T_2$ jsou korektní BVS.
> 2. **AVL vyváženost:** V každém kroku se počet prvků rozdělí na poloviny: levý podstrom má $\lfloor (k-1)/2 \rfloor$ prvků a pravý $\lceil (k-1)/2 \rceil$ prvků. Rozdíl počtu prvků mezi levým a pravým podstromem je nejvýše 1 na každé hladině. Z toho plyne, že rozdíl hloubek podstromů je pro každý uzel nejvýše 1, tedy $|\text{bf}(v)| \le 1$. Výsledné stromy splňují definici AVL stromu.
> 3. **Dělení množin:** $T_1$ obsahuje prvky $A[1 \dots n_1]$ (přesně $n_1$ nejmenších) a $T_2$ obsahuje $A[n_1+1 \dots n]$, žádný prvek se neztratil ani nezdvojil.
>
> ### Časová a paměťová složitost
> - Inorder průchod navštíví každý vrchol stromu $T$ právě jednou $\implies O(n)$.
> - Rekurzivní stavba stromu ze seřazeného pole: pro každé podpole spočte index `mid` v $O(1)$ a vytvoří jeden uzel. Celkem se vytvoří $n$ uzlů, v každém uzlu strávíme $O(1)$ času $\implies O(n)$.
> - Rekurentní vztah pro stavbu: $T(k) = 2T(k/2) + O(1) \implies O(k)$ podle Master Theoremu.
> - Celková časová složitost je $O(n) + O(n_1) + O(n_2) = \mathbf{O(n)}$, což přesně splňuje požadavek zadání.


---

## Úloha 7.7: Návštěva předchůdce a následníka při vkládání do BVS
* **Zdroj:** FitWiki (Šestáková 2019, scan testu `test_ag1_sestakova_29_11_2019_18_00_6.jpg`)
* **Relevantní skripta:** [Definice 8.1 (Binární vyhledávací strom) a operace BVSInsert](https://truepeak78.github.io/ag1/08_Vyhled%C3%A1vac%C3%AD_stromy/8.1_Bin%C3%A1rn%C3%AD_vyhled%C3%A1vac%C3%AD_stromy/#def-8.1)

### Zadání
Dokažte nebo vyvraťte následující tvrzení:
> „Při vkládání nového klíče $k$ do binárního vyhledávacího stromu (BVS) navštívíme při hledání vhodné pozice pro klíč $k$ kromě jiných vrcholů také vždy jeho bezprostředního následníka (pokud v původním stromě existuje) a bezprostředního předchůdce (pokud v původním stromě existuje).“

Pro plný počet bodů je vyžadováno precizní matematické zdůvodnění.

> [!success]- Řešení
> ### Výrok
> **Tvrzení PLATÍ.**
> 
> ### Formální důkaz
> Mějme binární vyhledávací strom $T$ s množinou klíčů $V(T)$ a předpokládejme, že vkládaný klíč $k \notin V(T)$.
> Označme:
> - Bezprostředního předchůdce: $p = \max \{ x \in V(T) \mid x < k \}$ (pokud $\{ x \in V(T) \mid x < k \} \neq \emptyset$).
> - Bezprostředního následníka: $s = \min \{ y \in V(T) \mid y > k \}$ (pokud $\{ y \in V(T) \mid y > k \} \neq \emptyset$).
>
> Standardní algoritmus `BVSInsert(T, k)` začíná v kořeni stromu a v každém navštíveném vrcholu $u$ provede porovnání:
> - je-li $k < u.\text{key}$, pokračuje do levého syna $u.\text{left}$,
> - je-li $k > u.\text{key}$, pokračuje do pravého syna $u.\text{right}$.
>
> Prohledávání končí v okamžiku, kdy má algoritmus sestoupit do prázdného ukazatele (`null`), kde vytvoří nový list s klíčem $k$. Množina všech navštívených vrcholů tvoří právě cestu $P$ z kořene do rodiče nově vloženého listu.
>
> #### 1. Důkaz pro předchůdce $p$
> Předpokládejme, že v $T$ existuje alespoň jeden prvek menší než $k$.
> - Množina předků nového listu na cestě $P$, v nichž vyhledávání odbočilo **doprava**, je neprázdná (neboť kořen nebo některý uzel byl menší než $k$). Označme tyto předky $A_R = \{ u \in P \mid u.\text{key} < k \}$.
> - Nechť $u^* \in A_R$ je **nejhlubší** (poslední navštívený) vrchol z $A_R$.
> - Protože $u^* \in P$, platí $u^*.\text{key} < k$.
> - Z definice BVS platí:
>   1. Všechny uzly v levém podstromu $u^*$ jsou ostře menší než $u^*.\text{key} < k$.
>   2. V pravém podstromu $u^*$ leží nová pozice pro $k$. Jelikož $u^*$ je *nejhlubší* předek na cestě $P$ menší než $k$, v žádném dalším podstromu na cestě $P$ pod $u^*$ se již neodbočilo doprava — ze všech uzlů pod $u^*$ na cestě $P$ se odbočovalo výhradně doleva (všechny jsou $> k$).
>   3. Pokud by v celém stromě $T$ existoval nějaký uzel $z$ splňující $u^*.\text{key} < z < k$, musel by $z$ ležet v pravém podstromu $u^*$ a zároveň v levých podstromech všech uzlů na cestě $P$ pod $u^*$. To by znamenalo, že vyhledávací cesta pro $k$ by musela vést přes $z$, což je spor s tím, že vyhledávání prošlo až do listu a $z$ neminulo.
> - Tedy mezi $u^*.\text{key}$ a $k$ neleží žádný klíč z $T$. Proto $u^*$ je nutně bezprostředním předchůdcem: $u^* = p$.
> - Protože $u^* \in P$, byl vrchol $p$ při vyhledávání **přímo navštíven**.
>
> #### 2. Důkaz pro následníka $s$
> Analogicky:
> - Pokud existuje alespoň jeden prvek větší než $k$, uvažujme množinu $A_L = \{ w \in P \mid w.\text{key} > k \}$, což jsou předci na vyhledávací cestě, v nichž vyhledávání odbočilo **doleva**.
> - Nechť $w^* \in A_L$ je **nejhlubší** (poslední navštívený) vrchol z $A_L$.
> - Z vlastností BVS je $w^*.\text{key} > k$ a pod $w^*$ na cestě $P$ se již neodbočilo doleva (všechny uzly na cestě $P$ pod $w^*$ jsou $< k$).
> - Žádný uzel stromu nemůže mít hodnotu v otevřeném intervalu $(k, w^*.\text{key})$, neboť by musel ležet v levém podstromu $w^*$ a pravých podstromech jeho potomků na cestě $P$, což by přinutilo vyhledávání projít přes něj.
> - Tedy $w^*$ je bezprostředním následníkem: $w^* = s$.
> - Protože $w^* \in P$, byl vrchol $s$ při vyhledávání **přímo navštíven**.
>
> #### Závěr
> Pokud předchůdce $p$ (resp. následník $s$) existuje, je roven nejhlubšímu předkovi na vyhledávací cestě, ze kterého se odbočilo doprava (resp. doleva). Protože oba tito předci leží na vyhledávací cestě $P$, algoritmus `BVSInsert` je **vždy oba navštíví**.

---

## Úloha 7.8: Stavba AVL stromu a mazání uzlu se dvěma syny
* **Zdroj:** FitWiki (Pokorný 2021 `sbirka-2021`, scan testu `20211125_091802.jpg`)
* **Relevantní skripta:** [Sekce 8.2 (AVL stromy – operace AVLInsert a AVLDelete)](https://truepeak78.github.io/ag1/08_Vyhled%C3%A1vac%C3%AD_stromy/8.2_AVL_stromy/#avlinsert)

### Zadání
1. Mějme na začátku prázdný AVL strom. Vložte do něj postupně následující klíče:
   $$8, 9, 2, 1, 5, 6, 7, 4, 3$$
   Zakreslete strom před a po každé rotaci, uveďte typy rotací a výsledný strom po vložení všech 9 prvků.
2. Z výsledného stromu odstraňte klíč $5$ pomocí operace `AVLDelete(5)`. Popište průběh mazání (náhradu následníkem) a případné vyvažovací rotace.

> [!success]- Řešení
> ### Část 1: Postupné vkládání a rotace
> 
> 1. **Vložení 8, 9, 2:**
>    - Vložením 8 a 9 vznikne kořen 8 s pravým synem 9.
>    - Vložením 2 vznikne uzel 2 jako levý syn 8.
>    - Strom: Kořen 8, levý syn 2, pravý syn 9. Výšky obou podstromů jsou 1, koeficient vyváženosti $\text{bf}(8) = 1 - 1 = 0$. Vyváženo.
>
> 2. **Vložení 1:**
>    - 1 se vloží jako levý syn 2. $\text{bf}(2) = 0 - 1 = -1$, $\text{bf}(8) = 1 - 2 = -1$. Vyváženo.
>
> 3. **Vložení 5:**
>    - 5 se vloží jako pravý syn 2 ($2 < 5 < 8$).
>    - Strom pod 2: uzel 2 má levého syna 1 a pravého syna 5. $\text{bf}(2) = 1 - 1 = 0$.
>    - Strom u kořene 8: levý podstrom (kořen 2) má výšku 2, pravý (kořen 9) výšku 1. $\text{bf}(8) = 1 - 2 = -1$. Vyváženo.
>
> 4. **Vložení 6:**
>    - 6 se vloží jako pravý syn 5 ($2 < 5 < 6 < 8$).
>    - Cesta ke kořeni:
>      - Uzel 5: má pravého syna 6. Výška $h(5)=2$, $\text{bf}(5) = +1$.
>      - Uzel 2: levý syn 1 ($h=1$), pravý syn 5 ($h=2$). Výška $h(2)=3$, $\text{bf}(2) = 2 - 1 = +1$.
>      - Uzel 8: levý podstrom má výšku 3, pravý podstrom (9) má výšku 1.
>      - Koeficient vyváženosti: $\text{bf}(8) = h(9) - h(2) = 1 - 3 = -2$ $\implies$ **Porušení v uzlu 8!**
>    - **Typ porušení:** $\text{bf}(8) = -2$ (levá větev) a $\text{bf}(2) = +1$ (pravá větev) $\implies$ případ **Left-Right (LR)**.
>    - **Řešení:** Dvojitá rotace LR v uzlu 8:
>      1. Krok 1: Levá rotace v levém synovi (`LeftRotate(2)`).
>         - Uzel 5 se stane levým synem 8, uzel 2 jeho levým synem (s levým synem 1), uzel 6 zůstane pravým synem 5.
>      2. Krok 2: Pravá rotace v uzlu 8 (`RightRotate(8)`).
>         - Uzel 5 se stane novým kořenem!
>         - Levý syn 5 je 2 (s levým synem 1).
>         - Pravý syn 5 je 8 (s levým synem 6 a pravým synem 9).
>    - **Stav po vložení 6 a rotaci LR:**
>      ```text
>              5
>            /   \
>           2     8
>          /     / \
>         1     6   9
>      ```
>      Výška stromu je 3, všechny balance faktory jsou v $\{-1, 0, 1\}$.
>
> 5. **Vložení 7:**
>    - 7 se vloží jako pravý syn 6 ($5 < 6 < 7 < 8$).
>    - V uzlu 8: levý syn 6 má pravého syna 7 ($h(6)=2$), pravý syn 9 má $h=1$.
>    - $\text{bf}(6) = +1$, $\text{bf}(8) = 1 - 2 = -1$.
>    - Kořen 5: levý podstrom má výšku 2, pravý podstrom (kořen 8) má výšku 3. $\text{bf}(5) = 3 - 2 = +1$. Vyváženo.
>
> 6. **Vložení 4:**
>    - 4 se vloží jako pravý syn 2 pod 1? Ne: $2 < 4 < 5$, takže 4 se vloží jako pravý syn 2!
>    - Uzel 2 měl dříve levého syna 1 a pravý byl prázdný. Nyní má levého syna 1 a pravého syna 4. $\text{bf}(2) = 1 - 1 = 0$.
>    - Výška levého podstromu 5 se nezměnila. Vyváženo.
>
> 7. **Vložení 3:**
>    - 3 se vloží jako levý syn 4 ($2 < 3 < 4 < 5$).
>    - Cesta:
>      - Uzel 4: levý syn 3, $\text{bf}(4) = -1$, $h(4)=2$.
>      - Uzel 2: levý syn 1 ($h=1$), pravý syn 4 ($h=2$). Výška $h(2)=3$, $\text{bf}(2) = 2 - 1 = +1$.
>      - Kořen 5: levý podstrom má výšku 3, pravý má výšku 3. $\text{bf}(5) = 0$.
>      - Uzel 2: $\text{bf}(2) = +1$, uzel 4: $\text{bf}(4) = -1$.
>      - Ale pozor na uzel 2: $\text{bf}(2) = h(4) - h(1) = 2 - 1 = +1$, tedy uzel 2 porušen NENÍ.
>      - Výsledný strom po vložení 3:
>      ```text
>                5
>             /     \
>            2       8
>           / \     / \
>          1   4   6   9
>             /     \
>            3       7
>      ```
>      Všechny uzly mají vyváženost $\in \{-1, 0, 1\}$.
>
> ---
>
> ### Část 2: Odstranění uzlu 5 (`AVLDelete(5)`)
> 1. **Identifikace uzlu:** Klíč 5 se nachází v kořeni stromu a má **dva syny** (levý syn 2, pravý syn 8).
> 2. **Hledání náhrady:** Klíč 5 nahradíme jeho **inorder následníkem**, což je minimální prvek pravého podstromu:
>    - V pravém podstromu (kořen 8) jdeme doleva do uzlu 6. Uzel 6 nemá levého syna, takže následníkem je **klíč 6**.
> 3. **Přemístění:**
>    - Hodnota 6 se zkopíruje do kořene (nahradí hodnotu 5).
>    - Fyzicky se smaže původní uzel 6 z pravého podstromu.
>    - Uzel 6 měl pravého syna 7, takže uzel 7 se stane levým synem 8 na místě původního uzlu 6.
> 4. **Zpětné vyvážení od místa smazání:**
>    - Původní otec uzlu 6 byl uzel 8.
>    - Uzel 8 má nyní levého syna 7 ($h=1$) a pravého syna 9 ($h=1$).
>    - Výška uzlu 8 klesla z 3 na 2. Koeficient vyváženosti $\text{bf}(8) = 1 - 1 = 0$.
>    - Kořen (nyní s hodnotou 6):
>      - Levý podstrom (kořen 2): výška je 3.
>      - Pravý podstrom (kořen 8): výška je 2.
>      - Koeficient vyváženosti: $\text{bf}(6) = h(8) - h(2) = 2 - 3 = -1$.
>    - Strom je vyvážený, žádná rotace po smazání nenastala!
>
> **Výsledný AVL strom po `AVLDelete(5)`:**
> ```text
>           6
>        /     \
>       2       8
>      / \     / \
>     1   4   7   9
>        /
>       3
> ```

---

## Úloha 7.9: Konstrukce dokonale vyváženého BVS ze seřazeného pole v čase $O(n)$
* **Zdroj:** FitWiki (Mitura 2016 `81216mitura1`, Mitura 2017)
* **Relevantní skripta:** [Sekce 8.1 (Stavba a vyvažování vyhledávacích stromů)](https://truepeak78.github.io/ag1/08_Vyhled%C3%A1vac%C3%AD_stromy/8.1_Bin%C3%A1rn%C3%AD_vyhled%C3%A1vac%C3%AD_stromy/)

### Zadání
Na vstupu je dáno vzestupně seřazené pole různých prvků $A[1 \dots n]$.
1. Navrhněte algoritmus (zapište jej přehledným pseudokódem), který z tohoto pole vybuduje **dokonale vyvážený binární vyhledávací strom** (tj. strom, kde se pro každý vrchol počty uzlů v levém a pravém podstromu liší nejvýše o 1).
2. Dokažte korektnost algoritmu (splnění vyhledávací vlastnosti a dokonalého vyvážení).
3. Dokažte, že časová složitost algoritmu je striktně $O(n)$.

> [!success]- Řešení
> ### Pseudokód
> ```text
> Algorithm BuildPerfectBST(A):
>     Input:  Vzestupně seřazené pole A[1 ... n]
>     Output: Kořen dokonale vyváženého BVS
>     return RecBuild(A, 1, n)
>
> Function RecBuild(A, L, R):
>     if L > R:
>         return null
>     mid = L + (R - L) / 2          // celočíselné dělení (střední prvek)
>     v = NovýUzel(A[mid])
>     v.left  = RecBuild(A, L, mid - 1)
>     v.right = RecBuild(A, mid + 1, R)
>     return v
> ```
>
> ### Důkaz korektnosti
> 1. **Vlastnost BVS (vyhledávací uspořádání):**
>    - Pole $A$ je ostře rostoucí: $A[1] < A[2] < \dots < A[n]$.
>    - Volbou $mid = \lfloor (L+R)/2 \rfloor$ platí:
>      - Všechny prvky v podpoli $A[L \dots mid-1]$ mají indexy $< mid$, proto pro každý prvek $x \in A[L \dots mid-1]$ platí $x < A[mid]$. Tyto prvky rekurzivně tvoří levý podstrom $v.\text{left}$.
>      - Všechny prvky v podpoli $A[mid+1 \dots R]$ mají indexy $> mid$, proto pro každý prvek $y \in A[mid+1 \dots R]$ platí $y > A[mid]$. Tyto prvky tvoří pravý podstrom $v.\text{right}$.
>    - Indukcí podle velikosti intervalu $R - L + 1$ jsou podstromy korektními BVS, a tedy i celý strom je korektním BVS.
>
> 2. **Dokonalá vyváženost (počet uzlů):**
>    - Nechť $k = R - L + 1$ je počet prvků v intervalu.
>    - Levý podstrom obsahuje prvky $L \dots mid-1$, jejichž počet je:
>      $$k_L = (mid - 1) - L + 1 = mid - L = \lfloor \frac{L+R}{2} \rfloor - L = \lfloor \frac{k-1}{2} \rfloor$$
>    - Pravý podstrom obsahuje prvky $mid+1 \dots R$, jejichž počet je:
>      $$k_R = R - (mid + 1) + 1 = R - mid = R - \lfloor \frac{L+R}{2} \rfloor = \lceil \frac{k-1}{2} \rceil$$
>    - Rozdíl velikostí podstromů je:
>      $$|k_L - k_R| = \lceil \frac{k-1}{2} \rceil - \lfloor \frac{k-1}{2} \rfloor \le 1$$
>    - Tato rovnost platí pro každý podstrom na všech úrovních rekurze. Strom je tedy podle definice **dokonale vyvážený** (a tím pádem je i AVL vyvážený, neboť rozdíl hloubek je $\le 1$).
>
> ### Důkaz časové složitosti
> - V každém rekurzivním volání provede funkce:
>   - Výpočet indexu `mid` v $O(1)$,
>   - Alokaci a inicializaci jednoho uzlu v $O(1)$,
>   - Dvě rekurzivní volání na disjunktních intervalech poloviční velikosti.
> - Každý prvek pole $A$ je navštíven právě jednou jako `mid` právě jednoho rekurzivního volání.
> - Celkový počet vytvořených uzlů je $n$.
> - Rekurentní vztah pro časovou složitost:
>   $$T(n) = 2 T(n/2) + O(1)$$
> - Podle Kuchařkové věty (Master Theorem): $a = 2, b = 2, c = 0$. Protože $\log_b a = \log_2 2 = 1 > c = 0$, platí:
>   $$T(n) = \Theta(n^{\log_2 2}) = \mathbf{\Theta(n)}$$
> - Algoritmus běží v optimálním lineárním čase $\Theta(n)$.

---

## Úloha 7.10: Pravděpodobnost a střední hodnota počtu rotací v AVL stromu
* **Zdroj:** FitWiki (Catay 2024 `catay_2024_12_3`, scan testu `grafff.png`, Dvořák 2024 `dvorak_2024_12_2`)
* **Relevantní skripta:** [Sekce 8.2 (Složitost a rotace AVLInsert)](https://truepeak78.github.io/ag1/08_Vyhled%C3%A1vac%C3%AD_stromy/8.2_AVL_stromy/#avlinsert)

### Zadání
Mějme AVL strom $T$ obsahující 5 uzlů s klíči $\{8, 10, 11, 12, 14\}$ s následující strukturou:
- Kořen: $12$
- Levý syn kořene: $10$, který má levého syna $8$ a pravého syna $11$
- Pravý syn kořene: $14$ (list)

Do tohoto stromu vložíme jeden klíč $x$ zvolený náhodně se stejnou pravděpodobností z množiny dosud neobsažených celých čísel:
$$S = \{1, 2, 3, \dots, 20\} \setminus \{8, 10, 11, 12, 14\}, \quad |S| = 15$$

Předpokládejme konvenci FIT ČVUT: jednoduchá rotace (L nebo R) se počítá jako **1 rotace**, dvojitá rotace (LR nebo RL) se počítá jako **2 rotace**.
1. Pro každý možný klíč $x \in S$ určete počet vyvolaných rotací při volání `AVLInsert(T, x)`.
2. Určete pravděpodobnost, že vložení vyvolá dvojitou rotaci.
3. Vypočítejte střední hodnotu počtu rotací $\mathbb{E}[R]$.

> [!success]- Řešení
> ### 1. Analýza počátečního stromu
> Spočtěme hloubky podstromů a koeficienty vyváženosti $\text{bf}(v) = h(v.\text{right}) - h(v.\text{left})$:
> - Uzel 8: list $\implies h(8) = 1$, $\text{bf}(8) = 0$.
> - Uzel 11: list $\implies h(11) = 1$, $\text{bf}(11) = 0$.
> - Uzel 10: $h(10.\text{left}) = 1$, $h(10.\text{right}) = 1 \implies h(10) = 2$, $\text{bf}(10) = 1 - 1 = 0$.
> - Uzel 14: list $\implies h(14) = 1$, $\text{bf}(14) = 0$.
> - Kořen 12: $h(12.\text{left}) = h(10) = 2$, $h(12.\text{right}) = h(14) = 1 \implies h(12) = 3$.
>   $$\text{bf}(12) = h(14) - h(10) = 1 - 2 = -1$$
> 
> Strom je v platném AVL stavu, kořen 12 je vychýlen doleva ($\text{bf} = -1$).
>
> ---
>
> ### 2. Rozbor případů podle hodnoty vkládaného klíče $x \in S$
>
> Množina $S$ má 15 prvků, které se rozdělí do intervalů:
>
> #### Případ A: $x \in \{1, 2, 3, 4, 5, 6, 7\}$ (7 hodnot)
> - Každý takový klíč je menší než 8, takže se vloží jako **levý syn uzlu 8**.
> - Změny výšek podél cesty ke kořeni:
>   - $h(8)$ vzroste z 1 na 2 $\implies \text{bf}(8) = -1$.
>   - Uzel 10: levý syn 8 má výšku 2, pravý syn 11 má výšku 1 $\implies h(10)$ vzroste z 2 na 3, $\text{bf}(10) = 1 - 2 = -1$.
>   - Kořen 12: levý podstrom má výšku 3, pravý podstrom (14) má výšku 1 $\implies \text{bf}(12) = 1 - 3 = -2$ $\implies$ **Porušení v kořeni 12!**
> - **Klasifikace porušení:**
>   - $\text{bf}(12) = -2$ (levá větev) a $\text{bf}(10) = -1$ (levá větev) $\implies$ případ **Left-Left (LL)**.
> - **Potřebná rotace:** Jednoduchá pravá rotace `RightRotate(12)`.
> - Počet rotací: **1 rotace**.
>
> #### Případ B: $x = 9$ (1 hodnota)
> - Platí $8 < 9 < 10$, takže klíč 9 se vloží jako **pravý syn uzlu 8**.
> - Změny výšek:
>   - $h(8)$ vzroste z 1 na 2 $\implies \text{bf}(8) = +1$.
>   - Uzel 10: levý syn 8 má výšku 2, pravý syn 11 má výšku 1 $\implies h(10)$ vzroste na 3, $\text{bf}(10) = 1 - 2 = -1$.
>   - Kořen 12: levý podstrom má výšku 3, pravý má výšku 1 $\implies \text{bf}(12) = 1 - 3 = -2$ $\implies$ **Porušení v kořeni 12!**
> - **Klasifikace porušení:**
>   - V kořeni 12 je $\text{bf}(12) = -2$.
>   - V jeho levém synovi 10 je $\text{bf}(10) = -1$ (protože levý syn 8 má výšku 2 a pravý 11 má výšku 1).
>   - Jelikož znaménka $\text{bf}(12)$ a $\text{bf}(10)$ jsou **obě záporná**, jedná se stále o případ **Left-Left (LL)**!
> - **Potřebná rotace:** Jednoduchá pravá rotace `RightRotate(12)`.
> - Počet rotací: **1 rotace**.
>
> *(Poznámka k vyvažování v uzlu 8: uzel 8 má $\text{bf} = +1$, což je v toleranci, porušení nastalo až v kořeni 12).*
>
> #### Případ C: Klíče mezi 10 a 12 (žádný celočíselný klíč)
> - Jediný celočíselný klíč mezi 10 a 12 je 11, ten je však již ve stromě obsažen. Pro tento případ neexistuje žádný nový prvek v $S$.
>
> #### Případ D: $x = 13$ (1 hodnota)
> - Platí $12 < 13 < 14$, takže se vloží jako **levý syn uzlu 14**.
> - Změny výšek:
>   - $h(14)$ vzroste z 1 na 2, $\text{bf}(14) = -1$.
>   - Kořen 12: levý podstrom (10) má výšku 2, pravý podstrom (14) má výšku 2.
>   - Koeficient vyváženosti kořene: $\text{bf}(12) = h(14) - h(10) = 2 - 2 = 0$.
> - Žádný uzel neporušil AVL podmínku.
> - Počet rotací: **0 rotací**.
>
> #### Případ E: $x \in \{15, 16, 17, 18, 19, 20\}$ (6 hodnot)
> - Platí $x > 14$, takže se vloží jako **pravý syn uzlu 14**.
> - Změny výšek:
>   - $h(14)$ vzroste z 1 na 2, $\text{bf}(14) = +1$.
>   - Kořen 12: levý podstrom má výšku 2, pravý má výšku 2 $\implies \text{bf}(12) = 2 - 2 = 0$.
> - Žádné porušení nenastalo.
> - Počet rotací: **0 rotací**.
>
> ---
>
> ### 3. Shrnutí a výpočet střední hodnoty
>
> | Množina klíčů | Počet prvků | Cesta vložení | Vyvolané porušení | Typ rotace | Počet rotací $R$ |
> | :--- | :---: | :--- | :--- | :---: | :---: |
> | $\{1, 2, 3, 4, 5, 6, 7\}$ | 7 | pod uzel 8 doleva | Kořen 12 ($\text{bf}=-2, \text{bf}_L=-1$) | LL (Right) | 1 |
> | $\{9\}$ | 1 | pod uzel 8 doprava | Kořen 12 ($\text{bf}=-2, \text{bf}_L=-1$) | LL (Right) | 1 |
> | $\{13\}$ | 1 | pod uzel 14 doleva | Žádné ($\text{bf}(12)=0$) | Žádná | 0 |
> | $\{15, 16, 17, 18, 19, 20\}$ | 6 | pod uzel 14 doprava | Žádné ($\text{bf}(12)=0$) | Žádná | 0 |
> | **Celkem** | **15** | | | | |
>
> 1. **Pravděpodobnost dvojité rotace:**
>    Protože žádný vkládaný prvek nevyvolá případ LR ani RL, dvojitá rotace nenastane pro žádný z prvků.
>    $$P(\text{dvojitá rotace}) = \mathbf{0}$$
>
> 2. **Pravděpodobnost jednoduché rotace:**
>    Rotace nastane pro $7 + 1 = 8$ prvků z 15:
>    $$P(\text{rotace}) = \frac{8}{15} \approx 53{,}33\,\%$$
>
> 3. **Střední hodnota počtu rotací $\mathbb{E}[R]$:**
>    $$\mathbb{E}[R] = \sum_{r} r \cdot P(R = r) = 0 \cdot \frac{7}{15} + 1 \cdot \frac{8}{15} = \mathbf{\frac{8}{15}} \approx \mathbf{0{,}533}$$
