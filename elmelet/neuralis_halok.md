Rendben, vágjunk bele a **Neurális Hálók** és a kapcsolódó témák elméleti alapjaiba! Ez a gépi tanulás egyik legizgalmasabb és legmodernebb területe. A diák a "klasszikus" lineáris modellektől indulnak, és eljutnak a többrétegű neurális hálókig.

---

### 1. A Nagy Kép: Mi a célunk? (A Tanulás Folyamata)

A legtöbb felügyelt tanulási probléma (így a neurális hálóké is) egy **optimalizálási feladat**.

*   **Cél:** Találjunk egy olyan modellt, ami a bemeneti adatokból (`x`) a lehető legjobban megjósolja a kimenetet (`y`).
*   **A Modell:** A modell "tudását" a **súlyok (`w`)** tárolják. A tanulás lényege, hogy megtaláljuk a **legjobb `w` súlyvektort**.
*   **Hogyan tanulunk?**
    1.  Definiálunk egy **veszteségfüggvényt (Loss/Error Function)**, ami megméri, hogy a modell jóslata mennyire tér el a valóságtól.
    2.  A célunk, hogy megtaláljuk azokat a súlyokat (`w`), amik ezt a **veszteséget minimalizálják**.

---

### 2. A legegyszerűbb modell: Lineáris Modellek

Mielőtt a neurális hálókra ugranánk, a diák bemutatják az alapötletet a legegyszerűbb eseteken.

#### A) Lineáris Regresszió (Számjóslás)
*   **Feladat:** Folyamatos értékeket jósolunk (pl. egy ház árát a mérete alapján).
*   **Modell:** Egy egyenest (vagy több dimenzióban hipersíkot) illesztünk a pontokra.
    *   `y = w_1*f_1(x) + w_2*f_2(x) + ...`
*   **Tanulás (Optimalizálás):** A legjobb egyenest a **Legkisebb Négyzetek (Least Squares)** módszerével találjuk meg.
    *   **Veszteségfüggvény:** A jósolt és a valós pontok közötti függőleges távolságok **négyzetösszege**. Célunk ezt minimalizálni.

#### B) Lineáris Osztályozó (Kategóriajóslás) - A Perceptron
*   **Feladat:** Két kategória (pl. Spam/Nem Spam) közé húzunk egy döntési határt.
*   **Modell (Perceptron):**
    1.  Vesszük a bemeneti jellemzőket (`f(x)`).
    2.  Mindegyiket megszorozzuk egy súllyal (`w_i`).
    3.  Ezeket összeadjuk. Ezt hívjuk **aktivációnak**.
    4.  Ha az aktiváció egy küszöb (általában 0) felett van, akkor az egyik osztályba soroljuk (+1), ha alatta, akkor a másikba (-1).
*   **Tanulás (Perceptron tanulási szabály):**
    *   Veszünk egy példát. Ha a modell jól tippelt, nem csinálunk semmit.
    *   Ha rosszul tippelt, a súlyokat "megbüntetjük": egy kicsit elmozdítjuk őket a helyes döntés irányába.

**A Perceptron problémái:**
*   Csak **lineárisan szétválasztható** problémákat tud megoldani (ahol egyetlen vonallal el lehet választani a két csoportot).
*   Nem ad valószínűséget, csak egy kemény "igen/nem" döntést.
*   Érzékeny a zajra.

#### C) Logisztikus Regresszió (Valószínűségjóslás)
Ez a Perceptron "okosabb" testvére.
*   **Modell:** Az aktivációt (a súlyozott összeget) nem egy kemény küszöbhöz hasonlítja, hanem átküldi egy **szigmoid (logisztikus) függvényen**.
*   **Szigmoid függvény:** Egy "S" alakú görbe, ami bármilyen bemeneti számot 0 és 1 közötti értékké alakít.
*   **Eredmény:** A kimenet egy **valószínűség** lesz (pl. 85% eséllyel Spam).
*   **Tanulás (Maximum Likelihood):** A súlyokat úgy állítjuk be, hogy a modell által adott valószínűségek a lehető "leginkább hihetővé" tegyék a tanító adathalmazt.

---

### 3. A Neurális Háló: Sok Perceptron Egymáson

