Rendben, vettem! Ez a diasor a **Modellstruktúra és Paraméterezés** témakörét járja körbe, különös tekintettel a **Naiv Bayes-hálóra**.

Ez az anyag az előző (Valószínűségi Hálók) folytatása, de most már nemcsak a háló felépítését nézzük, hanem azt is, hogyan "tanítjuk meg" neki a számokat (paramétereket) az adatokból.

Íme a részletes, példákkal gazdagított magyarázat:

---

### 1. Osztályozás (Classification) alapjai (3-6. dia)

A **Felügyelt Tanulás** (Supervised Learning) egyik legfontosabb feladata.
*   **Feladat:** Van egy bemenetünk (pl. egy kép, egy e-mail, egy orvosi lelet), és el kell döntenünk, melyik kategóriába tartozik (címke).
*   **Példa (SPAM-szűrő - 4. dia):**
    *   *Input:* Egy e-mail szövege.
    *   *Output:* "SPAM" (szemét) vagy "HAM" (hasznos).
    *   *Módszer:* Megnézzük a jellemzőket (szavak: "ingyen", "nyeremény", csupa nagybetű, stb.).
*   **Tanítás (Learning):** Van egy nagy halmazunk régi e-mailekből, amiket emberek már felcímkéztek. Ebből tanulja meg a gép a szabályokat.

---

### 2. Modellalapú Osztályozás: Naiv Bayes (7-12. dia) – **KULCSFOGALOM**

Hogyan építünk ehhez modellt?
*   **Generatív modell:** Nemcsak azt tanuljuk meg, hogy "ha X, akkor Y", hanem megpróbáljuk modellezni, hogyan "keletkeznek" az adatok.
    *   Feltételezzük, hogy van egy rejtett ok (pl. a levélíró szándéka: SPAM vagy HAM), és ez generálja a szavakat.

**A Naiv Bayes-háló szerkezete (8. dia):**
*   Egyetlen gyökércsomópont: **Osztály ($Y$)** (pl. SPAM/HAM).
*   Sok gyerekcsomópont: **Jellemzők ($F_1 \dots F_n$)** (pl. szavak).
*   **Naiv feltételezés:** A jellemzők **feltételesen függetlenek** egymástól, ha ismerjük az osztályt.
    *   *Magyarul:* Ha tudom, hogy ez egy SPAM levél, akkor az, hogy szerepel benne a "Viagra" szó, nem befolyásolja annak az esélyét, hogy szerepel-e benne a "Lottó" szó. (Ez a valóságban nem teljesen igaz, de egyszerűsíti a számolást).

**A Képlet (12. dia - TUDD FEJBŐL!):**
$$P(Y, W_1, \dots, W_n) = P(Y) \cdot \prod_{i=1}^n P(W_i \mid Y)$$
*   Egy levél valószínűsége = (Mennyire gyakori a SPAM?) $\times$ (Mennyire gyakori a "szia" szó SPAM-ben?) $\times$ (Mennyire gyakori a "nyertél" szó SPAM-ben?)...

---

### 3. Paraméterbecslés (Parameter Estimation) (15-21. dia)

Honnan vesszük a számokat ($P(Y)$ és $P(W_i \mid Y)$)? Az adatokból!

**A) Maximum Likelihood Becslés (MLE - 19-20. dia):**
*   Ez a "józan paraszti ész" módszere.
*   **Elv:** Azt a paramétert választjuk, ami mellett a legvalószínűbb, hogy pont ezeket az adatokat láttuk.
*   **Gyakorlatban:** Egyszerűen megszámoljuk az előfordulásokat (relatív gyakoriság).
    *   *Példa (Rajszög - 17. dia):* Feldobom 5-ször. 3-szor fej (csúcsával felfelé), 2-szer írás (oldalára dőlve).
    *   MLE becslés: $P(Fej) = 3/5 = 0.6$.
    *   Ez matematikailag levezethető (a derivált nullává tételével - 20. dia), de az eredmény mindig a relatív gyakoriság.

**A probléma az MLE-vel (14. dia - Túltanulás/Overfitting):**
Mi van, ha egy szó *soha* nem szerepelt a tanító SPAM levelekben (pl. "krokodil"), de bejön egy új SPAM levél ezzel a szóval?
*   $P("krokodil" \mid SPAM) = 0/1000 = 0$.
*   Mivel összeszorozzuk a valószínűségeket, az egész szorzat **nulla** lesz!
*   A modell azt mondja: "Ez 0% eséllyel SPAM", csak egyetlen ismeretlen szó miatt. Ez hiba.

