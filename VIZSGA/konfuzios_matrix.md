![Alt text](/img/konf__m.png)


Persze, tegyük ezt teljesen tisztába! A konfúziós mátrix (tévesztési mátrix) a legfontosabb eszköz, hogy megértsd, mit csinál egy modell, mert nem csak azt mondja meg, hogy "mennyire okos", hanem azt is, hogy **milyen fajta hibákat vét.**

Felejtsük el a matekot egy percre. Képzeld el, hogy **orvos vagy (ez az Osztályozó)**, és bejönnek hozzád páciensek. Két dolgot tehetsz:
1.  Azt mondod: **"Ön Beteg!"** (Ez a **Pozitív** döntés).
2.  Azt mondod: **"Ön Egészséges, hazamehet."** (Ez a **Negatív** döntés).

A valóságban a beteg vagy tényleg beteg, vagy nem.

---

### 1. A Kódfejtés: Mit jelentenek a betűk?

A rövidítések (TP, TN, FP, FN) mindig **két betűből** állnak.

**A MÁSODIK BETŰ (P vagy N): Mit mondtál TE?**
Ez csak a te döntésedet jelöli.
*   **P (Positive):** Azt mondtad, hogy **VAN** valami (pl. beteg).
*   **N (Negative):** Azt mondtad, hogy **NINCS** semmi (pl. egészséges).

**AZ ELSŐ BETŰ (T vagy F): Igazad volt?**
Ez a valósággal való összehasonlítás.
*   **T (True / Igaz):** Eltaláltad! A valóság megegyezik a döntéseddel.
*   **F (False / Hamis):** Tévedtél! A valóság az ellenkezője annak, amit mondtál.

---

### 2. A négy eset részletesen

1.  **TP (True Positive - Valós Pozitív): "A Helyes Találat"**
    *   *Döntésed:* Beteg (P).
    *   *Valóság:* Tényleg beteg.
    *   *Eredmény:* **Igazad volt (T).**

2.  **TN (True Negative - Valós Negatív): "A Helyes Elutasítás"**
    *   *Döntésed:* Egészséges (N).
    *   *Valóság:* Tényleg egészséges.
    *   *Eredmény:* **Igazad volt (T).**

3.  **FP (False Positive - Hamis Pozitív): "A Vaklárma"**
    *   *Döntésed:* Beteg (P).
    *   *Valóság:* Kutya baja (Negatív).
    *   *Eredmény:* **Tévedtél (F).**
    *   *Miért baj?* Feleslegesen ijesztetted meg, felesleges gyógyszert kap.

4.  **FN (False Negative - Hamis Negatív): "Az Elnézett Eset"**
    *   *Döntésed:* Egészséges, menjen haza (N).
    *   *Valóság:* Valójában beteg (Pozitív).
    *   *Eredmény:* **Tévedtél (F).**
    *   *Miért baj?* Hazaküldtél egy beteget, aki lehet, hogy meghal. Általában ez a veszélyesebb hiba!

---

### 3. Hogyan kell beírni a táblázatba? (A Térkép)

Ez a legtrükkösebb rész, mert a vizsgán néha megcserélik a sorokat és az oszlopokat. **Mindig nézd meg a fejlécet!**

A te feladatlapodon (4. feladat) így néz ki a szerkezet:
*   **Sorok (Vízszintes):** Az **Osztályozó** döntése (Mit mondott a gép?).
*   **Oszlopok (Függőleges):** A **Valós** helyzet (Mi az igazság?).

Rajzoljuk le a te táblázatodat a kódokkal:

| | **Valós: P** (Tényleg beteg) | **Valós: N** (Tényleg egészséges) |
| :--- | :---: | :---: |
| **Osztályozó: P** (Azt mondta: beteg) | **TP** <br> *(Azt mondta beteg, és tényleg az)* | **FP** <br> *(Azt mondta beteg, de nem az)* |
| **Osztályozó: N** (Azt mondta: nem beteg) | **FN** <br> *(Azt mondta nem beteg, de az)* | **TN** <br> *(Azt mondta nem beteg, és tényleg nem)* |

---

### 4. Hogyan töltöttük ki a feladatot ez alapján?

Nézzük újra a feladat szövegét ezzel a tudással:

1.  *"...algoritmus összesen 3622 mintára mondta azt, hogy negatív..."*
    *   Ez azt jelenti, hogy az **Alsó sor (Osztályozó N) összege** 3622.
    *   Tehát: $FN + TN = 3622$.

