Szia! Ez a diasor a Neurális Hálók "finomhangolásáról" szól. Ha az előző anyag volt a motor megépítése (hogyan működik a neuron, backpropagation), akkor ez az anyag arról szól, **hogyan vezetjük az autót, hogy ne menjen neki a falnak**.

A fő téma a **Regularizáció**, ami a gépi tanulás egyik legfontosabb fogalma: hogyan érjük el, hogy a modellünk ne csak bemagolja a tanító adatokat (**túltanulás**), hanem az éles vizsgán (új adatokon) is jól szerepeljen.

Íme a részletes, példákkal gazdagított magyarázat:

---

### 1. A Tervezés Nagy Kérdései (2-4. dia)

Mielőtt elindítjuk a tanítást, mérnöki döntéseket kell hoznunk. Nincs egyetlen "tökéletes" recept, kísérletezni kell.

**A) Mekkora legyen a hálózat? (3-4. dia)**
*   **Túl kicsi háló:** "Buta" lesz, nem tudja megtanulni az összefüggéseket (Alulilleszkedés).
*   **Túl nagy háló:** Túl okos, mindent bemagol, de nem érti a lényeget (Túlilleszkedés), és lassú.
*   **Megoldások:**
    1.  **Pruning (Metszés):** Építünk egy nagy hálót, majd ami felesleges (kis súlyú kapcsolatok), azt levágjuk.
    2.  **Konstruktív (Építő):** Kicsivel kezdünk, és addig adunk hozzá neuronokat/rétegeket, amíg javul a teljesítmény.

---

### 2. A Tanítás Finomhangolása (6-8. dia)

**B) Tanulási tényező ($\alpha$ vagy Learning Rate):**
Ez a lépéshossz a hegymászásnál (gradiens ereszkedés).
*   **Túl kicsi:** Órákig tart leérni a völgybe.
*   **Túl nagy:** Átugorjuk a minimumot, oszcillálunk.
*   **Megoldás (Adaptív $\alpha$):** Kezdjünk naggyal (hogy haladjunk), majd ahogy közeledünk a célhoz, csökkentsük, hogy pontosan beletrafáljunk.

**C) Kezdeti súlyok (Initialization):**
*   Nem indulhatunk csupa 0-val! Ha minden súly 0, minden neuron ugyanazt tanulná (szimmetria).
*   **Megoldás:** Kis véletlenszámokkal töltjük fel a súlyokat (pl. Gauss-eloszlás szerint).

---

### 3. Adatkezelés: Tanító, Validáló, Teszt (9-16. dia) – **NAGYON FONTOS**

Hogyan mérjük, hogy jó-e a modell? Nem mérhetjük azon, amin tanítottuk (az csalás lenne)!

**Az adatok felosztása:**
1.  **Tanító halmaz (Training):** Ebből tanul a gép (ebből állítjuk a súlyokat). Ez a tankönyv.
2.  **Validációs (Kiértékelő) halmaz:** Ezen mérjük menet közben, hogy állunk. Ezzel állítgatjuk a *hiperparamétereket* (pl. mikor álljunk meg). Ez a próbavizsga.
3.  **Teszt halmaz:** Ezt a végéig félretesszük. Csak egyszer használjuk, a legvégén. Ez az éles érettségi.

**Keresztvalidáció (Cross-validation) – 12-13. dia:**
Ha kevés az adatunk, luxus 20%-ot félretenni tesztnek.
*   **K-fold:** Az adatot $k$ részre vágjuk.
    *   1. kör: Az 1. rész a teszt, többi a tanító.
    *   2. kör: A 2. rész a teszt, többi a tanító.
    *   ...és így tovább. A végén átlagoljuk az eredményt. Így minden adat volt tanító is és teszt is.
*   **Leave-one-out:** (Extrém eset) Mindig csak 1 db adat a teszt, az összes többi tanító. (Nagyon pontos, de nagyon lassú).

**Batch vs. Online (16. dia):**
*   **Pontonként (Online):** Minden egyes példa után frissítjük a súlyokat. (Ideges, rángatózó tanulás).
*   **Kötegelt (Batch):** Megnézünk pl. 32 vagy 64 példát, átlagoljuk a hibát, és csak utána lépünk egyet. (Stabilabb, gyorsabb).

---

### 4. A Fő Ellenség: Túltanulás (Overfitting) és a Bias-Variance (17-24. dia)

Ez a gépi tanulás elméleti magja.

*   **Túltanulás (Overfitting):** A modellünk a tanító adatokon zseniális (közel 0 hiba), de a validációs adatokon rossz és romlik. "Bemagolta a zajt is". (19. dia jobb oldali ábra: a görbe minden egyes pöttyöt kikerül, emiatt össze-vissza kanyarog).
*   **Alulilleszkedés (Underfitting):** A modell túl buta, még a tanító adatokat sem tudja megtanulni. (19. dia bal oldali ábra: egyenest húzunk a görbe helyett).