---

### 4. Simítás (Smoothing) – A megoldás (22-23. dia) – **VIZSGATIPP**

Hogy elkerüljük a 0 valószínűséget, "csalunk" egy kicsit.

**Laplace Simítás (Add-one smoothing):**
*   Úgy teszünk, mintha minden lehetséges szót láttunk volna már **még egy plusz alkalommal**.
*   **Képlet:**
    $$P(x) = \frac{\text{count}(x) + k}{N + k \cdot |X|}$$
    *   $count(x)$: Hányszor láttuk valójában.
    *   $k$: A simítás erőssége (pl. $k=1$).
    *   $N$: Összes eset száma.
    *   $|X|$: Lehetséges kimenetelek száma (hogy a nevező korrigálja a számláló növekedését).
*   *Példa (Rajszög):* 3 fej, 2 írás. ($N=5$).
    *   Eredeti: $P(Fej) = 3/5$.
    *   Laplace ($k=1$): Úgy számolunk, mintha lenne +1 fej és +1 írás.
    *   Új számok: $3+1=4$ fej, $2+1=3$ írás. Összesen $5+2=7$ eset.
    *   Simított becslés: $P(Fej) = 4/7 \approx 0.57$. (Kicsit közelebb húzza az 50%-hoz).

**Miért jó ez?**
Mert így a "krokodil" valószínűsége nem 0 lesz, hanem egy nagyon pici pozitív szám. Így nem rontja el az egész szorzatot, a többi szó (pl. "Viagra") még mindig eldöntheti, hogy SPAM-e.

---

### 5. Tuning és Validáció (24-25. dia)

Honnan tudjuk, mennyi legyen a $k$ (simítás mértéke)?
*   Nem a tanító adatokon döntjük el (ott a $k=0$ lenne a legjobb).
*   Nem a teszt adatokon (az csalás lenne).
*   **Megoldás:** Különítünk el egy **Validációs halmazt** (Validation Set).
    1.  Tanítjuk a modellt a Tanító halmazon különböző $k$ értékekkel.
    2.  Kipróbáljuk őket a Validációs halmazon.
    3.  Azt a $k$-t választjuk, ami a Validáción a legjobb.
    4.  A végső tesztet a Teszt halmazon végezzük (amit eddig nem láttunk).

---

### 6. Kiértékelés: Baseline (Alapmodell) (26. dia)

Hogyan tudjuk, hogy a modellünk jó-e? Mihez hasonlítsuk?
*   **Baseline (Alapmodell):** A lehető legegyszerűbb buta módszer.
*   **Leggyakoribb címke (Most Frequent Class):**
    *   Ha az e-mailek 90%-a HAM, akkor a buta modell mindenkire azt mondja: "HAM".
    *   Ennek a pontossága 90%.
    *   Ha a mi okos modellünk 91%-ot ér el, akkor alig jobb a semminél!
    *   Ha a modellünk 60%-ot ér el, akkor rosszabb, mint a vak tippelés.

---

### Összefoglaló a vizsgára (Ezeket vidd haza):

1.  **Naiv Bayes elve:** Minden jellemző független a többitől, ha ismerjük az osztályt. (Ezért "naiv", és ezért könnyű számolni).
2.  **Generatív modell:** $P(Osztály) \cdot \prod P(Jellemző \mid Osztály)$.
3.  **MLE (Maximum Likelihood):** Relatív gyakoriság alapú becslés.
4.  **Nulla valószínűség problémája:** Ha valami nem volt a tanítóhalmazban, az MLE 0-t ad, ami "kinyírja" a modellt.
5.  **Laplace simítás:** Mindenhez hozzáadunk $k$-t, hogy sose legyen 0.
6.  **Adatok felosztása:** Tanító (paraméterekhez) -> Validációs (hiperparaméterekhez, pl. $k$) -> Teszt (végső méréshez).

Ez a diasor a statisztikai tanulás alapjait rakja le. Ha érted a **nulla valószínűség problémáját** és a **simítás** logikáját, akkor a legfontosabb részt tudod!