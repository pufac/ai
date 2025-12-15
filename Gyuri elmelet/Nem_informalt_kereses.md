Szia! Ez a diasor az **„Informatikai Intelligencia”** (és általában a mérnöki problémamegoldás) egyik legfontosabb alapkövét tárgyalja: a **Keresést (Search)**.

Ez a téma szinte biztosan nagy súllyal fog szerepelni a vizsgán, különösen az algoritmusok összehasonlítása és tulajdonságaik (idő- és tárigény).

Itt van a strukturált, vizsga-fókuszú magyarázat:

---

### 1. Ágensek Típusai (5-10. dia)

Az előző anyagban beszéltünk az ágensekről. Itt két típust különböztetünk meg a tervezés szempontjából:

1.  **Reflex Ágens:** Csak a *jelenlegi* érzékelés alapján dönt (pl. ha meleg van -> hűtés be). Nem gondol a jövőre. **Ez nem intelligens tervezés.**
2.  **Tervkészítő (Planning) Ágens:**
    *   Felteszi a kérdést: *"Mi lenne, ha...?"*
    *   Van egy **belső modellje** a világról.
    *   Tudja a **célt**.
    *   Szimulálja a lépéseket, mielőtt cselekedne. **Erről szól ez a diasor.**

---

### 2. A Keresési Probléma Definíciója (11-20. dia)

Ahhoz, hogy a gép tervezni tudjon, definiálnunk kell a problémát 4 elemmel:

1.  **Állapottér (State Space):** Minden lehetséges helyzet, amiben a világ lehet (pl. sakkban az összes lehetséges bábuállás, térképen a városok).
2.  **Állapotátmenet-függvény (Successor Function / Actions):** Mit csinálhatok? Milyen állapotból milyen állapotba jutok? (pl. Arad -> Zerind). Ehhez tartozik a **Költség** (pl. távolság km-ben).
3.  **Kezdőállapot (Start State):** Hol vagyok most?
4.  **Célteszt (Goal Test):** Megérkeztem-e? (pl. `if location == "Bukarest"`).

**Fontos:** A valós világ túl bonyolult, ezért **absztrakciót** használunk (csak a lényeget modellezzük). Lásd a *Palacsinta problémát* (63. dia): nem a palacsinta íze számít, hanem a sorrendje.

---

### 3. Állapottér-gráf vs. Keresési Fa (25-32. dia)

Ez egy elméleti megkülönböztetés, amit gyakran kérdeznek:

*   **Állapottér-gráf:** A probléma *matematikai* leírása. Statikus. Minden állapot csak egyszer szerepel benne. (Pl. Románia térképe).
*   **Keresési Fa:** Amit az algoritmus *felépít* a fejében (memóriájában) keresés közben.
    *   A kezdőállapot a gyökér.
    *   Az ágak a döntések.
    *   **Fontos:** Itt egy állapot többször is szerepelhet! (Pl. ha Aradról elmegyek Nagyszebenbe, majd vissza Aradra, a fában Arad kétszer lesz: egyszer gyökérként, egyszer gyermekként).
    *   Ezért a fa végtelen nagy lehet akkor is, ha a világ véges (körbe-körbe járás).

---

### 4. Keresési Algoritmusok Értékelése (23. és 42. dia) – **VIZSGATÉTEL!**

Hogyan döntjük el, melyik algoritmus a jó? 4 szempont alapján:

1.  **Teljesség (Completeness):** Ha van megoldás, garantáltan megtalálja?
2.  **Optimalitás (Optimality):** A legjobb (legkisebb költségű) megoldást találja meg?
3.  **Időigény:** Mennyi ideig tart?
4.  **Tárigény:** Mennyi memóriát eszik?

**Jelölések a képletekhez:**
*   $b$: **Elágazási tényező** (branching factor) – átlagosan hány felé mehetek egy csomópontból.
*   $d$: A legsekélyebb **megoldás mélysége** (depth).
*   $m$: A keresési fa **maximális mélysége** (lehet végtelen is).

---

### 5. Nem Informált (Vak) Keresések

Ezek az algoritmusok **nem tudják**, milyen messze van a cél, csak azt látják, mi a következő lépés. ("Bekötött szemmel tapogatózunk").

#### A) Szélességi Keresés (BFS - Breadth-First Search) (44-46. dia)
*   **Működés:** Szintről szintre halad. Először megnézi az összes szomszédot (1 lépés távolság), aztán azok szomszédjait (2 lépés), stb.
*   **Adatszerkezet:** **FIFO Sor** (First In, First Out).
*   **Tulajdonságok:**
    *   *Teljes:* Igen (ha véges a megoldás mélysége).
    *   *Optimális:* Igen (de csak ha minden lépés költsége 1, vagy egyforma).
    *   *Idő:* $O(b^d)$ (Exponenciális - ez sok).
    *   *Tár:* $O(b^d)$ (**Ez a nagy baj!** Minden csomópontot meg kell jegyezni az aktuális szinten. Exponenciális memóriaigény).

