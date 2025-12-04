Rendben, nézzük meg ezt az anyagrészt is a szokásos részletességgel! A diák a **Felügyelt Tanulás modelljeinek kiértékeléséről** szólnak. Ez egy kulcsfontosságú téma, mert hiába tanítunk be egy modellt, ha nem tudjuk objektíven megmérni, hogy az **valóban jó-e**.

A központi kérdés: **"De mi a jó döntés?"** (Slide 4)

---

### 1. A Nagy Kép: A Probléma felállítása

A felügyelt tanulásnál általában **osztályozási (klasszifikációs)** problémákat oldunk meg. A legegyszerűbb eset a **bináris osztályozás**, ahol két kategória van.

*   **Valóság (Tény, `f(x)`):** A dolog ténylegesen micsoda. (Pl. a páciens **beteg** vagy **egészséges**).
*   **Modell (Hipotézis, `h(x)`):** Amit a mi MI modellünk **jósol**. (Pl. a modell azt mondja: "szerintem beteg").

A célunk, hogy a modell jóslata (`h(x)`) minél jobban egyezzen a valósággal (`f(x)`). De mit jelent az, hogy "jól egyezik"? Erre kellenek a metrikák.

---

### 2. Az Alap: A Konfúziós Mátrix (Slide 5-7)

Ez a kiértékelés alfája és omegája. Egy egyszerű 2x2-es táblázat, ami szembeállítja a valóságot a jóslatunkkal.

A négy lehetséges kimenetel (orvosi példával):

1.  **Valós Pozitív (True Positive - TP):**
    *   **Valóság:** Beteg.
    *   **Jóslat:** Beteg.
    *   **Eredmény:** HELYES találat. Jól ismertük fel a bajt.

2.  **Valós Negatív (True Negative - TN):**
    *   **Valóság:** Egészséges.
    *   **Jóslat:** Egészséges.
    *   **Eredmény:** HELYES elutasítás. Jól ismertük fel, hogy nincs baj.

3.  **Hamis Pozitív (False Positive - FP) - 1-es típusú hiba:**
    *   **Valóság:** Egészséges.
    *   **Jóslat:** Beteg.
    *   **Eredmény:** TÉVES riasztás. Feleslegesen ijesztettünk rá valakire. ("Hamis riadó").

4.  **Hamis Negatív (False Negative - FN) - 2-es típusú hiba:**
    *   **Valóság:** Beteg.
    *   **Jóslat:** Egészséges.
    *   **Eredmény:** ELNÉZETT támadás. Nem vettük észre a bajt, ez a legveszélyesebb hiba.

**A Mátrix:**

| | Valóság: Beteg | Valóság: Egészséges |
| :--- | :--- | :--- |
| **Jóslat: Beteg** | TP | FP |
| **Jóslat: Egészséges**| FN | TN |

---

### 3. A "Jósági Mutatók" (Metrikák)

A konfúziós mátrix számaiból különféle arányszámokat (mutatókat) képezhetünk, amik a modell különböző aspektusait mérik.

#### Alapvető arányok:

*   **Accuracy (Pontosság, ACC):**
    *   **Kérdés:** Az összes döntés hány százaléka volt helyes?
    *   **Képlet:** `(TP + TN) / (Összes eset)`
    *   **Vigyázat!** Ez a leggyakoribb, de **legfélrevezetőbb** mutató. Ha a betegség nagyon ritka (pl. 99% egészséges, 1% beteg), akkor egy "buta" modell, ami mindenkire azt mondja, hogy egészséges, 99%-os pontosságú lesz, pedig a lényeget (a betegeket) egyáltalán nem ismeri fel.

#### A "Fontos" mutatópár: Recall és Precision

Ezek sokkal árnyaltabb képet adnak, különösen, ha az osztályok aránytalanok.

*   **Recall (Érzékenység, True Positive Rate - TPR):**
    *   **Kérdés:** Az **összes ténylegesen beteg** ember közül hányat találtunk meg?
    *   **Képlet:** `TP / (TP + FN)`
    *   **Fókusz:** A **hamis negatív (FN)** hibák minimalizálása.
    *   **Mikor fontos?** Amikor egyetlen pozitív esetet sem hagyhatunk ki (pl. rákdiagnosztika, terrorista-szűrés). Inkább legyen 10 téves riasztásunk (FP), mint hogy egyetlen beteget (FN) hazaküldjünk.

