Szia! Ez a diasor a **Játékok és Ellenséges Keresés (Adversarial Search)** témakörét dolgozza fel. Ez az egyik legizgalmasabb része a mesterséges intelligenciának, hiszen itt jönnek képbe a sakkprogramok (Deep Blue), a GO (AlphaGo) és az olyan stratégiák, amikor egy gondolkodó ellenfél ellen kell nyernünk.

Itt a részletes, vizsga-fókuszú összefoglaló:

---

### 1. Játékok típusai (4-6. dia)

Először definiálnunk kell, milyen környezetben vagyunk. A játékokat több szempontból osztályozhatjuk:

*   **Determinisztikus vs. Sztochasztikus:**
    *   *Determinisztikus:* Nincs véletlen (pl. Sakk). Ha lépek egyet, biztosan az történik.
    *   *Sztochasztikus:* Van véletlen faktor (pl. kockadobás a Backgammonban).
*   **Információ:**
    *   *Teljes információ:* Látom az egész táblát (Sakk, Go).
    *   *Részleges információ:* Nem látok mindent (Póker, kártyajátékok).
*   **Játékosok száma:** Egy, kettő vagy több.
*   **Zéró összegű (Zero-sum) vs. Általános:**
    *   **Zéró összegű:** Ami nekem jó (+1), az az ellenfélnek rossz (-1). A hasznosságok összege konstans (0). Ez a tiszta versengés (pl. Sakk). **Vizsgán leginkább ezzel foglalkozunk!**
    *   *Nem zéró összegű:* Lehetséges a kooperáció is (pl. Fogolydilemma).

---

### 2. A Minimax Algoritmus (7-18. dia) – **KULCSANYAG**

Ez az alapja minden kétszemélyes, determinisztikus játéknak.

**Alapfelvetés:** Két játékos van:
1.  **MAX (mi vagyunk):** A saját hasznosságunkat akarjuk **maximalizálni**.
2.  **MIN (az ellenfél):** A mi hasznosságunkat akarja **minimalizálni** (vagyis a sajátját növelni, de mivel zéró összegű a játék, ez ugyanaz).

**Működése (Játékfa):**
*   Felépítünk egy fát a lehetséges lépésekből.
*   A levelek (végállapotok) értékét a játékszabályok adják (pl. nyertem=+1, vesztettem=-1).
*   Az értékeket „felbuborékoltatjuk” (backpropagate) a gyökérig:
    *   Ha **MAX** jön: A gyerekek közül a **legnagyobbat** választja.
    *   Ha **MIN** jön: A gyerekek közül a **legkisebbet** választja (feltételezzük, hogy optimálisan játszik és ki akar szúrni velünk).

**Tulajdonságai:**
*   **Optimális:** Igen, ha az ellenfél is optimális.
*   **Hatékonyság:** Ez egy Mélységi Keresés (DFS).
    *   Időigény: $O(b^m)$ (Exponenciális). Ez nagyon rossz! Sakkban kivitelezhetetlen a teljes fát felépíteni.

---

### 3. Alfa-Béta Nyesés (Alpha-Beta Pruning) (19-24. dia) – **VIZSGATÉTEL**

Mivel a Minimax túl lassú, optimalizálni kell. A nyesés lényege: **Ne vizsgáljunk meg olyan ágakat, amikről már biztosan tudjuk, hogy nem fogjuk választani.**

**A két változó:**
*   **$\alpha$ (Alfa):** A **MAX** játékos eddigi legjobb (legmagasabb) garantált lehetősége. (Minimum ennyit elérek).
*   **$\beta$ (Béta):** A **MIN** játékos eddigi legjobb (legalacsonyabb) garantált lehetősége. (Maximum ennyit enged nekem).

**A nyesés szabálya:**
Ha egy csomópontban a lehetséges érték rosszabb, mint amit máshol már találtunk (pl. MIN talál egy olyan lépést, ami nagyon rossz nekünk, rosszabb mint $\alpha$), akkor nem nézi meg a többi lehetőséget azon az ágon, mert MAX úgysem lépne oda.

**Tulajdonságai:**
*   **Ugyanazt az eredményt adja**, mint a sima Minimax, csak gyorsabban!
*   Ideális esetben (jó sorrendezéssel) az effektív elágazási tényező a gyökére csökken: $O(b^{m/2})$.
*   Magyarul: Ugyanannyi idő alatt **kétszer olyan mélyre** tudunk keresni.

