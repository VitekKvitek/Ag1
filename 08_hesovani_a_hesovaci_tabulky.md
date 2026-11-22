# 08. Hešování a hešovací tabulky – Zadání z písemek a řešení

Tento dokument obsahuje nejkomplexnější sbírku zkouškových a zápočtových úloh z BI-AG1 zaměřených na **hešovací tabulky**. Zahrnuje veškeré známé varianty z písemek (Pokorný, Suchý, Catay, Baier, Malík), online testů z Marastu a selftestů.

Obsahuje:
- **Otevřené adresování s lineárním přidáváním i dvojitým hešováním**
- **Správu a recyklaci náhrobků (`TOMBSTONE` / $\dagger$)**
- **Detekci duplicitních klíčů při vkládání přes náhrobky**
- **Algebraické hledání kolidujících prvků v intervalu $\{0 \dots 4095\}$**
- **Multiplikativní hešování (metodu vyšších bitů součinu) s dynamickým zvětšením (rehashing)**
- **Separátní řetězení (řetízky / Chaining) a hledání nejdelších řetízků**
- **Hešování textových řetězců znaků a kolizních stringů**
- **Teoretické otázky: shlukování, faktor naplnění $\alpha$ a narozeninový paradox**

Všechna zadání i mezikroky jsou zapsány v **autentické horizontální tabulkové podobě** přesně tak, jak se vyskytují na zkouškových arších FIT ČVUT.

---

## Úloha 8.1: Otevřená adresace z tištěné písemky (Pokorný 2021) – lineární probing a náhrobky $\dagger$