*   **Precision (Precizitás, Positive Predictive Value - PPV):**
    *   **Kérdés:** Azok közül, **akiket betegnek jelöltünk**, hányan voltak tényleg betegek?
    *   **Képlet:** `TP / (TP + FP)`
    *   **Fókusz:** A **hamis pozitív (FP)** hibák minimalizálása.
    *   **Mikor fontos?** Amikor a pozitív jóslatnak komoly következménye van (pl. spam szűrés - ne kerüljön fontos levél a spam mappába; bűnösség megállapítása). Inkább csússzon át egy-két spam (FN), minthogy egy fontos levelet (FP) kidobjunk.

**Kompromisszum:** A Recall és a Precision általában egymás ellen dolgozik. Ha növeled az egyiket, a másik csökken. A jó modell megtalálja a kettő között az optimális egyensúlyt.

---

### 4. A Modell finomhangolása: A Küszöb (Threshold)

Sok modell (pl. logisztikus regresszió, neurális hálók) nem egy sima "beteg/egészséges" címkét ad, hanem egy **valószínűséget** (pl. "80% eséllyel beteg"). Hol húzzuk meg a határt (küszöböt)? 50%-nál? (Slide 17-18)

*   **A küszöb mozgatásának hatása:**
    *   Ha **alacsonyra** tesszük a küszöböt (pl. 10% felett már mindenki beteg): A **Recall magas** lesz (mindenkit megtalálunk), de a **Precision alacsony** (sok lesz a téves riasztás).
    *   Ha **magasra** tesszük a küszöböt (pl. csak 95% felett mondjuk, hogy beteg): A **Precision magas** lesz (akire azt mondjuk, az szinte biztosan beteg), de a **Recall alacsony** (sok enyhe esetet elnézünk).

#### ROC és AUC (Receiver Operating Characteristic)

*   **ROC görbe (slide 19-21):** Egy grafikon, ami megmutatja, hogyan változik a **TPR (Recall)** és az **FPR (False Positive Rate)** a küszöbérték változtatásával.
    *   A **tökéletes modell** a bal felső sarokba ér (100% Recall, 0% téves riasztás).
    *   A véletlen tippelés az átló.
*   **AUC (Area Under the Curve - Görbe alatti terület):** A ROC görbe alatti terület. Egyetlen szám, ami összefoglalja a modell teljesítményét az összes lehetséges küszöbön.
    *   AUC = 1.0: Tökéletes modell.
    *   AUC = 0.5: Használhatatlan modell (véletlen tippelés).
    *   **Minél nagyobb az AUC, annál jobb a modell.**

---

### 5. A Hibák Költsége (Slide 23-30)

A valóságban a hibák nem egyformák.
*   **Hamis Negatív (FN) költsége ($C_{01}$):** Egy beteg hazaküldésének költsége (emberélet, későbbi drágább kezelés).
*   **Hamis Pozitív (FP) költsége ($C_{10}$):** Egy egészséges ember felesleges kezelésének költsége.

Általában **$C_{01} \gg C_{10}$** (egy beteg elnézése sokkal drágább).

A racionális döntéshez a küszöböt úgy kell beállítani, hogy az ne csak a valószínűségeket, hanem a **várható költséget** is minimalizálja. A diák levezetik, hogy a döntési szabály (pozitív vagy negatív jóslatot adjunk) a hibák költségétől is függ.

### Összefoglaló a vizsgára:

*   **Konfúziós Mátrix:** A kiértékelés alapja (TP, TN, FP, FN).
*   **Accuracy:** Félrevezető lehet, óvatosan használd!
*   **Recall vs. Precision:** A legfontosabb mutatópár. A **Recall** a "ne hagyj ki semmit" metrikája, a **Precision** a "ne tévedj, ha tippelsz" metrikája.
*   **ROC / AUC:** A modell általános jóságát méri a különböző küszöbértékek mentén. Minél nagyobb, annál jobb.
*   **Költségmátrix:** A valós életben a különböző hibáknak különböző költsége van, ezt is figyelembe kell venni a modell optimalizálásánál.