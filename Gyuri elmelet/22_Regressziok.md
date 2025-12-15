Rendben, vettem! Ez a diasorozat a **Lineáris Modellek (Linear Models)** és a **Neurális Hálók** (nagyon) alapjairól szól, különös tekintettel a **Regresszióra** és az **Osztályozásra**.

Ez a matekja az AI-nak. Itt látjuk először, hogyan alakul át a "tanulás" egy optimalizálási problémává (hibaminimalizálássá).

Íme a részletes, példákkal gazdagított magyarázat:

---

### 1. Lineáris Regresszió (Linear Regression) (3-8. dia) – **ALAPFOGALOM**

Mi a feladat? Vannak adatpontjaink (pl. lakásméret vs. ár), és szeretnénk megjósolni az árat egy új lakásnál.

**A Modell (4. dia):**
Feltételezzük, hogy az összefüggés **lineáris** (egyenes vonallal leírható).
$$y = w_0 + w_1 \cdot x$$
*   $x$: A bemenet (pl. méret).
*   $y$: A kimenet (pl. ár).
*   $w_1$: Súly (meredekség). Mennyivel drágább egy nm?
*   $w_0$: Bias (eltolás). Mennyi az alapár 0 nm-nél?

**Több dimenzióban:** $y = \sum w_i \cdot f_i(x)$. (Súlyozott összeg).

**A Hiba (Error) – 7. dia:**
A modellünk nem lesz tökéletes. A hiba a **valós érték ($y$)** és a **becsült érték ($\hat{y}$)** közötti különbség (reziduális).
*   **Célfüggvény (Loss Function):** A hibák négyzetösszegét akarjuk minimalizálni (**Least Squares**).
    $$E = \sum (y_i - \hat{y}_i)^2$$
*   *Miért négyzet?* Hogy a negatív hiba (alábecslés) ne oltsa ki a pozitívat (túlbecslés), és hogy a nagy hibákat jobban büntessük.

**Tanulás (8. dia):**
Hogyan találjuk meg a legjobb $w$ súlyokat?
*   Deriváljuk a hibafüggvényt $w$ szerint.
*   Ahol a derivált 0, ott van a minimum. (Analitikus megoldás).

---

### 2. Lineáris Osztályozás (Linear Classification) (9-17. dia)

Mi van, ha nem számot (árat) akarunk jósolni, hanem kategóriát (pl. SPAM vagy NEM SPAM)?

**Jellemző vektorok (Feature Vectors) – 10. dia:**
A bemenetet (pl. email) számokká alakítjuk.
*   Email $\to$ [szavak száma, "ingyen" szó db, feladó ismerős-e...].
*   Ez az $f(x)$ vektor.

**A Perceptron (12. dia) – Az ős-neurális háló:**
*   Vesszük a jellemzők súlyozott összegét: $S = \sum w_i \cdot f_i(x)$.
*   **Aktivációs függvény:** Egy lépcsőfüggvényt alkalmazunk rá.
    *   Ha $S > 0 \to$ Pozitív osztály (+1).
    *   Ha $S \le 0 \to$ Negatív osztály (-1).
*   Ez geometriailag egy **egyest** (vagy síkot) húz az adatok közé. Az egyik oldalon vannak a pozitívak, a másikon a negatívak (15. dia).

**Tanulás: Perceptron Update Rule (19-20. dia) – **VIZSGAPÉLDA!****
Hogyan találjuk meg a súlyokat, amik elválasztják a piros pöttyöket a kékektől?
1.  Kezdjük véletlen (vagy 0) súlyokkal.
2.  Veszünk egy példát. Ha a gép eltalálta $\to$ Nem csinálunk semmit.
3.  **Ha tévedett:**
    *   Ha pozitívat kellett volna mondania, de negatívat mondott: A súlyvektorhoz **hozzáadjuk** a példa vektorát. ($w \leftarrow w + f(x)$). (Ezzel a súlyvektor iránya közelebb kerül a példához).
    *   Ha negatívat kellett volna mondania, de pozitívat mondott: A súlyvektorból **kivonjuk** a példa vektorát. ($w \leftarrow w - f(x)$).
4.  Ezt ismételjük, amíg el nem fogynak a hibák.
*   **Korlát:** Ez csak akkor működik, ha az adatok **lineárisan elválaszthatók** (húzható közéjük egyenes). Ha nem (pl. XOR probléma), a perceptron sosem áll meg (25. dia).

---

### 3. Logisztikus Regresszió (Logistic Regression) (29-30. dia) – **A VALÓSZÍNŰSÉGI DÖNTÉS**

A Perceptron túl "kemény": vagy +1, vagy -1. Mi van, ha bizonytalanok vagyunk? ("80% hogy spam").

**Szigmoid (Sigmoid) függvény (29. dia):**
A "kemény" lépcsőfüggvény helyett egy "puha" S-alakú görbét használunk.
$$\sigma(z) = \frac{1}{1 + e^{-z}}$$
*   Ez a függvény bármilyen számot (a súlyozott összeget) a **[0, 1]** tartományba nyom össze.
*   Ezt értelmezhetjük **valószínűségként**: $P(Y=+1 \mid x)$.

**Softmax (33. dia):**
Ha nem 2, hanem több osztály van (pl. kutya, macska, egér), a szigmoidot kiterjesztjük.
*   Minden osztályhoz számolunk egy pontszámot ($e^{z_i}$).
*   Ezeket elosztjuk az összegükkel (normalizálás).
*   Így az osztályok valószínűségeinek összege 1 lesz.

---

### Összefoglaló a vizsgára (Ezeket vidd haza):

1.  **Lineáris Regresszió:** Számot jósolunk. A hiba a négyzetes eltérés. Analitikusan megoldható.
2.  **Perceptron:** Osztályoz (Igen/Nem). Súlyozott összeg + Lépcsőfüggvény.
    *   **Tanulás:** Ha hibázik, a súlyokat a hiba irányába módosítjuk ($w + f(x)$ vagy $w - f(x)$).
    *   Csak lineárisan elválasztható adatokon működik.
3.  **Logisztikus Regresszió:** Valószínűséget jósol. Súlyozott összeg + **Szigmoid** függvény.
    *   Nem analitikusan oldjuk meg, hanem **Gradiens ereszkedéssel** (lásd következő téma).
4.  **Softmax:** Többosztályos osztályozás valószínűségeihez.

Ha érted a **Perceptron súlyfrissítését** (hozzáad/kivon), és hogy miért jobb a **Szigmoid** (mert deriválható és valószínűséget ad), akkor a lényeget tudod!