**Bias-Variance Tradeoff (Kompromisszum) – 23. dia:**
*   **Bias (Torzítás):** Mennyire egyszerűsítjük le a világot? (Nagy bias = alulilleszkedés, túl merev modell).
*   **Variancia:** Mennyire érzékeny a modell az adatok apró változásaira? (Nagy variancia = túlilleszkedés, ha kicsit más adatot kap, teljesen mást tanul).
*   **Cél:** Megtalálni az egyensúlyt (Low Bias, Low Variance).

---

### 5. Regularizáció: A Védekezés (25-45. dia) – **VIZSGATÉTEL**

Hogyan akadályozzuk meg a túltanulást? "Megnehezítjük" a tanulást, hogy a modell kénytelen legyen az általános összefüggéseket megtalálni a magolás helyett.

#### A) Early Stopping (Korai leállás) – 26-27. dia
*   Figyeljük a hibát a **Validációs halmazon**.
*   Kezdetben mind a tanító, mind a validációs hiba csökken.
*   Egy ponton a validációs hiba elkezd **nőni** (miközben a tanító hiba még csökken). Ez a túltanulás kezdete!
*   **Teendő:** Itt azonnal állítsuk le a tanítást!

#### B) Adat Augmentáció (Bővítés) – 28. dia
*   Ha kevés az adat, csináljunk többet!
*   Képeknél: Forgassuk el, tükrözzük, vágjuk meg, színezzük át a meglévő képeket.
*   Így a modell megtanulja, hogy a macska akkor is macska, ha fejjel lefelé van.

#### C) L1 és L2 Regularizáció (Weight Decay) – 29-34. dia
*   **Ötlet:** Büntessük a nagy súlyokat! Ha a súlyok nagyok, a függvény nagyon "hullámzó" lesz (túlilleszkedés).
*   A hibafüggvényhez hozzáadunk egy büntetőtagot:
    *   **L1 (Lasso):** A súlyok abszolút értékét adjuk hozzá ($|w|$). Hatása: Sok súlyt **nullára** visz. (Ritkítja a hálót, feature selection).
    *   **L2 (Ridge):** A súlyok négyzetét adjuk hozzá ($w^2$). Hatása: Minden súlyt kicsire csökkent, de nem nullára. (Ez a gyakoribb).

#### D) Zaj hozzáadása (Noise Injection) – 35-38. dia
*   Ha zajos (pontatlan) adatokkal tanítunk, a modell kénytelen robusztusabb lenni. Nem bízhat meg vakon egyetlen pixelben sem.
*   Zajt adhatunk a bemenethez, a címkékhez, vagy akár a gradiensekhez is.

#### E) Dropout (Kiesés) – 39-40. dia – **NAGYON GYAKORI**
*   Tanítás közben véletlenszerűen **kikapcsoljuk** a neuronok egy részét (pl. 50%-át).
*   **Hatása:** A háló nem támaszkodhat egy-egy "szuper-neuronra", mert az bármikor kieshet. Minden neuronnak meg kell tanulnia a feladatot. Olyan, mintha sok különböző kisebb hálót tanítanánk egyszerre (Ensemble hatás).

#### F) Batch Normalization (41-43. dia)
*   A rétegek között normalizáljuk az adatokat (hogy az átlag 0, szórás 1 legyen).
*   Ez stabilizálja és **gyorsítja** a tanulást, és van egy kis regularizáló (túltanulás-gátló) hatása is.

---

### 6. Hiperparaméterek (46-47. dia)

Mik azok a **Hiperparaméterek**?
Azok a beállítások, amiket **NEM** a gép tanul meg, hanem **NEKÜNK** kell beállítani a tanítás előtt.
*   Pl.: Rétegek száma, neuronok száma, tanulási ráta ($\alpha$), batch méret, regularizációs erősség.

**Hangolás (Tuning):**
*   Ezeket a validációs halmazon próbálgatjuk. (Pl. Grid Search: kipróbálunk minden kombinációt).

---

### Összefoglaló a vizsgára (Ezeket vidd haza):

1.  **Túltanulás (Overfitting):** A modell túl jól teljesít a tanító adatokon, de rosszul az újakon. (Magas variancia).
2.  **Regularizáció:** Minden módszer, ami a túltanulás ellen véd.
3.  **Módszerek:**
    *   **Early Stopping:** Állj meg, ha romlik a validáció!
    *   **Dropout:** Kapcsold ki véletlenszerűen a neuronokat!
    *   **L1/L2:** Büntesd a nagy súlyokat! (L1 ritkít, L2 kicsinyít).
    *   **Augmentáció:** Forgasd/torzítsd a képeket, hogy több adatod legyen.
4.  **Adatok:** Tanító (súlyokhoz) / Validációs (hiperparaméterekhez és early stoppinghoz) / Teszt (végső méréshez).

Ha érted, miért baj a túltanulás, és hogyan segít ellene a Dropout vagy az Early Stopping, akkor érted a diasor lényegét!