2.  *"...és ebből 3111 volt ténylegesen negatív."*
    *   Mit jelent ez? A gép Negatívat mondott, ÉS a valóság is Negatív volt.
    *   Ez a **TN**.
    *   Tehát beírtuk a jobb alsó sarokba: **3111**.

3.  **Számoljuk ki a hiányzó FN-t (Bal alsó):**
    *   Ha az egész alsó sor 3622, és a jobb fele 3111, akkor a bal fele (FN):
    *   $3622 - 3111 = 511$. (Ez az FN).

4.  **Most nézzük az oszlopokat (A valóságot):**
    *   A szöveg eleje: *"Ebből 3576 volt negatív (nem releváns) minta..."*
    *   Ez a **Jobb oldali oszlop (Valós N) összege**. ($FP + TN = 3576$).
    *   Mivel a TN-t már tudjuk (3111), kiszámolhatjuk az **FP**-t (jobb felső):
    *   $3576 - 3111 = 465$. (Ez az FP).

5.  **A másik oszlop:**
    *   A szöveg: *"...és 1424 minta mutatott érdekes jellemzőket (pozitív)."*
    *   Ez a **Bal oldali oszlop (Valós P) összege**. ($TP + FN = 1424$).
    *   Mivel az FN-t már kiszámoltuk az előbb (511), kiszámolhatjuk a **TP**-t (bal felső):
    *   $1424 - 511 = 913$. (Ez a TP).

Így jött ki a táblázat!
*   **TP (913)** | **FP (465)**
*   **FN (511)** | **TN (3111)**

Érthetőbb így a logika? A lényeg: **Sor = Mit mondott a gép? Oszlop = Mi a valóság? A metszéspont a kód.**

Természetesen! Most, hogy érted a mátrixot (hogy mi micsoda), ezek a mutatók már csak **egyszerű osztások** lesznek.

Mindegyik mutató egy-egy kérdésre ad választ arról, hogyan teljesített az "orvosunk" (az algoritmus).

Használjuk a korábban kiszámolt számainkat:
*   **TP (Találat):** 913
*   **FN (Elnézett):** 511
*   **FP (Vaklárma):** 465
*   **TN (Helyes elutasítás):** 3111

---

### b) Recall (Valódi pozitív arány / Érzékenység)

**A kérdés:** *"A ténylegesen betegek (pozitívak) hány százalékát sikerült megtalálnunk?"*

Ez a legfontosabb, ha nem akarunk beteget hazaküldeni. Itt a mátrix **Bal Oszlopát** nézzük (a Valós Pozitívokat).

*   **Képlet:** $\frac{\text{Akiket megtaláltunk (TP)}}{\text{Az összes Valós Pozitív (TP + FN)}}$
*   **Számolás:**
    *   Megtaláltunk: 913
    *   Összes beteg volt: $913 + 511 = 1424$
    *   Recall = $913 / 1424 = \mathbf{0.641}$ (vagy 64.1%)

**Mit jelent ez?** Az algoritmus a "betegek" kb. 64%-át vette észre, a többieket (36%-ot) sajnos hazaküldte (FN). Ez egy közepes eredmény.

---

### c) Precision (Pontosság)

**A kérdés:** *"Amikor azt mondtuk valakire, hogy beteg, mennyire volt igazunk?"*

Ez a riasztás megbízhatósága. Itt a mátrix **Felső Sorát** nézzük (azokat, akiket az algoritmus Pozitívnak tippelt).

*   **Képlet:** $\frac{\text{Akik tényleg betegek voltak (TP)}}{\text{Akikre azt mondtuk, hogy betegek (TP + FP)}}$
*   **Számolás:**
    *   Tényleg beteg: 913
    *   Összes riasztásunk: $913 + 465 = 1378$
    *   Precision = $913 / 1378 = \mathbf{0.663}$ (vagy 66.3%)

**Mit jelent ez?** Ha az algoritmus riaszt, 66% esély van rá, hogy tényleg baj van. De 34%-ban csak vaklárma (FP).

---

### d) FPR (False Positive Rate / Hamis pozitív arány)

**A kérdés:** *"Az egészséges emberek (negatívok) hány százalékát ijesztettük meg feleslegesen?"*

Ez a "Vaklárma-mutató". Azt akarjuk, hogy ez minél kisebb legyen (közel a 0-hoz). Itt a mátrix **Jobb Oszlopát** nézzük (a Valós Negatívokat).

*   **Képlet:** $\frac{\text{Akit tévesen riasztottunk (FP)}}{\text{Az összes Valós Negatív (TN + FP)}}$
*   **Számolás:**
    *   Téves riasztás: 465
    *   Összes egészséges ember: $3111 + 465 = 3576$
    *   FPR = $465 / 3576 = \mathbf{0.130}$ (vagy 13.0%)

