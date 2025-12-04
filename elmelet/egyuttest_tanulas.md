Rendben, nézzük meg az **Együttes Tanulás (Ensemble Learning)** elméletét a diák alapján. Ez a gépi tanulás egyik leghatékonyabb "trükkje", amivel szinte mindig javíthatunk egy modell teljesítményén.

---

### 1. A Nagy Kép: Miért jobb a "tömeg", mint az egyén?

A központi ötlet egyszerű, mint a "Legyen Ön is milliomos!" közönségkérdése. (Slide 3, 4)

*   **A probléma:** Van egy nehéz kérdés. Megkérdezünk egyetlen "szakértőt" (egy gépi tanulási modellt), aki lehet, hogy téved.
*   **A megoldás:** Mi lenne, ha megkérdeznénk **sok "közepesen okos" szakértőt**, és a válaszaikat valahogy **kombinálnánk** (pl. megszavaztatnánk őket)? A "tömeg bölcsessége" elve alapján a közös döntésük valószínűleg jobb és megbízhatóbb lesz, mint bármelyik egyéni szakértőé.

**Definíció:** Az **együttes tanulás (ensemble learning)** az a folyamat, ahol több modellt tanítunk be ugyanarra a problémára, majd a végső döntést a modelljeik predikcióinak **aggregálásával** (pl. átlagolásával vagy szavazásával) hozzuk meg.

*   **Előny:** Általában jobb pontosságot és nagyobb megbízhatóságot ad.
*   **Hátrány:** Több modellt kell tanítani, ami több időbe és számítási kapacitásba kerül.

---

### 2. A Módszerek: Hogyan hozzuk létre a "tömeget"?

A siker kulcsa a **diverzitás**: a modelleknek valamennyire **különbözőeknek** kell lenniük. Ha mindegyik ugyanazt a hibát követi el, a közös döntésük sem lesz jobb. A diák három fő technikát mutatnak be ennek elérésére.

#### A) Bagging (Bootstrap Aggregating)
*   **A "Párhuzamos" módszer** (Slide 5-11)
*   **Filozófia:** "Adjunk minden szakértőnek egy kicsit más tananyagot!"
*   **Működése:**
    1.  **Bootstrapping (Visszahelyezéses mintavételezés):** Az eredeti `N` méretű tanítóhalmazból hozz létre sok új, szintén `N` méretű adathalmazt úgy, hogy **visszahelyezéssel** válogatsz. Ez azt jelenti, hogy egy-egy adatpont többször is szerepelhet egy új halmazban, míg mások kimaradhatnak.
    2.  **Párhuzamos tanítás:** Minden új adathalmazon taníts be egy-egy független modellt. Mivel mindegyik kicsit más adatokon tanult, a "véleményük" is kicsit más lesz.
    3.  **Aggregálás:** Egy új adatpont osztályozásakor minden modell szavaz, és a **többségi döntés** nyer. (Regressziónál a jóslatok átlagát vesszük).
*   **Leghíresebb példa: Random Forest (Véletlen Erdő)**
    *   Ez egy speciális Bagging, ahol az alapmodellek **döntési fák**.
    *   A diverzitást tovább növelik azzal, hogy minden fa építésekor nemcsak a mintákat, hanem a **jellemzőket (attribútumokat)** is véletlenszerűen választják ki. Ezért a fák még kevésbé fognak hasonlítani egymásra.

#### B) Boosting
*   **A "Szekvenciális" módszer** (Slide 12-17)
*   **Filozófia:** "Tanuljunk egymás hibáiból!"
*   **Működése:** A modelleket nem párhuzamosan, hanem **egymás után** tanítjuk.
    1.  Tanítunk egy egyszerű, "gyenge" modellt (pl. egy nagyon sekély döntési fát).
    2.  Megnézzük, melyik tanítópéldákat **rontotta el**.
    3.  A következő modell tanításakor **nagyobb súlyt (fontosságot)** adunk azoknak a példáknak, amiket az előző elrontott. Így a következő modell arra fog koncentrálni, hogy a nehéz eseteket megoldja.
    4.  Ezt ismételgetjük.
    5.  **Aggregálás:** A végső döntésnél a modellek **súlyozottan** szavaznak. Amelyik modell a tanítás során jobban teljesített, annak a szavazata többet ér.
*   **Leghíresebb példa: AdaBoost (Adaptive Boosting)**
    *   Pontosan a fenti logikát valósítja meg a minták súlyozásával.

#### C) Stacking
*   **A "Meta-tanulás" módszer** (Slide 18-20)
*   **Filozófia:** "Legyen egy főnök, aki megtanulja, melyik szakértőben bízhat!"
*   **Működése:**
    1.  **Alapmodellek tanítása:** Tanítunk be több, akár **teljesen különböző típusú** modellt (pl. egy döntési fát, egy neurális hálót és egy logisztikus regressziót).
    2.  **Meta-modell tanítása:** Létrehozunk egy új "meta" adathalmazt, ahol a jellemzők az alapmodellek **jóslatai**. Ezen az új adathalmazon tanítunk be egy "főnök" modellt (meta-tanulót), akinek az a dolga, hogy megtanulja, hogyan kombinálja az alapmodellek jóslatait a legjobb végeredmény érdekében.
*   **Előnye:** Képes kihasználni a különböző modelltípusok erősségeit.

---

### Összehasonlítás a vizsgára:

| Technika | Működési elv | Tanítás módja | Modell diverzitás forrása |
| :--- | :--- | :--- | :--- |
| **Bagging** | "Tömeg bölcsessége" | **Párhuzamos** | Különböző adathalmaz-részletek (bootstrapping) |
| **Boosting** | "Hibákból tanulás" | **Szekvenciális** | Fókusz a nehéz, elrontott példákon |
| **Stacking** | "Tanuljunk meg kombinálni" | **Hierarchikus** | Különböző típusú modellek használata |

**Kulcsgondolat:** Az együttes tanulás célja a **variancia csökkentése** (Bagging) vagy a **torzítás (bias) csökkentése** (Boosting), ami végeredményben egy stabilabb és pontosabb modellt eredményez.