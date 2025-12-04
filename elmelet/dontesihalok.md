Abszolút! Ezek a diák a **racionális döntéshozatalról**, a **hasznosságokról** és a **döntési hálókról** szólnak. Ez egy logikus lépés a Bayes-hálók után: nemcsak modellezzük a világot, hanem **cselekszünk** is benne.

---

### 1. A Nagy Kép: Mi a probléma, amit megoldunk?

Eddig a Bayes-hálókkal azt kérdeztük: *"Mi az igazság?"*. Most azt kérdezzük: ***"Mit tegyek?"***

Egy racionális ágensnek (robotnak, szoftvernek) úgy kell döntenie, hogy a lehető legjobb eredményt érje el egy bizonytalan világban.

*   **Példa (slide 3):** Az időjárás-előrejelzés (`Forecast`) bizonytalanul befolyásolja a valós időjárást (`Weather`). A döntésem, hogy viszek-e esernyőt (`Umbrella`), befolyásolja, hogy mennyire leszek boldog (`U` - Utility/hasznosság). Vizesen boldogtalan vagyok, de feleslegesen cipelni az esernyőt is rossz. **Mi a racionális döntés?**

---

### 2. A Fő Elv: MEU (Maximum Expected Utility)

A diákok központi fogalma a **MEU (Maximum Várható Hasznosság)** elve. (Slide 4)

*   **Jelentése:** "Válaszd azt a cselekvést, amely a várható hasznosságot maximalizálja."

Ez nem azt jelenti, hogy a legjobb lehetséges kimenetelt választjuk! Azt jelenti, hogy minden lehetséges kimenetelt **súlyozunk** annak valószínűségével, és azt a cselekvést választjuk, ahol ez a súlyozott átlag a legnagyobb.

**Példa a lottóra:**
*   A legjobb kimenetel a főnyeremény.
*   De ennek valószínűsége elképesztően alacsony.
*   A várható hasznossága (nyeremény * esély) valószínűleg negatív (mert a szelvény többe kerül). A MEU elv szerint nem racionális lottózni.

---

### 3. Az Eszköz: Döntési hálók (Decision Networks)

A döntési háló egy **kibővített Bayes-háló**, ami segít kiszámolni a MEU-t. (Slide 4, 5)

Két új csomóponttípust vezet be:

1.  **Döntési csomópont (Téglalap):**
    *   **Jele:** `Umbrella` (Esernyő)
    *   **Jelentése:** Ezt **mi irányítjuk**. Ide írjuk be a lehetséges cselekedeteinket (pl. "leave" vagy "take").
    *   **Szabály:** Nincsenek szülei, mert a mi döntésünk nem a véletlen műve, hanem mi választunk.

2.  **Hasznosság csomópont (Gyémánt):**
    *   **Jele:** `U`
    *   **Jelentése:** Ez egy **szám**, ami megmondja, mennyire vagyunk boldogok egy adott kimenetel esetén.
    *   **Szabály:** A szülei azok a változók, amiktől a boldogságunk függ.
        *   Az esernyős példában `U` szülei: `Umbrella` (a döntésünk) és `Weather` (a véletlen). Mert az, hogy boldog vagyok-e, attól függ, hogy vittem-e esernyőt ÉS hogy milyen az idő.

---

### 4. A Számítás Menete: Hogyan döntünk? (Slide 6, 7, 10)

A döntési háló kiértékelése mindig ugyanazt a receptet követi:

1.  **Rögzítsd a bizonyítékot (Evidencia):** Ha tudunk valamit a világról (pl. az előrejelzés "rosszat" mondott - `Forecast=bad`), akkor ezt rögzítjük. (Slide 9, 10)

2.  **Próbáld ki az összes döntést:** Minden lehetséges cselekvésre (döntési csomópont beállítására) külön-külön számolj!
    *   **Eset A:** Tegyük fel, hogy **otthon hagyom** az esernyőt (`Umbrella = leave`).
    *   **Eset B:** Tegyük fel, hogy **magammal viszem** az esernyőt (`Umbrella = take`).

3.  **Számold ki a várható hasznosságot (EU - Expected Utility) minden döntésre:**
    *   A képlet:  $EU(\text{döntés}) = \sum_{\text{kimenetelek}} P(\text{kimenetel} | \text{döntés}) \cdot U(\text{kimenetel})$
    *   **Eset A (leave):**
        *   $EU(\text{leave}) = P(\text{sun}) \cdot U(\text{leave, sun}) + P(\text{rain}) \cdot U(\text{leave, rain})$
        *   A példában (slide 7): $0.7 \cdot 100 + 0.3 \cdot 0 = 70$.
    *   **Eset B (take):**
        *   $EU(\text{take}) = P(\text{sun}) \cdot U(\text{take, sun}) + P(\text{rain}) \cdot U(\text{take, rain})$
        *   A példában (slide 7): $0.7 \cdot 20 + 0.3 \cdot 70 = 35$.

4.  **Válaszd a Maximumot (MEU):**
    *   Hasonlítsd össze a kiszámolt EU értékeket: $EU(\text{leave}) = 70$, $EU(\text{take}) = 35$.
    *   Melyik a nagyobb? A 70.
    *   **Optimális döntés:** `leave` (hagyd otthon).
    *   A **maximális várható hasznosság (MEU)** értéke: 70.

---

### Összefoglaló puska a vizsgára:

*   **Racionalitás alapelve:** **MEU** (Maximum Expected Utility) - Azt a cselekvést válaszd, aminek a legmagasabb a **várható** (valószínűségekkel súlyozott) hasznossága.
*   **Döntési háló:** Egy Bayes-háló, ami két új csomóponttal van kiegészítve:
    *   **Téglalap (Döntés):** Amit mi irányítunk.
    *   **Gyémánt (Hasznosság):** A "boldogságmérő", a végső pontszám.
*   **A számítás menete:**
    1.  Tegyél fel egy döntést (pl. "viszem az esernyőt").
    2.  Számold ki a lehetséges kimenetelek (napos, esős) valószínűségét.
    3.  Szorozd össze a valószínűségeket a hozzájuk tartozó hasznosságokkal (pontszámokkal) és add össze őket. Ez a **várható hasznosság (EU)**.
    4.  Ismételd meg ezt az összes lehetséges döntésedre.
    5.  Amelyik döntésnél a legmagasabb EU jött ki, az a **racionális választás**.