**Mi van, ha a probléma nem lineárisan szétválasztható?** (Pl. egy XOR probléma, ahol a pontok "sakktábla" szerűen helyezkednek el).
**Megoldás:** Rakjunk egymás után több réteg Perceptront!

#### Felépítés (MLP - Multi-Layer Perceptron)
*   **Bemeneti réteg:** Ide érkeznek az adatok (jellemzők).
*   **Rejtett rétegek (Hidden Layers):** A "mágia" itt történik. Minden neuron (egység) a saját súlyait tanulja meg, és az előző réteg kimenetét kapja bemenetként. Ezek a rétegek képesek bonyolult, nem-lineáris összefüggéseket, köztes "jellemzőket" megtanulni.
*   **Kimeneti réteg:** Itt születik meg a végső jóslat.

**Univerzális Függvényapproximációs Tétel (slide 10-12):**
Ez egy elképesztően fontos elméleti eredmény. Azt mondja ki, hogy egyetlen rejtett réteggel rendelkező neurális háló (ha elég neuron van benne) **bármilyen folytonos függvényt** tetszőleges pontossággal képes megközelíteni. Ez adja a neurális hálók erejét.

---

### 4. A Neurális Hálók Tanítása: Gradient Descent és Backpropagation

Hogyan állítjuk be a súlyokat egy ilyen bonyolult, sokrétegű rendszerben?

#### A) Gradient Descent (Gradiens ereszkedés) - A "Hegymászó"
*   **A Cél:** Minimalizálni a veszteségfüggvényt.
*   **Az Ötlet:** Képzeljük el a veszteséget egy hegyvidéki tájként. A súlyok a mi pozíciónk (koordinátáink). Meg akarjuk találni a legmélyebb völgyet.
    *   **Lépés:** Bármely pontban állunk, megmérjük, melyik irányba **legmeredekebb a lejtő**. Ezt a **gradiens** (a parciális deriváltak vektora) adja meg.
    *   Egy apró lépést teszünk ebbe az irányba.
    *   Ismételjük, amíg el nem érjük a völgy alját (a minimumot).
*   **A frissítési szabály:** `Új_súly = Régi_súly - tanulási_ráta * Gradiens`

#### B) Backpropagation (Hibavisszaterjesztés) - A "Felelősségre vonás"
*   **A Probléma:** A Gradient Descenthez szükségünk van a gradiensre, azaz hogy a hiba hogyan függ az **összes** súlytól. A kimeneti réteg súlyainál ez könnyű. De mi a helyzet a belső, rejtett rétegek súlyaival? Hogyan tudjuk, hogy egy belső neuron mennyire "hibás"?
*   **A Megoldás (Backpropagation):**
    1.  **Forward Pass (Előre terjesztés):** Végigszámoljuk a hálót a bemenettől a kimenetig, és megkapjuk a jóslatot.
    2.  **Hiba kiszámítása:** Összehasonlítjuk a jóslatot a valósággal a kimeneten.
    3.  **Backward Pass (Visszafelé terjesztés):** A hibát **visszafelé terjesztjük** a hálózaton, rétegről rétegre. Minden neuron a láncszabály segítségével megkapja a saját "hiba-részét" attól a neurontól, amelyiknek ő jelet adott.
    4.  **Súlyok frissítése:** Amikor minden neuron tudja a saját "hiba-részét", a Gradient Descent szabállyal frissítjük a bemeneti súlyait.

---

### Összefoglaló a vizsgára:

*   **A tanulás célja:** A **veszteségfüggvény minimalizálása** a súlyok (`w`) állítgatásával.
*   **Lineáris modellek:** Az alap, ahol a kimenet a bemenetek súlyozott összege. A **Perceptron** a legegyszerűbb osztályozó. A **Logisztikus regresszió** már valószínűséget jósol.
*   **Neurális háló:** Egymásra épített Perceptron-szerű rétegek, amik **nem-lineáris** problémákat is meg tudnak oldani. Az **Univerzális Approximációs Tétel** garantálja az erejüket.
*   **Gradient Descent:** Az optimalizálási algoritmus, ami a "legmeredekebb lejtő" elvét követve keresi a hiba minimumát.
*   **Backpropagation:** Az algoritmus, amivel **hatékonyan kiszámoljuk a gradienst** egy többrétegű hálózatban, a hibát a kimenettől visszafelé terjesztve.