* **Zdroj:** FitWiki (Pokorný 25.11.2021, Písemka t15, ZS 2021/2022)
* **Relevantní skripta:** [Definice 7.1 (Slovník)](https://truepeak78.github.io/ag1/07_Randomizace_a_he%C5%A1ov%C3%A1n%C3%AD/7.1_Slovn%C3%ADky_a_He%C5%A1ovac%C3%AD_tabulky/#def-7.1) · [Algoritmy 7.2 a 7.3 (OpenDelete a OpenInsert)](https://truepeak78.github.io/ag1/07_Randomizace_a_he%C5%A1ov%C3%A1n%C3%AD/7.3_%C5%98e%C5%A1en%C3%AD_koliz%C3%AD/#algo-7.2) · [Kapitola 7.4 (Lineární přidávání)](https://truepeak78.github.io/ag1/07_Randomizace_a_he%C5%A1ov%C3%A1n%C3%AD/7.4_Volba_vyhled%C3%A1vac%C3%AD_posloupnosti/)

### Zadání
Uvažujme hešování s otevřenou adresací s lineárním prohledáváním v tabulce velikosti $m = 7$.
Mějme hešovací funkci danou předpisem:
$$h(k, i) = \Big( k + i \cdot \big( 1 + (k \bmod 6) \big) \Big) \bmod 7$$

Výchozí stav hešovací tabulky je dán následující horizontální tabulkou (kde symbol $\dagger$ značí náhrobek `TOMBSTONE` po dříve smazaném prvku):

| Přihrádka | 0 | 1 | 2 | 3 | 4 | 5 | 6 |
| :---: | :---: | :---: | :---: | :---: | :---: | :---: | :---: |
| **Obsah** | — | $\dagger$ | — | — | $\dagger$ | — | — |

Do této tabulky postupně:
1. vložte číslo **9**,
2. vložte číslo **18**,
3. vložte číslo **4**,
4. smažte číslo **9**.

Pro každou operaci uveďte posloupnost testovaných přihrádek a po každé operaci zakreslete výslednou podobu horizontální tabulky.

> [!success]- Řešení
> **Pravidla otevřené adresace s náhrobky:**
> - `Insert(k)`: Prochází posloupnost pokusů $h(k, 0), h(k, 1), \dots$. Pokud narazí na náhrobek $\dagger$, zaznamená si jeho index jako první možnou pozici pro vložení ($\text{first\_tomb}$), ale **musí pokračovat v prohledávání**, dokud nenarazí na prázdnou buňku (—), aby se vyloučila existence duplicitního klíče $k$ v tabulce. Jakmile narazí na prázdnou buňku, vloží klíč na pozici $\text{first\_tomb}$ (pokud byl nalezen), jinak na tuto prázdnou buňku.
> - `Delete(k)`: Vyhledá klíč $k$. Pokud jej nalezne, nahradí jej značkou $\dagger$ (`TOMBSTONE`). Nikdy nenastavuje buňku na prázdnou (—), aby nepřerušil vyhledávací cestu prvků vložených za ním.
>
> ---
>
> ### Postup jednotlivých operací:
>
> 1. **Vložení prvku 9:**
>    - $h_1(9) = 9 \bmod 7 = \mathbf{2}$
>    - Krok: $1 + (9 \bmod 6) = 1 + 3 = 4$
>    - $i = 0$: testujeme index $h(9, 0) = 2 \bmod 7 = \mathbf{2}$.
>    - Buňka 2 je prázdná (—) $\implies$ klíč 9 vložen přímo na **index 2**.
>    - **Testované buňky:** `[2]`
>
> | Přihrádka | 0 | 1 | 2 | 3 | 4 | 5 | 6 |
> | :---: | :---: | :---: | :---: | :---: | :---: | :---: | :---: |
> | **Obsah** | — | $\dagger$ | **9** | — | $\dagger$ | — | — |
>
> 2. **Vložení prvku 18:**
>    - $h_1(18) = 18 \bmod 7 = \mathbf{4}$
>    - Krok: $1 + (18 \bmod 6) = 1 + 0 = 1$
>    - $i = 0$: testujeme index $h(18, 0) = 4$. Buňka 4 obsahuje náhrobek $\dagger$. Zaznamenáme $\text{first\_tomb} = 4$.
>    - $i = 1$: testujeme index $h(18, 1) = (4 + 1) \bmod 7 = \mathbf{5}$. Buňka 5 je prázdná (—).
>    - Klíč 18 v tabulce neleží. Vložíme jej na první volný náhrobek, tedy na **index 4**.
>    - **Testované buňky:** `[4, 5]`
>
> | Přihrádka | 0 | 1 | 2 | 3 | 4 | 5 | 6 |
> | :---: | :---: | :---: | :---: | :---: | :---: | :---: | :---: |
> | **Obsah** | — | $\dagger$ | **9** | — | **18** | — | — |
>
> 3. **Vložení prvku 4:**
>    - $h_1(4) = 4 \bmod 7 = \mathbf{4}$
>    - Krok: $1 + (4 \bmod 6) = 1 + 4 = 5$
>    - $i = 0$: testujeme index $h(4, 0) = 4$. Buňka 4 obsahuje prvek $18 \neq 4$ $\implies$ **kolize!**
>    - $i = 1$: testujeme index $h(4, 1) = (4 + 5) \bmod 7 = 9 \bmod 7 = \mathbf{2}$. Buňka 2 obsahuje prvek $9 \neq 4$ $\implies$ **kolize!**
>    - $i = 2$: testujeme index $h(4, 2) = (4 + 2 \cdot 5) \bmod 7 = 14 \bmod 7 = \mathbf{0}$. Buňka 0 je prázdná (—).
>    - Klíč 4 v tabulce není. Žádný náhrobek jsme nepotkali. Vložíme 4 na **index 0**.
>    - **Testované buňky:** `[4, 2, 0]`
>
> | Přihrádka | 0 | 1 | 2 | 3 | 4 | 5 | 6 |
> | :---: | :---: | :---: | :---: | :---: | :---: | :---: | :---: |
> | **Obsah** | **4** | $\dagger$ | **9** | — | **18** | — | — |
>
> 4. **Smazání prvku 9 (`Delete(9)`):**
>    - $h_1(9) = 9 \bmod 7 = 2$.
>    - $i = 0$: testujeme index 2. Buňka 2 obsahuje hledaný klíč 9 $\implies$ prvek nalezen!
>    - Přepíšeme buňku 2 na náhrobek $\dagger$.
>    - **Testované buňky:** `[2]`
>
> | Přihrádka | 0 | 1 | 2 | 3 | 4 | 5 | 6 |
> | :---: | :---: | :---: | :---: | :---: | :---: | :---: | :---: |
> | **Obsah** | **4** | $\dagger$ | **$\dagger$** | — | **18** | — | — |

---

## Úloha 8.2: Otevřená adresace z tištěné písemky (Suchý 2017) – částečně zaplněná tabulka a dvojité hešování

* **Zdroj:** FitWiki (Suchý 6.12.2017, Písemka AG1)
* **Relevantní skripta:** [Kapitola 7.4 (Dvojité hešování)](https://truepeak78.github.io/ag1/07_Randomizace_a_he%C5%A1ov%C3%A1n%C3%AD/7.4_Volba_vyhled%C3%A1vac%C3%AD_posloupnosti/) · [Věta 7.1 (O efektivitě otevřené adresace)](https://truepeak78.github.io/ag1/07_Randomizace_a_he%C5%A1ov%C3%A1n%C3%AD/7.4_Volba_vyhled%C3%A1vac%C3%AD_posloupnosti/#theorem-7.1)

### Zadání
Pro hešování s otevřenou adresací mějme následující hešovací tabulku velikosti $m = 11$:

| Přihrádka | 0 | 1 | 2 | 3 | 4 | 5 | 6 | 7 | 8 | 9 | 10 |
| :---: | :---: | :---: | :---: | :---: | :---: | :---: | :---: | :---: | :---: | :---: | :---: |
| **Obsah** | **11** | — | — | — | **5** | — | — | — | — | **3** | — |

s hešovací funkcí:
$$h(k, i) = \Big( 3k + i \cdot \big( 1 + (3k \bmod 5) \big) \Big) \bmod 11$$

Do této tabulky postupně:
1. vložte prvek **19**,
2. smažte prvek **11**,
3. vložte prvky **36** a **22**.

U každé operace popište vyhledávací posloupnost a zakreslete výsledný stav horizontální tabulky po dokončení všech operací.

> [!success]- Řešení
> Hešovací funkce má složky:
> - primární heš: $h_1(k) = 3k \bmod 11$
> - krok: $h_2(k) = 1 + (3k \bmod 5)$, přičemž $h_2(k) \in \{1, 2, 3, 4, 5\}$, což zaručuje nesoudělnost s $m = 11$.
>
> ---
>
> ### Postupné operace:
>
> 1. **Vložení 19 (`Insert(19)`):**
>    - $3k = 3 \cdot 19 = 57$
>    - $h_1(19) = 57 \bmod 11 = \mathbf{2}$
>    - $h_2(19) = 1 + (57 \bmod 5) = 1 + 2 = \mathbf{3}$
>    - $i = 0$: testujeme index $2$. Buňka 2 je prázdná (—).
>    - Vložíme 19 na **index 2**.
>    - Navštívené buňky: `[2]`
>
> 2. **Smazání 11 (`Delete(11)`):**
>    - $3k = 3 \cdot 11 = 33$
>    - $h_1(11) = 33 \bmod 11 = \mathbf{0}$
>    - $i = 0$: index 0 obsahuje prvek 11 $\implies$ nalezen.
>    - Buňka 0 je přepsána na náhrobek $\dagger$ (`TOMBSTONE`).
>    - Navštívené buňky: `[0]`
>
> 3. **Vložení 36 (`Insert(36)`):**
>    - $3k = 3 \cdot 36 = 108$
>    - $h_1(36) = 108 \bmod 11 = \mathbf{9}$ ($9 \cdot 11 = 99, 108 - 99 = 9$)
>    - $h_2(36) = 1 + (108 \bmod 5) = 1 + 3 = \mathbf{4}$
>    - $i = 0$: index 9 obsahuje hodnotu $3 \neq 36$ $\implies$ **kolize!**
>    - $i = 1$: index $(9 + 1 \cdot 4) \bmod 11 = 13 \bmod 11 = \mathbf{2}$. Buňka 2 obsahuje hodnotu $19 \neq 36$ $\implies$ **kolize!**
>    - $i = 2$: index $(9 + 2 \cdot 4) \bmod 11 = 17 \bmod 11 = \mathbf{6}$. Buňka 6 je prázdná (—).
>    - Klíč 36 není v tabulce, vložíme jej na **index 6**.
>    - Navštívené buňky: `[9, 2, 6]`
>
> 4. **Vložení 22 (`Insert(22)`):**
>    - $3k = 3 \cdot 22 = 66$
>    - $h_1(22) = 66 \bmod 11 = \mathbf{0}$
>    - $h_2(22) = 1 + (66 \bmod 5) = 1 + 1 = \mathbf{2}$
>    - $i = 0$: index 0 obsahuje náhrobek $\dagger$. Zaznamenáme kandidáta $\text{first\_tomb} = 0$.
>    - Musíme pokračovat v procházení:
>      - $i = 1$: index $(0 + 1 \cdot 2) \bmod 11 = \mathbf{2}$. Buňka 2 obsahuje $19 \neq 22$.
>      - $i = 2$: index $(0 + 2 \cdot 2) \bmod 11 = \mathbf{4}$. Buňka 4 obsahuje $5 \neq 22$.
>      - $i = 3$: index $(0 + 3 \cdot 2) \bmod 11 = \mathbf{6}$. Buňka 6 obsahuje $36 \neq 22$.
>      - $i = 4$: index $(0 + 4 \cdot 2) \bmod 11 = \mathbf{8}$. Buňka 8 je prázdná (—)!
>    - Ověřili jsme, že 22 v tabulce neleží. Vložíme jej na první volný náhrobek, tedy na **index 0**.
>    - Navštívené buňky: `[0, 2, 4, 6, 8]`
>
> ---
>
> ### Výsledný stav hešovací tabulky:
>
> | Přihrádka | 0 | 1 | 2 | 3 | 4 | 5 | 6 | 7 | 8 | 9 | 10 |
> | :---: | :---: | :---: | :---: | :---: | :---: | :---: | :---: | :---: | :---: | :---: | :---: |
> | **Obsah** | **22** | — | **19** | — | **5** | — | **36** | — | — | **3** | — |

---

## Úloha 8.3: Lineární přidávání s náhrobky a vyhledáváním (Catay 2024)

* **Zdroj:** FitWiki (Catay 3.12.2024, Suchý 2017)
* **Relevantní skripta:** [Algoritmus 7.3 (OpenInsert)](https://truepeak78.github.io/ag1/07_Randomizace_a_he%C5%A1ov%C3%A1n%C3%AD/7.3_%C5%98e%C5%A1en%C3%AD_koliz%C3%AD/#algo-7.3) · [Kapitola 7.4 (Lineární přidávání)](https://truepeak78.github.io/ag1/07_Randomizace_a_he%C5%A1ov%C3%A1n%C3%AD/7.4_Volba_vyhled%C3%A1vac%C3%AD_posloupnosti/)

### Zadání
Mějme hešovací tabulku velikosti $m = 7$ s lineárním přidáváním:
$$h(k, i) = (4k + 3 + i) \bmod 7$$

Výchozí stav tabulky je dán horizontální tabulkou:

| Přihrádka | 0 | 1 | 2 | 3 | 4 | 5 | 6 |
| :---: | :---: | :---: | :---: | :---: | :---: | :---: | :---: |
| **Obsah** | — | $\dagger$ | — | $\dagger$ | — | — | — |

Postupně proveďte operace:
1. `Insert(3)`
2. `Insert(12)`
3. `Delete(3)`
4. `Find(12)`
5. `Insert(7)`

Pro každou operaci uveďte přesný seznam navštívených indexů přihrádek a zakreslete finální horizontální tabulku.

> [!success]- Řešení
> ### Postupné provedení operací:
>
> 1. **`Insert(3)`:**
>    - $i = 0$: $h(3, 0) = (4 \cdot 3 + 3 + 0) \bmod 7 = 15 \bmod 7 = \mathbf{1}$.
>      - Buňka $1$ obsahuje $\dagger$. Poznamenáme si $\text{first\_tomb} = 1$.
>    - $i = 1$: $h(3, 1) = (15 + 1) \bmod 7 = 16 \bmod 7 = \mathbf{2}$.
>      - Buňka $2$ je prázdná (—).
>    - Vložíme klíč 3 na index **1**.
>    - **Navštívené buňky:** `[1, 2]`
>
> 2. **`Insert(12)`:**
>    - $i = 0$: $h(12, 0) = (4 \cdot 12 + 3 + 0) \bmod 7 = 51 \bmod 7 = \mathbf{2}$.
>      - Buňka $2$ je prázdná (—).
>    - Vložíme klíč 12 přímo na index **2**.
>    - **Navštívené buňky:** `[2]`
>
> 3. **`Delete(3)`:**
>    - Vyhledáváme klíč 3:
>      - $i = 0$: $h(3, 0) = 1$. Buňka 1 obsahuje prvek 3 $\implies$ prvek nalezen.
>    - Přepíšeme buňku 1 na náhrobek $\dagger$.
>    - **Navštívené buňky:** `[1]`
>
> 4. **`Find(12)`:**
>    - $i = 0$: $h(12, 0) = 51 \bmod 7 = \mathbf{2}$.
>    - Buňka 2 obsahuje hledaný klíč 12 $\implies$ nalezeno v 1 kroku.
>    - **Navštívené buňky:** `[2]`
>
> 5. **`Insert(7)`:**
>    - $i = 0$: $h(7, 0) = (4 \cdot 7 + 3 + 0) \bmod 7 = 31 \bmod 7 = \mathbf{3}$.
>      - Buňka 3 obsahuje náhrobek $\dagger$. Poznamenáme si $\text{first\_tomb} = 3$.
>    - $i = 1$: $h(7, 1) = (31 + 1) \bmod 7 = 32 \bmod 7 = \mathbf{4}$.
>      - Buňka 4 je prázdná (—).
>    - Vložíme klíč 7 na index **3**.
>    - **Navštívené buňky:** `[3, 4]`
>
> ---
>
> ### Výsledný stav hešovací tabulky:
>
> | Přihrádka | 0 | 1 | 2 | 3 | 4 | 5 | 6 |
> | :---: | :---: | :---: | :---: | :---: | :---: | :---: | :---: |
> | **Obsah** | — | $\dagger$ | **12** | **7** | — | — | — |

---

## Úloha 8.4: Hledání prvků v intervalu $\{0 \dots 4095\}$ kolidujících na dané adrese (Písemka ZS 2019/2020)

* **Zdroj:** FitWiki (Písemka ZS 2019/2020, `stromecek.png`)
* **Relevantní skripta:** [Kapitola 7.4 (Dvojité hešování)](https://truepeak78.github.io/ag1/07_Randomizace_a_he%C5%A1ov%C3%A1n%C3%AD/7.4_Volba_vyhled%C3%A1vac%C3%AD_posloupnosti/) · [Kapitola 7.2 (Kolize)](https://truepeak78.github.io/ag1/07_Randomizace_a_he%C5%A1ov%C3%A1n%C3%AD/7.2_He%C5%A1ovac%C3%AD_funkce/)

### Zadání
Mějme hešovací funkci $f(k, i)$ pro otevřenou adresaci a dvojité hešování v tabulce velikosti $m = 11$ danou předpisem:
$$f(k, i) = \Big( 5k + i \cdot \big( 1 + (k \bmod 3) \big) \Big) \bmod 11$$

Nalezněte **dva různé prvky** $k_1, k_2 \in \{0, 1, \dots, 4095\}$, které kolidují s prvkem uloženým na **adrese 5**, a to konkrétně v **druhém pokusu ($i = 2$)**.
Uveďte přesný postup odvození a ověřte výpočet.

> [!success]- Řešení
> ### Odvození
> Hledáme celá čísla $k \in \{0, \dots, 4095\}$, pro která platí:
> $$f(k, 2) = \Big( 5k + 2 \cdot \big( 1 + (k \bmod 3) \big) \Big) \bmod 11 = 5$$
> 
> Označme zbytek $r = k \bmod 3 \in \{0, 1, 2\}$. Prozkoumáme jednotlivé možnosti pro $r$:
> 
> 1. **Případ $r = 0$ ($k \equiv 0 \pmod 3$):**
>    $$5k + 2(1 + 0) \equiv 5 \pmod{11} \iff 5k + 2 \equiv 5 \iff 5k \equiv 3 \pmod{11}$$
>    Inverzní prvek k $5$ modulo $11$ je $9$ (protože $5 \cdot 9 = 45 \equiv 1 \pmod{11}$).
>    $$k \equiv 3 \cdot 9 = 27 \equiv 5 \pmod{11}$$
>    Hledáme číslo $k$, které splňuje soustavu kongruencí:
>    $$k \equiv 0 \pmod 3 \quad \text{a} \quad k \equiv 5 \pmod{11}$$
>    Čísla tvaru $k = 5 + 11t$:
>    - pro $t = 0$: $k = 5$ ($5 \bmod 3 = 2 \neq 0$)
>    - pro $t = 1$: $k = 16$ ($16 \bmod 3 = 1 \neq 0$)
>    - pro $t = 2$: $k = 27$ ($27 \bmod 3 = 0$) $\implies \mathbf{k_1 = 27}$.
>    Další řešení v této větvi vznikají přičtením $\text{lcm}(3, 11) = 33$:
>    - $k_2 = 27 + 33 = \mathbf{60}$.
> 
> 2. **Případ $r = 1$ ($k \equiv 1 \pmod 3$):**
>    $$5k + 2(1 + 1) \equiv 5 \pmod{11} \iff 5k + 4 \equiv 5 \iff 5k \equiv 1 \pmod{11}$$
>    $$k \equiv 1 \cdot 9 = 9 \pmod{11}$$
>    Hledáme $k \equiv 1 \pmod 3$ a $k \equiv 9 \pmod{11}$:
>    Čísla tvaru $k = 9 + 11t$:
>    - $t = 0$: $9 \bmod 3 = 0$
>    - $t = 1$: $20 \bmod 3 = 2$
>    - $t = 2$: $k = 31$ ($31 \bmod 3 = 1$) $\implies \mathbf{k_3 = 31}$.
> 
> ---
> 
> ### Ověření výsledků:
> - **Pro $k_1 = 27$:**
>   - $27 \bmod 3 = 0 \implies \text{krok } h_2 = 1 + 0 = 1$.
>   - $f(27, 2) = (5 \cdot 27 + 2 \cdot 1) \bmod 11 = (135 + 2) \bmod 11 = 137 \bmod 11 = 12 \cdot 11 + \mathbf{5}$. Platí!
> - **Pro $k_2 = 60$:**
>   - $60 \bmod 3 = 0 \implies \text{krok } h_2 = 1$.
>   - $f(60, 2) = (5 \cdot 60 + 2 \cdot 1) \bmod 11 = 302 \bmod 11 = 27 \cdot 11 + \mathbf{5}$. Platí!
> 
> Hledanými prvky jsou například **$k_1 = 27$** a **$k_2 = 60$** (obě čísla leží v zadaném intervalu $\{0, \dots, 4095\}$).

---

## Úloha 8.5: Dvojité hešování a počítání kolizí (Marast & Selftest)

* **Zdroj:** FitWiki (Selftest ZS 18/19, ZS 19/20, ZS 20/21)
* **Relevantní skripta:** [Kapitola 7.4 (Dvojité hešování)](https://truepeak78.github.io/ag1/07_Randomizace_a_he%C5%A1ov%C3%A1n%C3%AD/7.4_Volba_vyhled%C3%A1vac%C3%AD_posloupnosti/)

### Zadání
Máme prázdnou hešovací tabulku velikosti $m = 13$ (přihrádky označené $0, \dots, 12$):

| Přihrádka | 0 | 1 | 2 | 3 | 4 | 5 | 6 | 7 | 8 | 9 | 10 | 11 | 12 |
| :---: | :---: | :---: | :---: | :---: | :---: | :---: | :---: | :---: | :---: | :---: | :---: | :---: | :---: |
| **Obsah** | — | — | — | — | — | — | — | — | — | — | — | — | — |

Použijeme dvojité hešování s otevřeným adresováním a vyhledávací posloupností:
$$h(k, i) = \Big( k + i \cdot \big( 1 + (k \bmod 7) \big) \Big) \bmod 13$$
Do tabulky postupně zahešujte čísla:
$$8, 1, 21, 47, 3, 42 \quad \text{(v tomto pořadí)}.$$

1. Pro každé číslo uveďte hodnoty primárního heše, kroku, testované indexy a počet kolizí.
2. Vypište prvky, při jejichž vložení nastala kolize, v pořadí, jak byly vkládány.
3. Určete celkový počet všech kolizí.
4. Zakreslete obsah tabulky po vložení všech čísel v horizontálním zobrazení.

> [!success]- Řešení
> Hešovací funkce:
> - primární index: $h_1(k) = k \bmod 13$
> - krok: $h_2(k) = 1 + (k \bmod 7)$
>
> ### Postupné vkládání prvků:
>
> 1. **Prvek 8:**
>    - $h_1(8) = 8 \bmod 13 = \mathbf{8}$, $h_2(8) = 1 + 1 = 2$.
>    - $i = 0$: index 8 je volný $\implies$ prvek **8** uložen na **index 8**.
>    - Testované indexy: `[8]`. Počet kolizí: **0**.
>
> 2. **Prvek 1:**
>    - $h_1(1) = 1 \bmod 13 = \mathbf{1}$, $h_2(1) = 1 + 1 = 2$.
>    - $i = 0$: index 1 je volný $\implies$ prvek **1** uložen na **index 1**.
>    - Testované indexy: `[1]`. Počet kolizí: **0**.
>
> 3. **Prvek 21:**
>    - $h_1(21) = 21 \bmod 13 = \mathbf{8}$, $h_2(21) = 1 + (21 \bmod 7) = 1 + 0 = \mathbf{1}$.
>    - $i = 0$: index 8 je obsazen (prvek 8) $\implies$ **kolize!**
>    - $i = 1$: index $(8 + 1 \cdot 1) \bmod 13 = \mathbf{9}$. Index 9 je volný $\implies$ prvek **21** uložen na **index 9**.
>    - Testované indexy: `[8, 9]`. Počet kolizí: **1**.
>
> 4. **Prvek 47:**
>    - $h_1(47) = 47 \bmod 13 = \mathbf{8}$ ($3 \cdot 13 = 39, 47 - 39 = 8$).
>    - $h_2(47) = 1 + (47 \bmod 7) = 1 + 5 = \mathbf{6}$.
>    - $i = 0$: index 8 je obsazen (prvek 8) $\implies$ **kolize!**
>    - $i = 1$: index $(8 + 1 \cdot 6) \bmod 13 = 14 \bmod 13 = \mathbf{1}$. Index 1 je obsazen (prvek 1) $\implies$ **kolize!**
>    - $i = 2$: index $(8 + 2 \cdot 6) \bmod 13 = 20 \bmod 13 = \mathbf{7}$. Index 7 je volný $\implies$ prvek **47** uložen na **index 7**.
>    - Testované indexy: `[8, 1, 7]`. Počet kolizí: **2**.
>
> 5. **Prvek 3:**
>    - $h_1(3) = 3 \bmod 13 = \mathbf{3}$, $h_2(3) = 1 + 3 = 4$.
>    - $i = 0$: index 3 je volný $\implies$ prvek **3** uložen na **index 3**.
>    - Testované indexy: `[3]`. Počet kolizí: **0**.
>
> 6. **Prvek 42:**
>    - $h_1(42) = 42 \bmod 13 = \mathbf{3}$ ($3 \cdot 13 = 39, 42 - 39 = 3$).
>    - $h_2(42) = 1 + (42 \bmod 7) = 1 + 0 = \mathbf{1}$.
>    - $i = 0$: index 3 je obsazen (prvek 3) $\implies$ **kolize!**
>    - $i = 1$: index $(3 + 1 \cdot 1) \bmod 13 = \mathbf{4}$. Index 4 je volný $\implies$ prvek **42** uložen na **index 4**.
>    - Testované indexy: `[3, 4]`. Počet kolizí: **1**.
>
> ---
>
> ### Shrnutí odpovědí:
> - **Prvky, při jejichž vložení nastala kolize:** **`21, 47, 42`**
> - **Celkový počet kolizí:** $1 + 2 + 1 = \mathbf{4}$
>
> ### Výsledný stav tabulky:
>
> | Přihrádka | 0 | 1 | 2 | 3 | 4 | 5 | 6 | 7 | 8 | 9 | 10 | 11 | 12 |
> | :---: | :---: | :---: | :---: | :---: | :---: | :---: | :---: | :---: | :---: | :---: | :---: | :---: | :---: |
> | **Obsah** | — | **1** | — | **3** | **42** | — | — | **47** | **8** | **21** | — | — | — |

---

## Úloha 8.6: Dvojité hešování s testem duplicitních klíčů a náhrobky (Marast 2022)

* **Zdroj:** FitWiki (Marast 4 ZS 2022/2023)
* **Relevantní skripta:** [Algoritmus 7.3 (OpenInsert)](https://truepeak78.github.io/ag1/07_Randomizace_a_he%C5%A1ov%C3%A1n%C3%AD/7.3_%C5%98e%C5%A1en%C3%AD_koliz%C3%AD/#algo-7.3) · [Kapitola 7.4 (Dvojité hešování)](https://truepeak78.github.io/ag1/07_Randomizace_a_he%C5%A1ov%C3%A1n%C3%AD/7.4_Volba_vyhled%C3%A1vac%C3%AD_posloupnosti/)

### Zadání
Máme prázdnou hešovací tabulku velikosti $m = 17$ (přihrádky označené $0, \dots, 16$):

| Přihrádka | 0 | 1 | 2 | 3 | 4 | 5 | 6 | 7 | 8 | 9 | 10 | 11 | 12 | 13 | 14 | 15 | 16 |
| :---: | :---: | :---: | :---: | :---: | :---: | :---: | :---: | :---: | :---: | :---: | :---: | :---: | :---: | :---: | :---: | :---: | :---: |
| **Obsah** | — | — | — | — | — | — | — | — | — | — | — | — | — | — | — | — | — |

Používáme dvojité hešování s hešovací funkcí:
$$h(k, i) = \Big( 6k + i \cdot \big( (4k \bmod 7) + 1 \big) \Big) \bmod 17$$

Postupně proveďte tyto operace:
1. Pomocí `OpenInsert` vložte klíče: $19, 69, 53, 52, 27, 18$.
2. Pomocí `OpenDelete` smažte klíče: $27, 69$.
3. Pomocí `OpenInsert` vložte klíče: $29, 37, 18$ *(pozor na chování při pokusu o vložení čísla 18, které již v tabulce leží!)*.

Zakreslete výslednou horizontální tabulku a vysvětlete, co se stalo při operaci `Insert(18)`.

> [!success]- Řešení
> Hešovací funkce:
> - $h_1(k) = 6k \bmod 17$
> - $h_2(k) = (4k \bmod 7) + 1$
>
> ---
>
> ### 1. Fáze: Vložení prvních 6 prvků
> - **19:** $h_1 = 114 \bmod 17 = \mathbf{12}$. Buňka 12 volná $\implies$ uložen na **index 12**.
> - **69:** $h_1 = 414 \bmod 17 = \mathbf{6}$. Buňka 6 volná $\implies$ uložen na **index 6**.
> - **53:** $h_1 = 318 \bmod 17 = 12$ (kolize!). $h_2 = (212 \bmod 7) + 1 = 3$.
>   $i=1$: $(12 + 3) \bmod 17 = \mathbf{15}$ volná $\implies$ uložen na **index 15**.
> - **52:** $h_1 = 312 \bmod 17 = 6$ (kolize!). $h_2 = (208 \bmod 7) + 1 = 6$.
>   $i=1$: $(6 + 6) \bmod 17 = 12$ (kolize!).
>   $i=2$: $(6 + 12) \bmod 17 = \mathbf{1}$ volná $\implies$ uložen na **index 1**.
> - **27:** $h_1 = 162 \bmod 17 = \mathbf{9}$. Buňka 9 volná $\implies$ uložen na **index 9**.
> - **18:** $h_1 = 108 \bmod 17 = 6$ (kolize!). $h_2 = (72 \bmod 7) + 1 = 3$.
>   Posloupnost: index 6 (kolize), 9 (kolize), 12 (kolize), 15 (kolize), 1 (kolize), index $(6 + 15) \bmod 17 = \mathbf{4}$ volná $\implies$ uložen na **index 4**.
>
> ---
>
> ### 2. Fáze: Mazání klíčů 27 a 69
> - `Delete(27)`: index 9 se přepíše na náhrobek $\dagger$.
> - `Delete(69)`: index 6 se přepíše na náhrobek $\dagger$.
>
> ---
>
> ### 3. Fáze: Vkládání 29, 37 a 18
> - **`Insert(29)`:**
>   - $h_1(29) = 174 \bmod 17 = \mathbf{4}$ (kolize s prvkem 18). $h_2 = 5$.
>   - $i=1$: $(4 + 5) \bmod 17 = \mathbf{9}$ obsahuje $\dagger$. Zaznamenáme $\text{first\_tomb} = 9$.
>   - $i=2$: $(4 + 10) \bmod 17 = \mathbf{14}$ je prázdná (—).
>   - Prvek 29 není v tabulce $\implies$ vložen na první volný náhrobek: **index 9**.
> - **`Insert(37)`:**
>   - $h_1(37) = 222 \bmod 17 = 1$ (kolize s prvkem 52). $h_2 = 2$.
>   - $i=1$: $(1 + 2) \bmod 17 = \mathbf{3}$ je prázdná $\implies$ vložen na **index 3**.
> - **`Insert(18)` (Duplikát!):**
>   - $h_1(18) = 6$, $h_2(18) = 3$.
>   - $i=0$: index 6 obsahuje náhrobek $\dagger$. Algoritmus si poznamená $\text{first\_tomb} = 6$, ale **musí pokračovat v testování**, zda 18 v tabulce již neleží!
>   - $i=1$: index 9 (obsahuje 29).
>   - $i=2$: index 12 (obsahuje 19).
>   - $i=3$: index 15 (obsahuje 53).
>   - $i=4$: index 1 (obsahuje 52).
>   - $i=5$: index 4 (obsahuje prvek 18!).
>   - **Klíč 18 byl v tabulce nalezen!** Operace vkládání okamžitě skončí bez zápisu. Náhrobek na indexu 6 **zůstává náhrobkem $\dagger$**.
>
> ---
>
> ### Výsledný stav hešovací tabulky:
>
> | Přihrádka | 0 | 1 | 2 | 3 | 4 | 5 | 6 | 7 | 8 | 9 | 10 | 11 | 12 | 13 | 14 | 15 | 16 |
> | :---: | :---: | :---: | :---: | :---: | :---: | :---: | :---: | :---: | :---: | :---: | :---: | :---: | :---: | :---: | :---: | :---: | :---: |
> | **Obsah** | — | **52** | — | **37** | **18** | — | **$\dagger$** | — | — | **29** | — | — | **19** | — | — | **53** | — |

---

## Úloha 8.7: Velká hešovací tabulka s dvojitým hešováním a recyklací náhrobků (Marast 2021 & 2024, Mitura)

* **Zdroj:** FitWiki (Marast 4 ZS 2021/2022, Mitura 2017)
* **Relevantní skripta:** [Kapitola 7.4 (Dvojité hešování)](https://truepeak78.github.io/ag1/07_Randomizace_a_he%C5%A1ov%C3%A1n%C3%AD/7.4_Volba_vyhled%C3%A1vac%C3%AD_posloupnosti/) · [Algoritmy 7.2 a 7.3](https://truepeak78.github.io/ag1/07_Randomizace_a_he%C5%A1ov%C3%A1n%C3%AD/7.3_%C5%98e%C5%A1en%C3%AD_koliz%C3%AD/)

### Zadání
Máme prázdnou hešovací tabulku velikosti $m = 17$ (přihrádky označené $0, \dots, 16$):

| Přihrádka | 0 | 1 | 2 | 3 | 4 | 5 | 6 | 7 | 8 | 9 | 10 | 11 | 12 | 13 | 14 | 15 | 16 |
| :---: | :---: | :---: | :---: | :---: | :---: | :---: | :---: | :---: | :---: | :---: | :---: | :---: | :---: | :---: | :---: | :---: | :---: |
| **Obsah** | — | — | — | — | — | — | — | — | — | — | — | — | — | — | — | — | — |

Používáme dvojité hešování s otevřeným adresováním a hešovací funkcí:
$$h(k, i) = \Big( 5k + i \cdot \big( (3k \bmod 7) + 1 \big) \Big) \bmod 17$$

Postupně proveďte tři fáze operací:
1. **Fáze 1 (Vkládání):** Pomocí `OpenInsert` vložte klíče $12, 54, 99, 33, 52, 37$.
2. **Fáze 2 (Mazání):** Pomocí `OpenDelete` smažte klíče $33$ a $99$.
3. **Fáze 3 (Nové vkládání):** Pomocí `OpenInsert` vložte klíče $27$ a $16$.

Zakreslete stav horizontální tabulky po každé fázi a detailně popište, jak se uplatnily náhrobky při vkládání prvků 27 a 16.

> [!success]- Řešení
> Hešovací funkce:
> - $h_1(k) = 5k \bmod 17$
> - $h_2(k) = (3k \bmod 7) + 1 \in \{1, \dots, 7\}$ (všechna nesoudělná s prvočíslem 17).
>
> ---
>
> ### Fáze 1: Vložení prvních 6 klíčů
> - **12:** $h_1(12) = 60 \bmod 17 = \mathbf{9}$, $h_2(12) = (36 \bmod 7) + 1 = 2$.
>   - $i=0$: index 9 je volný $\implies$ vložen na **index 9**.
> - **54:** $h_1(54) = 270 \bmod 17 = \mathbf{15}$, $h_2(54) = (162 \bmod 7) + 1 = 2$.
>   - $i=0$: index 15 je volný $\implies$ vložen na **index 15**.
> - **99:** $h_1(99) = 495 \bmod 17 = \mathbf{2}$, $h_2(99) = (297 \bmod 7) + 1 = 4$.
>   - $i=0$: index 2 je volný $\implies$ vložen na **index 2**.
> - **33:** $h_1(33) = 165 \bmod 17 = \mathbf{12}$, $h_2(33) = (99 \bmod 7) + 1 = 2$.
>   - $i=0$: index 12 je volný $\implies$ vložen na **index 12**.
> - **52:** $h_1(52) = 260 \bmod 17 = \mathbf{5}$, $h_2(52) = (156 \bmod 7) + 1 = 3$.
>   - $i=0$: index 5 je volný $\implies$ vložen na **index 5**.
> - **37:** $h_1(37) = 185 \bmod 17 = \mathbf{15}$, $h_2(37) = (111 \bmod 7) + 1 = 7$.
>   - $i=0$: index 15 obsazen (54) $\implies$ kolize!
>   - $i=1$: index $(15 + 7) \bmod 17 = 22 \bmod 17 = \mathbf{5}$ obsazen (52) $\implies$ kolize!
>   - $i=2$: index $(15 + 14) \bmod 17 = 29 \bmod 17 = \mathbf{12}$ obsazen (33) $\implies$ kolize!
>   - $i=3$: index $(15 + 21) \bmod 17 = 36 \bmod 17 = \mathbf{2}$ obsazen (99) $\implies$ kolize!
>   - $i=4$: index $(15 + 28) \bmod 17 = 43 \bmod 17 = \mathbf{9}$ obsazen (12) $\implies$ kolize!
>   - $i=5$: index $(15 + 35) \bmod 17 = 50 \bmod 17 = \mathbf{16}$. Index 16 je volný $\implies$ vložen na **index 16**.
>
> **Tabulka po Fázi 1:**
>
> | Přihrádka | 0 | 1 | 2 | 3 | 4 | 5 | 6 | 7 | 8 | 9 | 10 | 11 | 12 | 13 | 14 | 15 | 16 |
> | :---: | :---: | :---: | :---: | :---: | :---: | :---: | :---: | :---: | :---: | :---: | :---: | :---: | :---: | :---: | :---: | :---: | :---: |
> | **Obsah** | — | — | **99** | — | — | **52** | — | — | — | **12** | — | — | **33** | — | — | **54** | **37** |
>
> ---
>
> ### Fáze 2: Mazání klíčů 33 a 99
> - `Delete(33)`: $h_1(33) = 12 \implies$ buňka 12 se označí náhrobkem $\dagger$.
> - `Delete(99)`: $h_1(99) = 2 \implies$ buňka 2 se označí náhrobkem $\dagger$.
>
> **Tabulka po Fázi 2:**
>
> | Přihrádka | 0 | 1 | 2 | 3 | 4 | 5 | 6 | 7 | 8 | 9 | 10 | 11 | 12 | 13 | 14 | 15 | 16 |
> | :---: | :---: | :---: | :---: | :---: | :---: | :---: | :---: | :---: | :---: | :---: | :---: | :---: | :---: | :---: | :---: | :---: | :---: |
> | **Obsah** | — | — | **$\dagger$** | — | — | **52** | — | — | — | **12** | — | — | **$\dagger$** | — | — | **54** | **37** |
>
> ---
>
> ### Fáze 3: Vložení klíčů 27 a 16
> - **Vložení 27:**
>   - $h_1(27) = 135 \bmod 17 = \mathbf{16}$, $h_2(27) = (81 \bmod 7) + 1 = 5$.
>   - $i=0$: index 16 je obsazen (prvek 37) $\implies$ kolize!
>   - $i=1$: index $(16 + 5) \bmod 17 = 21 \bmod 17 = \mathbf{4}$. Buňka 4 je prázdná (—).
>   - Prvek 27 vložen přímo na **index 4**.
> - **Vložení 16:**
>   - $h_1(16) = 80 \bmod 17 = \mathbf{12}$, $h_2(16) = (48 \bmod 7) + 1 = 7$.
>   - $i=0$: index 12 obsahuje náhrobek $\dagger$. Zaznamenáme $\text{first\_tomb} = 12$.
>   - Pokračujeme pro ověření neexistence klíče:
>     - $i=1$: index $(12 + 7) \bmod 17 = \mathbf{2}$ (také $\dagger$).
>     - $i=2$: index $(12 + 14) \bmod 17 = 26 \bmod 17 = \mathbf{9}$ (obsazen prvkem 12).
>     - $i=3$: index $(12 + 21) \bmod 17 = 33 \bmod 17 = \mathbf{16}$ (obsazen prvkem 37).
>     - $i=4$: index $(12 + 28) \bmod 17 = 40 \bmod 17 = \mathbf{6}$ (prázdná buňka —).
>   - Prvek 16 není v tabulce $\implies$ zapíšeme jej na první nalezený náhrobek: **index 12**.
>
> ---
>
> ### Finální podoba hešovací tabulky:
>
> | Přihrádka | 0 | 1 | 2 | 3 | 4 | 5 | 6 | 7 | 8 | 9 | 10 | 11 | 12 | 13 | 14 | 15 | 16 |
> | :---: | :---: | :---: | :---: | :---: | :---: | :---: | :---: | :---: | :---: | :---: | :---: | :---: | :---: | :---: | :---: | :---: | :---: |
> | **Obsah** | — | — | **$\dagger$** | — | **27** | **52** | — | — | — | **12** | — | — | **16** | — | — | **54** | **37** |

---

## Úloha 8.8: Multiplikativní hešování (metoda vyšších bitů součinu) a rehashing (Marast 2022)

* **Zdroj:** FitWiki (Marast 4 ZS 2022/2023, `2022-11-26_19-43.png`)
* **Relevantní skripta:** [Kapitola 7.2 (Metoda násobení / vyšších bitů součinu)](https://truepeak78.github.io/ag1/07_Randomizace_a_he%C5%A1ov%C3%A1n%C3%AD/7.2_He%C5%A1ovac%C3%AD_funkce/) · [Kapitola 7.3 (Řešení kolizí řetězením)](https://truepeak78.github.io/ag1/07_Randomizace_a_he%C5%A1ov%C3%A1n%C3%AD/7.3_%C5%98e%C5%A1en%C3%AD_koliz%C3%AD/)

### Zadání
Budeme hešovat universum $\mathcal{U} = \{0, \dots, 65535\}$ (16bitové klíče, délka slova $w = 16$) pomocí **hešování se separátním řetězením** do tabulky s počáteční velikostí $m = 2^3 = 8$ ($\ell = 3$ bity) použitím **metody vyšších bitů součinu** s koeficientem $a = 127$.

Hešovací funkce pro tabulku velikosti $2^\ell$ je definována:
$$h(k) = (a \cdot k \bmod 2^w) \gg (w - \ell) = (127 \cdot k \bmod 65536) \gg (16 - \ell)$$

Postupně vkládejte prvky:
$$3271, 4161, 27, 14847, 534, 256, 313, 11527, 2234$$
tak dlouho, dokud každý z řetízků má délku **nejvýše 2**.
Jakmile libovolný řetízek dosáhne délky 3:
1. Určete prvek, jehož přidání vytvořilo řetízek délky 3 a vyvolalo nutnost zvětšení.
2. Přehešujte všechny dosavadní prvky do tabulky dvojnásobné velikosti ($m' = 2^4 = 16$, $\ell = 4$) a vložte zbytek prvků.
3. Určete nejdelší řetízek ve druhé tabulce po dokončení vkládání všech prvků (hodnoty uveďte v pořadí, jak přicházely na vstup).

> [!success]- Řešení
> ### Fáze 1: Vkládání do tabulky velikosti $m = 2^3 = 8$ ($\ell = 3$, posun o $16 - 3 = 13$ bitů):
> - Pro prvek $k$: spočteme $X = (127 \cdot k) \bmod 65536$ a heš je $\lfloor X / 2^{13} \rfloor = \lfloor X / 8192 \rfloor$.
>
> 1. $k = 3271$: $127 \cdot 3271 = 415417 \equiv 22201 \pmod{65536} \implies 22201 \gg 13 = \mathbf{2}$.
> 2. $k = 4161$: $127 \cdot 4161 = 528447 \equiv 4159 \implies 4159 \gg 13 = \mathbf{0}$.
> 3. $k = 27$: $127 \cdot 27 = 3429 \implies 3429 \gg 13 = \mathbf{0}$.
>    *(Přihrádka 0 má nyní řetízek $[4161, 27]$ délky 2).*
> 4. $k = 14847$: $127 \cdot 14847 = 1885569 \equiv 50561 \implies 50561 \gg 13 = \mathbf{6}$.
> 5. $k = 534$: $127 \cdot 534 = 67818 \equiv 2282 \implies 2282 \gg 13 = \mathbf{0}$.
>    *(Přihrádka 0 nyní obsahuje 3 prvky: $[4161, 27, 534]$ $\implies$ **délka 3!**)*
>
> **Prvek, který vyvolal přehešování:** **`534`**
>
> ---
>
> ### Fáze 2: Přehešování do tabulky velikosti $m' = 2^4 = 16$ ($\ell = 4$, posun o $16 - 4 = 12$ bitů):
> Dělíme $2^{12} = 4096$: $h'(k) = \lfloor X / 4096 \rfloor$.
>
> Spočteme indexy pro všech 9 prvků:
> - $k = 3271$: $22201 \gg 12 = \lfloor 22201 / 4096 \rfloor = \mathbf{5}$
> - $k = 4161$: $4159 \gg 12 = \mathbf{1}$
> - $k = 27$: $3429 \gg 12 = \mathbf{0}$
> - $k = 14847$: $50561 \gg 12 = \mathbf{12}$
> - $k = 534$: $2282 \gg 12 = \mathbf{0}$
> - $k = 256$: $127 \cdot 256 = 32512 \equiv 32512 \implies 32512 \gg 12 = \mathbf{7}$
> - $k = 313$: $127 \cdot 313 = 39751 \equiv 39751 \implies 39751 \gg 12 = \mathbf{9}$
> - $k = 11527$: $127 \cdot 11527 = 1463929 \equiv 22137 \implies 22137 \gg 12 = \mathbf{5}$
> - $k = 2234$: $127 \cdot 2234 = 283718 \equiv 21574 \implies 21574 \gg 12 = \mathbf{5}$
>
> ---
>
> ### Horizontální přehled obsazených přihrádek ve 2. tabulce ($m' = 16$):
>
> | Přihrádka | 0 | 1 | 5 | 7 | 9 | 12 | Ostatní |
> | :---: | :---: | :---: | :---: | :---: | :---: | :---: | :---: |
> | **Obsah** | 27, 534 | 4161 | **3271, 11527, 2234** | 256 | 313 | 14847 | $\emptyset$ |
>
> ---
>
> ### Výsledek:
> - Prvek vyvolávající přehešování: **`534`**
> - Nejdelší řetízek ve 2. tabulce leží v přihrádce 5 a obsahuje prvky: **`3271, 11527, 2234`**
> - Formát odpovědi: **`534:3271,11527,2234`**

---

## Úloha 8.9: Hešování se separátním řetězením (řetízky / Chaining) (Marast 2021 & 2024)

* **Zdroj:** FitWiki (Marast 4 ZS 2021/2022, ZS 2024/2025)
* **Relevantní skripta:** [Kapitola 7.3 (Řešení kolizí řetězením)](https://truepeak78.github.io/ag1/07_Randomizace_a_he%C5%A1ov%C3%A1n%C3%AD/7.3_%C5%98e%C5%A1en%C3%AD_koliz%C3%AD/) · [Definice 7.1](https://truepeak78.github.io/ag1/07_Randomizace_a_he%C5%A1ov%C3%A1n%C3%AD/7.1_Slovn%C3%ADky_a_He%C5%A1ovac%C3%AD_tabulky/#def-7.1)

### Zadání
Máme prázdnou hešovací tabulku velikosti $m = 17$ (přihrádky označené $0, \dots, 16$).
Použijeme hešování se separátním řetězením (každá přihrádka je spojový seznam) s hešovací funkcí:
$$h(x) = 11 \cdot x \bmod 17$$
Do tabulky postupně zahešujte čísla:
$$5, 65, 12, 24, 73, 99, 41, 63, 39 \quad \text{(v tomto pořadí)}.$$

1. Pro každé číslo určete hodnotu hešovací funkce $h(x)$.
2. Zakreslete obsah všech 17 přihrádek v horizontálním přehledu.
3. Určete délku nejdelšího vzniklého řetízku a vypište jeho prvky:
   - v pořadí od první vložené hodnoty po poslední,
   - v pořadí od hlavy spojového seznamu za předpokladu, že nové prvky se vkládají na začátek seznamu (`insert_first`).

> [!success]- Řešení
> ### 1. Výpočet hešovacích hodnot $h(x) = 11x \bmod 17$:
> - $h(5) = 55 \bmod 17 = \mathbf{4}$ ($3 \cdot 17 = 51$)
> - $h(65) = (11 \cdot 65) \bmod 17 = 715 \bmod 17 = \mathbf{1}$ ($42 \cdot 17 = 714$)
> - $h(12) = 132 \bmod 17 = \mathbf{13}$ ($7 \cdot 17 = 119$)
> - $h(24) = 264 \bmod 17 = \mathbf{9}$ ($15 \cdot 17 = 255$)
> - $h(73) = 803 \bmod 17 = \mathbf{4}$ ($47 \cdot 17 = 799$)
> - $h(99) = 1089 \bmod 17 = \mathbf{1}$ ($64 \cdot 17 = 1088$)
> - $h(41) = 451 \bmod 17 = \mathbf{9}$ ($26 \cdot 17 = 442$)
> - $h(63) = 693 \bmod 17 = \mathbf{13}$ ($40 \cdot 17 = 680$)
> - $h(39) = 429 \bmod 17 = \mathbf{4}$ ($25 \cdot 17 = 425$)
>
> ---
>
> ### 2. Horizontální tabulka všech 17 přihrádek (v pořadí vkládání):
>
> | Přihrádka | 0 | 1 | 2 | 3 | 4 | 5 | 6 | 7 | 8 | 9 | 10 | 11 | 12 | 13 | 14 | 15 | 16 |
> | :---: | :---: | :---: | :---: | :---: | :---: | :---: | :---: | :---: | :---: | :---: | :---: | :---: | :---: | :---: | :---: | :---: | :---: |
> | **Řetízek** | $\emptyset$ | 65, 99 | $\emptyset$ | $\emptyset$ | **5, 73, 39** | $\emptyset$ | $\emptyset$ | $\emptyset$ | $\emptyset$ | 24, 41 | $\emptyset$ | $\emptyset$ | $\emptyset$ | 12, 63 | $\emptyset$ | $\emptyset$ | $\emptyset$ |
>
> ---
>
> ### 3. Vyhodnocení nejdelšího řetízku:
> - Maximální délka řetízku je **3 prvky** (v přihrádce 4).
> - **V pořadí vkládání:** **`5, 73, 39`**
> - **Při vkládání na začátek (`insert_first`):** **`39, 73, 5`**

---

## Úloha 8.10: Otevřená adresace, počítání kolizí a mazání (Písemka Baier 2016)

* **Zdroj:** FitWiki (Baier 13.12.2016, `img_baier.jpg`)
* **Relevantní skripta:** [Kapitola 7.4 (Dvojité hešování)](https://truepeak78.github.io/ag1/07_Randomizace_a_he%C5%A1ov%C3%A1n%C3%AD/7.4_Volba_vyhled%C3%A1vac%C3%AD_posloupnosti/) · [Algoritmy 7.2 a 7.3](https://truepeak78.github.io/ag1/07_Randomizace_a_he%C5%A1ov%C3%A1n%C3%AD/7.3_%C5%98e%C5%A1en%C3%AD_koliz%C3%AD/)

### Zadání
Mějme hešovací tabulku velikosti $m = 7$ pro hešování s otevřenou adresací a hešovací funkcí:
$$h(k, i) = \Big( 5k + i \cdot \big( (k \bmod 3) + 1 \big) \Big) \bmod 7$$

1. Do prázdné tabulky vložte prvky:
   $$15, 41, 22, 42, 25, 1 \quad \text{(v tomto pořadí)}.$$
2. Pro každé vkládané číslo uveďte počet vyzkoušených obsazených polí (počet kolizí).
3. Zakreslete horizontální tabulku po vložení všech 6 prvků.
4. Z tabulky vymažte prvek **15** a zakreslete výsledný stav.

> [!success]- Řešení
> Hešovací funkce:
> - $h_1(k) = 5k \bmod 7$
> - $h_2(k) = (k \bmod 3) + 1 \in \{1, 2, 3\}$
>
> ---
>
> ### Postup vkládání a trasování kolizí:
>
> 1. **Prvek 15:**
>    - $h_1(15) = 75 \bmod 7 = \mathbf{5}$, $h_2(15) = (15 \bmod 3) + 1 = 1$.
>    - $i = 0$: index 5 je volný $\implies$ prvek 15 uložen na **index 5**.
>    - Kolizí: **0**.
>
> 2. **Prvek 41:**
>    - $h_1(41) = 205 \bmod 7 = \mathbf{2}$ ($29 \cdot 7 = 203$), $h_2(41) = (41 \bmod 3) + 1 = 3$.
>    - $i = 0$: index 2 je volný $\implies$ prvek 41 uložen na **index 2**.
>    - Kolizí: **0**.
>
> 3. **Prvek 22:**
>    - $h_1(22) = 110 \bmod 7 = \mathbf{5}$ ($15 \cdot 7 = 105$), $h_2(22) = (22 \bmod 3) + 1 = 2$.
>    - $i = 0$: index 5 je obsazen (prvek 15) $\implies$ **kolize!**
>    - $i = 1$: $(5 + 1 \cdot 2) \bmod 7 = \mathbf{0}$. Index 0 je volný $\implies$ prvek 22 uložen na **index 0**.
>    - Kolizí: **1**.
>
> 4. **Prvek 42:**
>    - $h_1(42) = 210 \bmod 7 = \mathbf{0}$, $h_2(42) = (42 \bmod 3) + 1 = 1$.
>    - $i = 0$: index 0 je obsazen (prvek 22) $\implies$ **kolize!**
>    - $i = 1$: $(0 + 1 \cdot 1) \bmod 7 = \mathbf{1}$. Index 1 je volný $\implies$ prvek 42 uložen na **index 1**.
>    - Kolizí: **1**.
>
> 5. **Prvek 25:**
>    - $h_1(25) = 125 \bmod 7 = \mathbf{6}$ ($17 \cdot 7 = 119$), $h_2(25) = (25 \bmod 3) + 1 = 2$.
>    - $i = 0$: index 6 je volný $\implies$ prvek 25 uložen na **index 6**.
>    - Kolizí: **0**.
>
> 6. **Prvek 1:**
>    - $h_1(1) = 5 \bmod 7 = \mathbf{5}$, $h_2(1) = (1 \bmod 3) + 1 = 2$.
>    - $i = 0$: index 5 obsazen (15) $\implies$ **kolize!**
>    - $i = 1$: $(5 + 2) \bmod 7 = \mathbf{0}$ obsazen (22) $\implies$ **kolize!**
>    - $i = 2$: $(5 + 4) \bmod 7 = \mathbf{2}$ obsazen (41) $\implies$ **kolize!**
>    - $i = 3$: $(5 + 6) \bmod 7 = 11 \bmod 7 = \mathbf{4}$. Index 4 je volný $\implies$ prvek 1 uložen na **index 4**.
>    - Kolizí: **3**.
>
> ---
>
> ### Přehled kolizí:
> - **15:** 0 kolizí
> - **41:** 0 kolizí
> - **22:** 1 kolize
> - **42:** 1 kolize
> - **25:** 0 kolizí
> - **1:** 3 kolize
> - **Celkem nastalo:** $0 + 0 + 1 + 1 + 0 + 3 = \mathbf{5 \text{ kolizí}}$.
>
> ### Stav tabulky po vložení všech 6 prvků:
>
> | Přihrádka | 0 | 1 | 2 | 3 | 4 | 5 | 6 |
> | :---: | :---: | :---: | :---: | :---: | :---: | :---: | :---: |
> | **Obsah** | **22** | **42** | **41** | — | **1** | **15** | **25** |
>
> ---
>
> ### Smazání prvku 15 (`Delete(15)`):
> Vyhledáme prvek 15 ($h_1 = 5$), nalezen na indexu 5. Buňka 5 je nahrazena náhrobkem $\dagger$.
>
> ### Finální horizontální tabulka:
>
> | Přihrádka | 0 | 1 | 2 | 3 | 4 | 5 | 6 |
> | :---: | :---: | :---: | :---: | :---: | :---: | :---: | :---: |
> | **Obsah** | **22** | **42** | **41** | — | **1** | **$\dagger$** | **25** |

---

## Úloha 8.11: Hešovací funkce řetězců znaků, kolizní string a otevřená adresace (Malík 2018)

* **Zdroj:** FitWiki (Malík 6.12.2018)
* **Relevantní skripta:** [Kapitola 7.2 (Hešovací funkce a kolize)](https://truepeak78.github.io/ag1/07_Randomizace_a_he%C5%A1ov%C3%A1n%C3%AD/7.2_He%C5%A1ovac%C3%AD_funkce/) · [Kapitola 7.4 (Volba vyhledávací posloupnosti)](https://truepeak78.github.io/ag1/07_Randomizace_a_he%C5%A1ov%C3%A1n%C3%AD/7.4_Volba_vyhled%C3%A1vac%C3%AD_posloupnosti/)

### Zadání
Mějme hešovací funkci $g(S)$, která pro textový řetězec $S = s_0 s_1 \dots s_{k-1}$ znaků počítá hodnotu podle algoritmu:
```text
g(S):
    r = 0
    for i = 0 to length(S) - 1:
        r = (r * 3 + 2 * S[i]) mod 5
    return r
```
Uvažujte ASCII kódy znaků: `'a' = 97 \equiv 2, 'b' = 98 \equiv 3, 'c' = 99 \equiv 4, 'd' = 100 \equiv 0, 'e' = 101 \equiv 1, 'f' = 102 \equiv 2 \pmod 5$.

1. Vypočtěte hodnotu heše $g(\text{"bbb"})$.
2. Nalezněte řetězec $S'$ o délce právě 4 znaky složený ze znaků $\{'a', 'b', 'c'\}$, který má stejnou hešovací hodnotu jako `"bbb"`.
3. Vypočtěte hodnoty $g(S)$ pro řetězce: `"bac"`, `"f"`, `"ea"`, `"bad"`.
4. Do prázdné tabulky velikosti $m = 5$ s otevřeným adresováním a funkcí:
   $$h(S, i) = \big( g(S) + 3 \cdot i \big) \bmod 5$$
   postupně vložte řetězce `["bac", "f", "ea", "bad"]`. Zakreslete výslednou horizontální tabulku.

> [!success]- Řešení
> ### 1. Výpočet $g(\text{"bbb"})$
> Iterace pro znak `'b'` ($98 \equiv 3 \pmod 5$):
> $r_{i+1} = (3 r_i + 2 \cdot 3) \bmod 5 = (3 r_i + 1) \bmod 5$.
> - $r_0 = 0$
> - $r_1 = (0 + 1) \bmod 5 = \mathbf{1}$
> - $r_2 = (3 \cdot 1 + 1) \bmod 5 = \mathbf{4}$
> - $r_3 = (3 \cdot 4 + 1) \bmod 5 = 13 \bmod 5 = \mathbf{3}$
> $$g(\text{"bbb"}) = \mathbf{3}$$
>
> ---
>
> ### 2. Nalezení kolizního řetězce délky 4
> Zvolme předponu $s_0 = \text{'b'}, s_1 = \text{'a'}, s_2 = \text{'a'}$:
> - $r_1 = (0 + 2 \cdot 3) \bmod 5 = 1$
> - $r_2 = (3 \cdot 1 + 2 \cdot 2) \bmod 5 = 7 \bmod 5 = 2$
> - $r_3 = (3 \cdot 2 + 2 \cdot 2) \bmod 5 = 10 \bmod 5 = 0$
> Hledáme čtvrtý znak $s_3$ s kódem $x \in \{2, 3, 4\}$:
> $$r_4 = (3 \cdot 0 + 2x) \bmod 5 = 3 \iff 2x \equiv 8 \implies x = 4.$$
> Kód 4 odpovídá znaku **`'c'`**. Kolizním řetězcem je **`"baac"`** ($g(\text{"baac"}) = 3$).
>
> ---
>
> ### 3. Výpočet hešů pro zadanou čtveřici řetězců:
> - **`"bac"`:**
>   - $s_0 = \text{'b'} \implies r_1 = 1$.
>   - $s_1 = \text{'a'} \implies r_2 = (3 \cdot 1 + 4) \bmod 5 = 2$.
>   - $s_2 = \text{'c'} \equiv 4 \implies r_3 = (3 \cdot 2 + 2 \cdot 4) \bmod 5 = 14 \bmod 5 = \mathbf{4}$.
>   - $g(\text{"bac"}) = \mathbf{4}$.
> - **`"f"`:**
>   - $s_0 = \text{'f'} \equiv 2 \implies r_1 = (0 + 2 \cdot 2) \bmod 5 = \mathbf{4}$.
>   - $g(\text{"f"}) = \mathbf{4}$.
> - **`"ea"`:**
>   - $s_0 = \text{'e'} \equiv 1 \implies r_1 = (0 + 2 \cdot 1) \bmod 5 = 2$.
>   - $s_1 = \text{'a'} \equiv 2 \implies r_2 = (3 \cdot 2 + 2 \cdot 2) \bmod 5 = 10 \bmod 5 = \mathbf{0}$.
>   - $g(\text{"ea"}) = \mathbf{0}$.
> - **`"bad"`:**
>   - $s_0 = \text{'b'} \implies r_1 = 1$.
>   - $s_1 = \text{'a'} \implies r_2 = 2$.
>   - $s_2 = \text{'d'} \equiv 0 \implies r_3 = (3 \cdot 2 + 0) \bmod 5 = 6 \bmod 5 = \mathbf{1}$.
>   - $g(\text{"bad"}) = \mathbf{1}$.
>
> ---
>
> ### 4. Vkládání do tabulky velikosti $m = 5$ ($h(S, i) = (g(S) + 3i) \bmod 5$):
> - **`"bac"` ($g = 4$):**
>   - $i = 0$: index $4$. Buňka 4 je volná $\implies$ uložen na **index 4**.
> - **`"f"` ($g = 4$):**
>   - $i = 0$: index $4$ je obsazen (`"bac"`) $\implies$ **kolize!**
>   - $i = 1$: index $(4 + 3) \bmod 5 = 7 \bmod 5 = \mathbf{2}$. Buňka 2 je volná $\implies$ uložen na **index 2**.
> - **`"ea"` ($g = 0$):**
>   - $i = 0$: index $0$. Buňka 0 je volná $\implies$ uložen na **index 0**.
> - **`"bad"` ($g = 1$):**
>   - $i = 0$: index $1$. Buňka 1 je volná $\implies$ uložen na **index 1**.
>
> ### Výsledný stav hešovací tabulky:
>
> | Přihrádka | 0 | 1 | 2 | 3 | 4 |
> | :---: | :---: | :---: | :---: | :---: | :---: |
> | **Obsah** | **`"ea"`** | **`"bad"`** | **`"f"`** | — | **`"bac"`** |

---

## Úloha 8.12: Teorie hešování – shlukování, faktor naplnění $\alpha$ a narozeninový paradox

* **Zdroj:** FitWiki (Marast 2021, Dvořák 2022, Scholtzová 2020)
* **Relevantní skripta:** [Věta 7.1 (O efektivitě otevřené adresace)](https://truepeak78.github.io/ag1/07_Randomizace_a_he%C5%A1ov%C3%A1n%C3%AD/7.4_Volba_vyhled%C3%A1vac%C3%AD_posloupnosti/#theorem-7.1) · [Kapitola 7.4 (Volba vyhledávací posloupnosti)](https://truepeak78.github.io/ag1/07_Randomizace_a_he%C5%A1ov%C3%A1n%C3%AD/7.4_Volba_vyhled%C3%A1vac%C3%AD_posloupnosti/)

### Zadání
1. Popište rozdíl mezi **primárním** a **sekundárním shlukováním** (primary vs. secondary clustering) a uveďte, jaké techniky otevřeného adresování jimi trpí či netrpí.
2. Pro otevřené adresování s faktorem naplnění $\alpha = \frac{n}{m}$ ($0 \le \alpha < 1$) za předpokladu rovnoměrného hešování uveďte vzorce pro očekávaný počet testovaných buněk při neúspěšném a úspěšném vyhledávání. Vyčíslete pro $\alpha = 0{,}5$ a $\alpha = 0{,}9$.
3. **Narozeninový paradox v hešování:** Kolik prvků $n$ stačí náhodně vložit do tabulky velikosti $m$, aby pravděpodobnost vzniku alespoň jedné kolize byla přibližně alespoň $50\,\%$?

> [!success]- Řešení
> ### 1. Primární vs. sekundární shlukování
> - **Primární shlukování (Primary clustering):**
>   - Vzniká u **lineárního přidávání** ($h(k, i) = (h_1(k) + i) \bmod m$).
>   - Libovolná kolize vytváří souvislý blok obsazených přihrádek. Jakýkoliv další prvek, který se zahešuje kamkoliv do tohoto bloku nebo těsně před něj, blok ještě více prodlouží. Pravděpodobnost zasažení bloku roste úměrně jeho délce $\implies$ shluky lavinovitě rostou a degradují operace na $O(n)$.
>   - **Kvadratické přidávání i dvojité hešování primární shlukování zcela eliminují.**
> - **Sekundární shlukování (Secondary clustering):**
>   - Vzniká tehdy, když dva různé klíče se stejným primárním hešem ($h_1(k_1) = h_1(k_2)$) procházejí **identickou vyhledávací posloupností** pro všechna $i \ge 0$.
>   - Trpí jím lineární i **kvadratické přidávání** ($h(k, i) = (h_1(k) + c_1 i + c_2 i^2) \bmod m$), protože krok závisí pouze na indexu pokusu $i$, nikoliv na klíči $k$.
>   - **Dvojité hešování sekundárním shlukováním netrpí**, protože krok $h_2(k)$ závisí na samotném klíči, takže dva klíče se stejným $h_1$ mají s vysokou pravděpodobností odlišné kroky $h_2(k_1) \neq h_2(k_2)$ a ihned při $i=1$ se rozptýlí.
>
> ---
>
> ### 2. Faktor naplnění $\alpha$ a složitost operací (Věta 7.1)
> Za předpokladu rovnoměrného hešování:
> - **Neúspěšné vyhledávání (i vložení nového prvku):**
>   $$E[\text{unsuccessful}] = \sum_{i=0}^{\infty} \alpha^i = \mathbf{\frac{1}{1 - \alpha}}$$
> - **Úspěšné vyhledávání:**
>   $$E[\text{successful}] \approx \frac{1}{\alpha} \int_0^{\alpha} \frac{1}{1 - x} \, dx = \mathbf{\frac{1}{\alpha} \ln \left(\frac{1}{1 - \alpha}\right)}$$
>
> **Vyčíslení:**
> - Pro $\alpha = 0{,}5$:
>   $$E[\text{unsuccessful}] = \frac{1}{1 - 0{,}5} = \mathbf{2 \text{ testované buňky}}, \quad E[\text{successful}] = \frac{1}{0{,}5} \ln(2) \approx \mathbf{1{,}39 \text{ buňky}}.$$
> - Pro $\alpha = 0{,}9$:
>   $$E[\text{unsuccessful}] = \frac{1}{1 - 0{,}9} = \mathbf{10 \text{ testovaných buněk}}, \quad E[\text{successful}] = \frac{1}{0{,}9} \ln(10) \approx \mathbf{2{,}56 \text{ buňky}}.$$
>
> ---
>
> ### 3. Narozeninový paradox v hešování
> Pravděpodobnost, že při vložení $n$ prvků do $m$ přihrádek nedojde k žádné kolizi, je:
> $$P(\text{bez kolize}) = \prod_{i=1}^{n-1} \left(1 - \frac{i}{m}\right) \approx e^{-\sum_{i=1}^{n-1} \frac{i}{m}} = e^{-\frac{n(n-1)}{2m}} \approx e^{-\frac{n^2}{2m}}.$$
> Pro pravděpodobnost kolize $P(\text{kolize}) \ge \frac{1}{2}$ položíme $e^{-\frac{n^2}{2m}} \le \frac{1}{2}$:
> $$\frac{n^2}{2m} \ge \ln 2 \implies n \approx \sqrt{2 \ln 2 \cdot m} \approx \mathbf{1{,}177 \sqrt{m}}.$$
> K tomu, aby nastala kolize s pravděpodobností $\ge 50\,\%$, tedy stačí zaplnit tabulku pouze do počtu přibližně **$\mathbf{\Theta(\sqrt{m})}$ prvků** (např. pro tabulku velikosti $m = 100$ stačí pouhých $\approx 12$ prvků).


---

## Úloha 8.13: Počet platných posloupností vkládání a topologické uspořádání
* **Zdroj:** FitWiki (Šestáková 2019, scan reálného testu `test_ag1_sestakova_29_11_2019_18_00_3.jpg`)
* **Relevantní skripta:** [Sekce 7.2 (Lineární přidávání)](https://truepeak78.github.io/ag1/07_He%C5%A1ov%C3%A1n%C3%AD/7.2_Otev%C5%99en%C3%A1_adresace/#linearni-pridavani) a [Sekce 5.1 (Topologické uspořádání orientovaných grafů)](https://truepeak78.github.io/ag1/05_Orientovan%C3%A9_grafy/5.1_Z%C3%A1kladn%C3%AD_pojmy/#def-5.3)

### Zadání
Uvažujte hešovací tabulku velikosti $m = 10$ s přihrádkami $0, 1, 2, \dots, 9$.
Při vkládání klíčů byla použita hešovací funkce:
$$h(k) = k \bmod 10$$
Případné kolize byly řešeny lineárním prohledáváním s krokem 1, tedy vyšetřovací posloupností:
$$h(k, i) = (k + i) \bmod 10, \quad i \in \{0, 1, 2, \dots\}$$

V tabulce se po vložení 7 klíčů nachází následující prvky:

| Přihrádka | 0 | 1 | 2 | 3 | 4 | 5 | 6 | 7 | 8 | 9 |
| :--- | :---: | :---: | :---: | :---: | :---: | :---: | :---: | :---: | :---: | :---: |
| **Klíč** | **97** | | | **53** | **63** | **73** | | **17** | **38** | **47** |

Kolik existuje všech různých posloupností představujících pořadí, ve kterém mohly být tyto klíče postupně vkládány do prázdné tabulky tak, aby vznikla **přesně tato zaplněná tabulka**? Svou odpověď podrobně zdůvodněte.

> [!success]- Řešení
> ### 1. Analýza primárních hešů a kolizí jednotlivých klíčů
> 
> Pro každý klíč $k$ v tabulce spočítáme jeho přirozenou (primární) pozici $h_0(k) = k \bmod 10$ a porovnáme ji se skutečnou přihrádkou, v níž se nachází:
> 
> 1. **Klíč 53:** $h(53) = 53 \bmod 10 = 3$.
>    - Je uložen přímo v přihrádce **3** $\implies 0$ kolizí.
> 2. **Klíč 63:** $h(63) = 63 \bmod 10 = 3$.
>    - Je uložen v přihrádce **4**.
>    - To znamená, že při vkládání 63 musela být přihrádka 3 již obsazena klíčem 53!
>    - $\implies$ **53 muselo být vloženo dříve než 63** ($53 \prec 63$).
> 3. **Klíč 73:** $h(73) = 73 \bmod 10 = 3$.
>    - Je uložen v přihrádce **5**.
>    - Při vkládání 73 musely být přihrádky 3 i 4 obsazeny (klíči 53 a 63).
>    - $\implies$ **63 muselo být vloženo dříve než 73** ($63 \prec 73$).
> 4. **Klíč 17:** $h(17) = 17 \bmod 10 = 7$.
>    - Je uložen přímo v přihrádce **7** $\implies 0$ kolizí.
> 5. **Klíč 38:** $h(38) = 38 \bmod 10 = 8$.
>    - Je uložen přímo v přihrádce **8** $\implies 0$ kolizí.
> 6. **Klíč 47:** $h(47) = 47 \bmod 10 = 7$.
>    - Je uložen v přihrádce **9**.
>    - Prohlédl pozice 7 a 8 a našel je obsazené.
>    - $\implies$ **17 i 38 musely být vloženy dříve než 47** ($17 \prec 47$ a $38 \prec 47$).
> 7. **Klíč 97:** $h(97) = 97 \bmod 10 = 7$.
>    - Je uložen v přihrádce **0** (po cyklickém přetečení přes index 9: $7 \to 8 \to 9 \to 0$).
>    - Prohlédl pozice 7, 8 a 9 a našel je obsazené (klíči 17, 38, 47).
>    - $\implies$ **47 muselo být vloženo dříve než 97** ($47 \prec 97$).
>
> ---
>
> ### 2. Dekompozice na nezávislé shluky (komponenty závislostí)
> 
> V tabulce jsou přihrádky **1, 2 a 6 prázdné**.
> Tyto prázdné přihrádky fungují jako neprostupné bariéry lineárního prohledávání. Klíče jsou tedy rozděleny do dvou zcela nezávislých shluků:
> 
> #### Shluk A: Přihrádky $\{3, 4, 5\}$ (klíče $53, 63, 73$)
> - Závislosti tvoří lineární řetězec:
>   $$53 \longrightarrow 63 \longrightarrow 73$$
> - Existuje **právě jedno (1)** platné pořadí vložení prvků shluku A:
>   $$(53, 63, 73)$$
>
> #### Shluk B: Přihrádky $\{7, 8, 9, 0\}$ (klíče $17, 38, 47, 97$)
> - Závislosti tvoří orientovaný acyklický graf (DAG):
>   - $17 \longrightarrow 47$
>   - $38 \longrightarrow 47$
>   - $47 \longrightarrow 97$
> - Klíče 17 a 38 jsou vzájemně nezávislé (17 hashovalo do 7, 38 do 8; žádný z nich nekolidoval s tím druhým).
> - Proto mohou do tabulky přijít v libovolném pořadí: buď 17 dříve než 38, nebo 38 dříve než 17.
> - Jakmile jsou vloženy oba, musí následovat 47 a nakonec 97.
> - Počet platných topologických uspořádání shluku B je přesně **2**:
>   1. $(17, 38, 47, 97)$
>   2. $(38, 17, 47, 97)$
>
> ---
>
> ### 3. Kombinatorické prolínání nezávislých shluků
> 
> Celkový počet vkládaných klíčů je $N = |A| + |B| = 3 + 4 = 7$.
> Protože shluk A a shluk B jsou na sobě zcela nezávislé, libovolné vložení libovolného prvku ze shluku A neovlivní ani neobsadí žádnou přihrádku ze shluku B a naopak.
> 
> Počet způsobů, jak vybrat 3 pozice pro prvky shluku A v 7-prvkové celkové posloupnosti, je dán kombinačním číslem:
> $$\binom{7}{3} = \frac{7 \cdot 6 \cdot 5}{3 \cdot 2 \cdot 1} = 35$$
> 
> Na zvolených 3 pozicích se prvky shluku A musí nacházet v jediném platném pořadí ($1$ možnost).
> Na zbývajících 4 pozicích se prvky shluku B mohou nacházet ve $2$ platných pořadích.
> 
> ### Výsledný počet posloupností
> Celkový počet platných posloupností vkládání je:
> $$\text{Počet} = \binom{7}{3} \times (\text{počet pořadí shluku A}) \times (\text{počet pořadí shluku B}) = 35 \times 1 \times 2 = \mathbf{70}$$
> 
> Existuje přesně **70 různých posloupností vkládání**, které vedou k této identické hešovací tabulce.
