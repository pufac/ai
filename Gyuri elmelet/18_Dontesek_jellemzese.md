Rendben, tartjuk a részletes, példákkal gazdagított stílust. Ez a diasorozat a **Döntések Értékeléséről** szól, de már egy másik szemszögből: nemcsak a hasznosságot nézzük, hanem a **hibázás költségét** is.

Ez a téma a **statisztikai döntéselmélet** alapja, és nagyon fontos az orvosi diagnosztikában, a csalásdetektálásban vagy akár a spam szűrésben.

Itt a vizsga-fókuszú összefoglaló:

---

### 1. Bináris Döntések (Jó/Rossz) Értékelése (5-7. dia) – **ALAPFOGALMAK**

Képzelj el egy orvosi tesztet (vagy egy COVID-tesztet). Két állapot van: a valóság (beteg/egészséges) és a teszt eredménye (pozitív/negatív). Ez 4 lehetséges kombinációt ad, amit a **Konfúziós Mátrixban (Confusion Matrix)** foglalunk össze.

**A 4 eset (EZT TUDD KÍVÜLRŐL!):**
1.  **Valós Pozitív (TP - True Positive):** Beteg, és a teszt is azt mondja. (Helyes találat).
2.  **Valós Negatív (TN - True Negative):** Egészséges, és a teszt is azt mondja. (Helyes elutasítás).
3.  **Hamis Pozitív (FP - False Positive):** Egészséges, de a teszt azt mondja, beteg. ("Vaklárma", **I. típusú hiba**).
4.  **Hamis Negatív (FN - False Negative):** Beteg, de a teszt azt mondja, egészséges. ("Elnézett eset", **II. típusú hiba**).

*   *Példa:* Spam szűrő.
    *   TP: Spam volt, és kiszűrtük. (Jó)
    *   TN: Nem spam (fontos levél), és megkaptuk. (Jó)
    *   FP: Fontos levél (pl. állásajánlat), de a gép spamnek hitte és kidobta. (NAGY BAJ!)
    *   FN: Spam levél, de a gép átengedte. (Kicsi baj, csak törölni kell).

---

### 2. Jósági Mutatók (Metrikák) (8-14. dia)

Hogyan mérjük egy modell teljesítményét? Nem elég a "pontosság" (Accuracy), mert csalóka lehet! (Lásd lejjebb).

**A legfontosabb mutatók:**
1.  **TPR (True Positive Rate) / Érzékenység (Sensitivity) / Recall:**
    *   Képlet: $TP / (TP + FN)$.
    *   Jelentés: A *valódi betegek* hány százalékát vettük észre? (Mennyire érzékeny a teszt).
2.  **TNR (True Negative Rate) / Specificitás (Specificity):**
    *   Képlet: $TN / (TN + FP)$.
    *   Jelentés: Az *egészségesek* hány százalékát mondtuk helyesen egészségesnek?
3.  **PPV (Positive Predictive Value) / Precizitás (Precision):**
    *   Képlet: $TP / (TP + FP)$.
    *   Jelentés: Ha a teszt pozitív lett, mennyi az esélye, hogy *tényleg* beteg vagyok? (Mennyire bízhatunk a riasztásban).
4.  **Pontosság (Accuracy - ACC):**
    *   Képlet: $(TP + TN) / \text{Összes}$.
    *   Jelentés: Az esetek hány százalékában döntöttünk jól?

**A Pontosság csapdája (Paradoxon):**
Ha egy betegség nagyon ritka (pl. 1000 emberből 1 beteg), és én egy olyan "buta" modellt írok, ami **MINDENKIRE** azt mondja, hogy egészséges:
*   Találat: 999 helyes, 1 téves.
*   Accuracy: 99.9%. Hű, de jó!
*   Valójában: Használhatatlan, mert a beteget (a lényeget) nem találta meg. (Recall = 0%).
Ezért kell a többi mutatót is nézni!

---

### 3. ROC Görbe és AUC (17-21. dia) – **VIZSGATÉTEL**

A legtöbb modell nem csak "Igen/Nem" választ ad, hanem egy valószínűséget vagy pontszámot (pl. 38.5 °C). Nekünk kell meghúzni a határt (**Küszöbérték**), hogy honnantól számít pozitívnak.