**Mit jelent ez?** Az egészséges emberek 13%-át gyanúsította meg az algoritmus tévesen.

---

### Összefoglaló Puska a vizsgára:

1.  **Recall:** A bal oldali **oszlop** aránya. (Felső / Egész oszlop). *Cél: minél nagyobb.*
2.  **FPR:** A jobb oldali **oszlop** aránya. (Felső / Egész oszlop). *Cél: minél kisebb.*
3.  **Precision:** A felső **sor** aránya. (Bal / Egész sor). *Cél: minél nagyobb.*

Tökéletes, ezzel fel is tesszük a pontot az i-re! Ez a feladat köti össze a számolást a grafikonnal.

Itt a megoldás a képen lévő **e)** kérdésre:

---

### 1. MEGOLDÁS

**A pont jele:** **B**

**Miért? (Indoklás):**
Mert a ROC görbe az **FPR** (X-tengely) és a **TPR** (Y-tengely) értékeit ábrázolja. Az előző feladatokban kiszámoltuk, hogy **FPR = 0.13** és **TPR (Recall) = 0.64**. Ha ezeket a koordinátákat megkeressük a grafikonon (X=0.13, Y=0.64), azok pontosan a **B** pontra esnek.

---

### 2. LEVEZETÉS LÉPÉSRŐL LÉPÉSRE

1.  **Adatok összegyűjtése:**
    *   A ROC görbe definíciója szerint a vízszintes tengely az **FPR** (Hamis pozitív arány), a függőleges tengely pedig a **TPR** (Valódi pozitív arány, azaz a Recall).
    *   A **b)** feladatból tudjuk: **TPR = 0.64** (vagy 0.641).
    *   A **d)** feladatból tudjuk: **FPR = 0.13** (vagy 0.130).

2.  **Koordináták keresése a grafikonon:**
    *   Nézzük az X-tengelyt (FPR): A 0 és a 0.2 között vagyunk, kicsivel a közepe (0.1) után. Ez a **0.13**.
        *   Az **A** pont túl balra van (kb. 0.05).
        *   A **C** pont túl jobbra van (kb. 0.22).
        *   A **B** pont vízszintesen pont jónak tűnik.
    *   Nézzük az Y-tengelyt (TPR): A 0.6 és 0.7 között vagyunk, majdnem középen. Ez a **0.64**.
        *   Az **A** pont túl alacsonyan van (kb. 0.42).
        *   A **C** pont túl magasan van (kb. 0.8).
        *   A **B** pont függőlegesen is pont a 0.6 és 0.7 közé esik.

**Következtetés:** A kiszámolt $(0.13, 0.64)$ koordinátapár egyértelműen a **B** pontnak felel meg.

---

### 3. ELMÉLETI HÁTTÉR (ROC Görbe)

Hogy teljesen értsd a vizsgára:

*   **Mi az a ROC görbe?** (Receiver Operating Characteristic)
    Ez a görbe azt mutatja meg, hogyan teljesít az osztályozó modellünk, ha **állítgatjuk az "érzékenységét"** (a küszöbértéket).

*   **A Tengelyek:**
    *   **X (FPR - False Positive Rate):** A "Vaklárma" aránya. (Minél jobbra megyünk, annál több egészségest ijesztgetünk feleslegesen).
    *   **Y (TPR - True Positive Rate / Recall):** A "Találati" arány. (Minél feljebb megyünk, annál több beteget találunk meg).

*   **A Pontok jelentése:**
    *   Minden pont (A, B, C) a modell egy-egy lehetséges beállítását (munkapontját) jelöli.
    *   **A pont:** Nagyon szigorú a modell. Alig ad riasztást. Kevés a vaklárma (FPR kicsi), de sok beteget sem vesz észre (TPR is kicsi).
    *   **C pont:** Nagyon laza a modell. Szinte mindenkire ráijeszt. Megtalálja a betegek 80%-át (TPR magas), de cserébe rengeteg a vaklárma (FPR is magasabb).
    *   **B pont:** Ez a kettő közötti egyensúlyi állapot, amit a b-d feladatokban kiszámoltunk.

*   **Melyik a legjobb pont?**
    Általában a bal felső sarokhoz (0, 1) legközelebbi pontot keressük, vagy azt, ami a feladat költségei szerint (mennyire drága egy téves riasztás vs. egy elnézett beteg) a legkedvezőbb.

Ezzel a **teljes feladatsoron** végigmentünk! Gratulálok a kitartásodhoz, szerintem készen állsz a vizsgára. Van esetleg még bármi, ami nem tiszta?