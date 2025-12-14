![Alt text](/img/zsiros_hus0.png)
![alt text](/img/zsiros_hus.png)

Ez egy klasszikus **Döntési Fa (Decision Tree)** építős feladat, ahol az **Entrópia** és az **Információnyereség** számítása a lényeg.

Itt vannak a megoldások, majd a levezetés és a magyarázat.

---

### 1. MEGOLDÁSOK

**a.)** Az entrópia: **0.918 bit**

**b.)**
*   Fennmaradó (Maradék) entrópia: **0.824 bit**
*   Információnyereség: **0.094 bit**

**c.)**
*   Melyikkel érdemes tovább építeni? **A "Zsír eloszlása" attribútummal.**
*   Indoklás: Ennek nagyobb az információnyeresége (kisebb a maradék entrópiája), mivel két tiszta (0 entrópiájú) csoportot is létrehoz.

---

### 2. LEVEZETÉS LÉPÉSRŐL LÉPÉSRE

#### a.) Kezdeti Entrópia számítása

A kiinduló csomópontban 30 minta van:
*   Pozitív ($+$): 10 db
*   Negatív ($-$): 20 db

Valószínűségek:
$P(+) = \frac{10}{30} = \frac{1}{3}$
$P(-) = \frac{20}{30} = \frac{2}{3}$

Az entrópia képlete: $H(S) = -p_+ \log_2(p_+) - p_- \log_2(p_-)$

$H(S) = - \frac{1}{3} \log_2 \left(\frac{1}{3}\right) - \frac{2}{3} \log_2 \left(\frac{2}{3}\right)$
$H(S) \approx 0.528 + 0.390 = \mathbf{0.918}$

*(Megjegyzés: Ha nem tudsz fejből logaritmust számolni, a vizsgán általában elfogadják a képlet felírását, vagy "szép" számokat adnak, pl. 0.5-0.5 eloszlásnál az entrópia mindig 1).*

---

#### b.) "Zsírtartalom" vizsgálata

Megnézzük, hogyan bontja szét a 30 elemet a "Zsírtartalom" attribútum 3 ágra.

1.  **Alacsony ág:** $[0+, 3-]$ (Összesen 3 elem)
    *   Ez egy **tiszta** csoport (csak negatív van).
    *   Entrópia: $H(Alacsony) = \mathbf{0}$.
    *   Súly: $3/30$.

2.  **Közepes ág:** $[8+, 10-]$ (Összesen 18 elem)
    *   $P(+) = 8/18$, $P(-) = 10/18$.
    *   Entrópia: $H(Közepes) = - \frac{8}{18} \log_2 \frac{8}{18} - \frac{10}{18} \log_2 \frac{10}{18} \approx \mathbf{0.991}$.
    *   *(Ez majdnem 1, mert a 8 és 10 nagyon közel van egymáshoz, tehát nagy a bizonytalanság).*
    *   Súly: $18/30$.

3.  **Magas ág:** $[2+, 7-]$ (Összesen 9 elem)
    *   $P(+) = 2/9$, $P(-) = 7/9$.
    *   Entrópia: $H(Magas) = - \frac{2}{9} \log_2 \frac{2}{9} - \frac{7}{9} \log_2 \frac{7}{9} \approx \mathbf{0.764}$.
    *   Súly: $9/30$.

**Maradék Entrópia (Weighted Average Entropy):**
Súlyozott átlagot számolunk:
$H_{maradék} = \frac{3}{30}(0) + \frac{18}{30}(0.991) + \frac{9}{30}(0.764)$
$H_{maradék} = 0 + 0.5946 + 0.2292 = \mathbf{0.8238}$

**Információnyereség (Gain):**
$Gain = H(Kezdeti) - H_{maradék}$
$Gain = 0.918 - 0.8238 \approx \mathbf{0.094}$

---

#### c.) A következő lépés kiválasztása

Most csak a **Közepes** ággal foglalkozunk, ahol a minta: $[8+, 10-]$.
Ennek az entrópiája (ahogy fent számoltuk): $H_{start} \approx 0.991$.
A cél, hogy a lehető legjobban csökkentsük ezt a számot (minél közelebb a 0-hoz).

**1. opció: Rostok iránya**
*   *Véletlenszerű* $[0, 7]$: Tiszta csoport $\to H=0$. (Súly: $7/18$)
*   *Konzisztens* $[8, 3]$: Vegyes csoport. $H(- \frac{8}{11} \dots) \approx 0.845$. (Súly: $11/18$)
*   **Maradék entrópia:** $\frac{7}{18}(0) + \frac{11}{18}(0.845) \approx \mathbf{0.516}$

**2. opció: Zsír eloszlása**
*   *Koncentrált* $[0, 6]$: Tiszta csoport $\to H=0$. (Súly: $6/18$)
*   *Márványos* $[4, 0]$: Tiszta csoport $\to H=0$. (Súly: $4/18$)
*   *Homogén* $[4, 4]$: Teljesen kevert $\to H=1$. (Súly: $8/18$)
*   **Maradék entrópia:** $\frac{6}{18}(0) + \frac{4}{18}(0) + \frac{8}{18}(1) = \frac{8}{18} \approx \mathbf{0.444}$

**Döntés:**
Összehasonlítjuk a maradék entrópiákat:
*   Rostok: $0.516$
*   Zsír: $0.444$
Mivel **$0.444 < 0.516$**, a **Zsír eloszlása** jobb eredményt ad (kisebb a bizonytalanság), ezért ezt választjuk.