---

### 4. Erőforráskorlátok és Heurisztikák (48-51. dia)

A valóságban (Sakk, Go) még az Alfa-Béta nyeséssel sem érünk le a fa aljára (túl mély).
**Megoldás:**
1.  **Mélységi korlát:** Csak pl. 8 lépés mélységig nézünk előre.
2.  **Kiértékelő függvény (Evaluation Function / Heuristic):**
    *   Mivel nem értünk a végére, nem tudjuk, ki nyert. Ezért **megbecsüljük** az állást.
    *   Pl. Sakkban: `Saját vezérek száma - Ellenfél vezérei` + `Pozíció jósága`...
    *   Ez általában egy súlyozott lineáris függvény: $Eval(s) = w_1 f_1(s) + w_2 f_2(s) + \dots$

**Iteratívan mélyülő keresés:** Ha időre játszunk (pl. 2 perc gondolkodási idő), akkor először 1 mélységig nézzük, aztán 2, aztán 3... amikor lejár az idő, a legutolsó befejezett keresés eredményét lépjük meg.

---

### 5. Sztochasztikus Játékok: Expectimax (52-58. dia)

Mi van, ha van véletlen (pl. kockadobás), vagy az ellenfél nem optimális, hanem néha hibázik?

**Expectimax Algoritmus:**
*   A "MIN" csomópontokat lecseréljük **"Véletlen" (Chance)** csomópontokra.
*   Itt nem a minimumot vesszük, hanem **Várható Értéket (Súlyozott átlagot)** számolunk.
    *   Képlet: $\sum (Valószínűség \times Érték)$.
*   **Fontos különbség:** A Minimáxban mindegy volt az értékek skálája (csak a sorrend számított: 100 > 10). Az Expectimaxnál számítanak az arányok (nem mindegy, hogy 100 vagy 1000 a nyeremény, mert átlagolunk)!

---

### 6. Monte Carlo Tree Search (MCTS) (68-71. dia) – **MODERN AI**

Ez a technológia tette lehetővé, hogy a gép megverje az embert GO-ban (AlphaGo). Akkor használjuk, ha a fa túl nagy, és nehéz jó heurisztikát írni.

**Lényege:** Nem a teljes fát járja be, hanem "próbajátékokat" (szimulációkat) futtat véletlenszerűen a végéig, és statisztikát vezet.

**A 4 fázis (Vizsgán kérdezhetik!):**
1.  **Kiválasztás (Selection):** Leereszkedünk a fában egy ígéretes ágon a levelekig.
2.  **Kiterjesztés (Expansion):** Hozzáadunk egy új lépést a fához.
3.  **Szimuláció (Simulation/Play-out):** Innen véletlenszerűen lejátszjuk a partit a végéig.
4.  **Visszaterjesztés (Backpropagation):** Az eredményt (nyert/vesztett) visszavezetjük a fenti csomópontokba (frissítjük a statisztikát).

**Exploration vs. Exploitation (Felfedezés vs. Kiaknázás):**
Az MCTS-nek döntenie kell:
*   Olyan lépést nézzen, ami eddig jónak tűnt (**Exploitation**)?
*   Vagy olyat, amit még keveset vizsgált, hátha jó lesz (**Exploration**)?
*   Erre való az **UCB (Upper Confidence Bound)** képlet (71. dia).

---

### Összefoglaló a vizsgára:

1.  **Minimax:** Tudnod kell, hogyan működik (lentről felfelé max/min számolás).
2.  **Alfa-Béta nyesés:** Értsd meg a logikáját (nem változtat eredményt, csak gyorsít). Tudd, mikor lehet vágni (ha találtam jobbat a testvérágon).
3.  **Heurisztikus kiértékelés:** Kell, ha nem tudunk végigszámolni a fán.
4.  **Expectimax:** Véletlen események kezelése (átlagolás a minimum helyett).
5.  **MCTS:** A modern módszer nagy fákra. (4 lépés + UCB képlet elve).

Ez a diasor lefedi a klasszikus játékelméleti AI alapjaitól a modern AlphaGo technológiáig az anyagot. Ha a Minimax példát papíron le tudod vezetni, nyert ügyed van!