#### B) Mélységi Keresés (DFS - Depth-First Search) (39-43. dia)
*   **Működés:** Elindul egy úton, és addig megy, amíg tud (a fa aljáig), aztán visszalép (backtrack).
*   **Adatszerkezet:** **LIFO Verem** (Stack).
*   **Tulajdonságok:**
    *   *Teljes:* **NEM**. (Végtelen ágba vagy körbe futhat).
    *   *Optimális:* **NEM**. (Találhat egy rossz megoldást mélyen, miközben volt egy jobb is sekélyebben).
    *   *Idő:* $O(b^m)$ (Rosszabb lehet, mint a BFS, ha $m > d$).
    *   *Tár:* $O(b \cdot m)$ (**Ez az előnye!** Lineáris a memóriaigény, mert csak az aktuális utat kell tárolni).

#### C) Mélységkorlátozott Keresés (49. dia)
*   DFS, de adunk neki egy limitet ($l$). Pl. "Ne menj mélyebbre 10 lépésnél".
*   Megoldja a végtelen ciklus problémáját, de ha a megoldás a limiten túl van, nem találja meg (nem teljes).

#### D) Iteratívan Mélyülő Keresés (IDS) (50-53. dia) – **NAGYON FONTOS!**
*   **Trükk:** Egyesíti a BFS és DFS előnyeit.
*   **Működés:** Futtat egy DFS-t $limit=0$-val. Ha nincs meg, újraindítja $limit=1$-gyel. Aztán $limit=2$-vel...
*   **Miért jó ez?**
    *   *Teljes* és *Optimális* (mint a BFS).
    *   *Kis memóriaigényű* (mint a DFS - $O(bd)$).
*   **Nem pazarlás mindig újraindítani?** Nem! A fa csomópontjainak nagy része az alján van. Az, hogy a felső szinteket többször generáljuk le, elhanyagolható költség a legalsó szint méretéhez képest. (Lásd 53. dia számítása: csak kb +20% munka).
*   **Vizsgán:** Ez a preferált nem informált keresés, ha a memória korlátos!

#### E) Egyenletes Költségű Keresés (UCS - Uniform Cost Search) (56-59. dia)
*   **Működés:** Mindig a legkisebb *eddigi* útköltségű ($g(n)$) csomópontot fejti ki. Ez gyakorlatilag a **Dijkstra-algoritmus**.
*   **Adatszerkezet:** **Prioritásos sor**.
*   **Tulajdonságok:**
    *   *Teljes:* Igen.
    *   *Optimális:* **IGEN**, ez garantáltan a legolcsóbb utat találja meg, nem csak a legkevesebb lépést.
    *   *Hátrány:* Minden irányba tapogatózik, ha kicsik a költségek.

#### F) Kétirányú Keresés (Bidirectional Search) (68. dia)
*   **Működés:** Egyszerre indítunk keresést a Startból és a Célból. Amikor összeérnek, megvan az út.
*   **Előny:** $b^d$ helyett $2 \cdot b^{d/2}$. Ez hatalmas különbség (pl. $10^{6}$ helyett $2 \cdot 10^3$).
*   **Nehézség:** A célból visszafelé lépni sokszor nehéz (nem mindig reverzibilisek az operátorok), és a memóriában tartani a találkozási pontot nehéz.

---

### 6. Összefoglaló Táblázat (70. dia) – Ezt tanuld meg kívülről!

A vizsga egyik leggyakoribb feladata ezen algoritmusok összehasonlítása.

| Jellemző | BFS (Szélességi) | UCS (Egyenletes k.) | DFS (Mélységi) | IDS (Iteratív) | Kétirányú |
| :--- | :--- | :--- | :--- | :--- | :--- |
| **Teljes?** | Igen | Igen | Nem | Igen | Igen |
| **Optimális?** | Igen (ha lépésköltség=1) | Igen | Nem | Igen | Igen |
| **Időigény** | $O(b^d)$ (Nagy) | $O(b^{C^*/\epsilon})$ | $O(b^m)$ | $O(b^d)$ | $O(b^{d/2})$ (Kicsi) |
| **Tárigény** | $O(b^d)$ (Nagy - kritikus!) | $O(b^{C^*/\epsilon})$ | **$O(bm)$ (Kicsi - jó!)** | **$O(bd)$ (Kicsi - jó!)** | $O(b^{d/2})$ |

**Magyarázat a táblázathoz:**
*   A **DFS** legnagyobb baja, hogy nem teljes (végtelen ciklus) és nem optimális. Előnye a kis memória.
*   A **BFS** legnagyobb baja a memóriaigény (elfogy a RAM).
*   Az **IDS** (Iteratív mélyülő) a "győztes" a vak keresések között: optimális, teljes, és kevés memóriát eszik.

### Összegzés a vizsgára:
1.  Tudd a **4 értékelési szempontot** (Teljesség, Optimalitás, Idő, Tár).
2.  Értsd meg a **Keresési Fa** generálását (Frontier/Perem fogalma).
3.  Tudd, miért jobb az **IDS** (Iteratív mélyülő), mint a sima BFS vagy DFS.
4.  Tudd, hogy az **UCS** (Egyenletes költségű) találja meg a legolcsóbb utat, ha a lépésköltségek nem egyformák.

Ha ez megvan, stabil alapod van a következő (informált keresések) témához!