---

### 3. ELMÉLETI HÁTTÉR ÉS ÉRTELMEZÉS

Ez a feladat a **Döntési Fák (Decision Trees)** építésének a "motorja". Az ID3 vagy C4.5 algoritmusok pontosan így működnek.

#### Mi az az Entrópia?
Az entrópia a **rendetlenség** vagy **bizonytalanság** mérőszáma.
*   **H = 0:** Tökéletes rend. Mindenki egyforma (pl. $[10+, 0-]$). Már tudjuk a választ, nem kell tovább kérdezni.
*   **H = 1:** Teljes káosz. A minták fele pozitív, fele negatív (pl. $[5+, 5-]$). Itt a legnehezebb tippelni.

A célunk a fa építésekor, hogy a rendezetlen, magas entrópiájú halmazt kérdésekkel (attribútumokkal) szétbontsuk tiszta, 0 entrópiájú csoportokra.

#### Mi az az Információnyereség (Information Gain)?
Ez azt méri, hogy egy kérdés (attribútum) mennyivel **csökkentette** a bizonytalanságot.
*   `Kezdeti Entrópia` - `Maradék Entrópia`
*   Ahol a `Maradék Entrópia` az alágak entrópiáinak súlyozott átlaga.
*   A mohó algoritmus mindig azt az attribútumot választja, ahol ez a nyereség a **legnagyobb** (tehát a maradék entrópia a legkisebb).

#### A c) feladat logikája (Számolás nélkül is látható!)
Ha ránézel a c) feladat táblázatára, azonnal látszik, miért a "Zsír eloszlása" a nyerő, bonyolult logaritmusok nélkül is:

*   **Rostok iránya:** Létrehoz 1 tiszta csoportot (Véletlenszerű: csak negatív), de a másik csoport (Konzisztens) még mindig eléggé kevert (8 vs 3).
*   **Zsír eloszlása:** Létrehoz **2 tiszta csoportot is**! (Koncentrált: csak negatív, Márványos: csak pozitív). Csak a Homogén csoport marad bizonytalan.

Mivel a "Zsír eloszlása" több mintát tud tökéletesen osztályozni (6+4=10 db tiszta minta), mint a "Rostok" (csak 7 db tiszta minta), ezért intuitívan is érezzük, hogy az a jobb választás. A számítás ezt igazolta.

Teljesen jogos a kérdés! Ez a jelölés a **Döntési Fák** legfontosabb alapköve, de ha nincs elmagyarázva, csak egy rakás számhalmaznak tűnik.

Tegyük teljesen tisztába, mit jelentenek ezek a zárójelek!

### A Kód: `[Pozitív db, Negatív db]`

A feladat szövege azt mondja, hogy összesen **30 db termékünk** (műhúsunk) van.
Ezeket két csapatra osztották a tesztelők:
1.  **Pozitív (+):** "Hihető" (Jó a termék). Ebből van **10 db**.
2.  **Negatív (-):** "Nem meggyőző" (Rossz a termék). Ebből van **20 db**.

Tehát a kiinduló állapotunk: **`[10, 20]`**.
*(10 jó, 20 rossz).*

---

### Mit csinál a Döntési Fa? (A Szétválogatás)

A fa lényege, hogy felteszünk egy kérdést (pl. "Mennyi a zsírtartalma?"), és ez alapján **szétválogatjuk** a 30 terméket kisebb csoportokba (kosarakba).

Nézzük a feladatban lévő táblázatot (Zsírtartalom):

#### 1. "Alacsony" csoport: `[0, 3]`
*   Ez azt jelenti, hogy a 30 termékből **3 darabnak** volt alacsony a zsírtartalma.
*   Megnéztük ezt a 3 darabot, és:
    *   **0 db** volt közülük Jó (Pozitív).
    *   **3 db** volt közülük Rossz (Negatív).
*   **Ezért írtuk:** `[0, 3]`.
*   *Ez egy nagyon jó csoport, mert "tiszta"! Ha tudjuk, hogy alacsony a zsír, azonnal tudjuk, hogy rossz a termék.*

#### 2. "Közepes" csoport: `[8, 10]`
*   A 30 termékből **18 darabnak** volt közepes a zsírtartalma (8+10).
*   Megnéztük őket:
    *   **8 db** volt Jó.
    *   **10 db** volt Rossz.
*   **Ezért írtuk:** `[8, 10]`.
*   *Ez egy "koszos", kevert csoport. Itt még nagy a bizonytalanság, majdnem fele-fele.*

#### 3. "Magas" csoport: `[2, 7]`
*   A maradék **9 darabnak** magas volt a zsírtartalma.
*   Ebből:
    *   **2 db** volt Jó.
    *   **7 db** volt Rossz.
*   **Ezért írtuk:** `[2, 7]`.

---

### Összefoglalva

Amikor azt látod a levezetésben, hogy **`[8, 10]`**, az csak annyit jelent:
*"Ebben az ágban (csoportban) jelenleg **8 db pozitív** és **10 db negatív** példa van."*

Ezekből a számokból számoljuk az **arányokat** (valószínűségeket) az entrópiához:
*   $P(+) = \frac{8}{18}$
*   $P(-) = \frac{10}{18}$

Most már tisztább a kép?