*   **A dilemma:**
    *   Ha alacsonyra teszem a küszöböt (mindenkit gyanúsnak vélek): Sok beteget megtalálok (nagy TPR), de sok lesz a téves riasztás (nagy FPR).
    *   Ha magasra teszem a küszöböt (csak a biztosra megyek): Kevés a téves riasztás (kis FPR), de sok beteget elszalasztok (kis TPR).
*   **ROC Görbe (Receiver Operating Characteristic):**
    *   Ez a görbe ábrázolja ezt a kompromisszumot.
    *   X tengely: Hamis Pozitív Arány (FPR).
    *   Y tengely: Valós Pozitív Arány (TPR).
    *   Minden pont a görbén egy-egy lehetséges küszöbértéket jelöl.
*   **AUC (Area Under Curve):** A görbe alatti terület.
    *   Ez egyetlen szám (0 és 1 között), ami megmondja, milyen jó a modell.
    *   **0.5:** Véletlen tippelés (átló).
    *   **1.0:** Tökéletes modell.
    *   Minél nagyobb, annál jobb.

---

### 4. Döntés Költségekkel (Cost-Sensitive Learning) (23-30. dia) – **MATEKOS RÉSZ**

A valóságban a hibák nem egyformák.
*   **Spam szűrő:** Fontos levelet kidobni (FP) sokkal drágább hiba ($C_{10}$), mint átengedni egy reklámot (FN, $C_{01}$).
*   **Rákdiagnózis:** Hazaküldeni egy beteget (FN) sokkal súlyosabb hiba ($C_{01}$), mint feleslegesen visszahívni egy egészségeset (FP, $C_{10}$).

**A döntési szabály (Költségminimalizálás):**
Akkor mondjuk valamire, hogy Pozitív, ha a Pozitív döntés **várható költsége** kisebb, mint a Negatív döntésé.

A levezetés vége (29. dia) egy nagyon fontos egyenlőtlenséget ad:
$$ \frac{P(m_k \mid V_1)}{P(m_k \mid V_0)} > \frac{(C_{10} - C_{00}) \cdot P(V_0)}{(C_{01} - C_{11}) \cdot P(V_1)} $$

**Mit jelent ez magyarul? (Példa):**
*   Bal oldalon van a **Likelihood Arány**: Mennyivel valószínűbb ez a tünet ($m_k$) egy betegnél, mint egy egészségesnél? (Ez az orvosi tény).
*   Jobb oldalon van a **Költségek és a Priorok aránya**:
    *   $P(V_0)/P(V_1)$: Mennyire ritka a betegség? (Ha ritka, a jobb oldal nagy szám lesz -> nehezebb pozitívnak dönteni).
    *   $C_{10}/C_{01}$: Mennyire drága a téves riasztás a mulasztáshoz képest?
*   **Konklúzió:** Csak akkor mondjuk ki a betegséget, ha a tünetek **elég erősen** utalnak rá ahhoz, hogy ellensúlyozzák a betegség ritkaságát és a téves riasztás költségét.

---

### Összefoglaló a vizsgára (Ezeket tanuld meg!):

1.  **Konfúziós Mátrix:** Tudd fejből a 4 mezőt (TP, TN, FP, FN) és tudd példával azonosítani.
2.  **Mutatók:** Értsd a különbséget a **Recall** (megtaláltuk-e az összeset?) és a **Precision** (amit találtunk, az tényleg az-e?) között.
3.  **Accuracy paradoxon:** Miért nem jó az Accuracy ritka eseményeknél?
4.  **ROC és AUC:** Mire való? A küszöbérték állítgatására és modellek összehasonlítására. (A nagyobb terület a jobb).
5.  **Költségmátrix:** Tudd, hogy a hibáknak súlya van. Ha $C_{FN}$ (mulasztás) nagyon nagy, akkor lejjebb visszük a küszöböt (többet riasztunk), hogy biztosan ne nézzük el a bajt.

Ez a diasor a statisztikai kiértékelés alapja. Ha érted a "Hamis Pozitív" és "Hamis Negatív" közti különbséget, és hogy mikor melyik a fájdalmasabb, akkor érted a lényeget!