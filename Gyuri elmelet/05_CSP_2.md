Szia! Ez a diasor a **Kényszerkielégítési Problémák (CSP) második része**. Míg az első rész az alapfogalmakat (változók, tartományok, kényszerek) tisztázta, ez az előadás arról szól, **hogyan oldjuk meg ezeket hatékonyan**.

A „sima” visszalépéses keresés (backtracking) önmagában nagyon lassú lehet. Itt tanuljuk meg azokat a trükköket (heurisztikákat és algoritmusokat), amikkel a gyakorlatban is megoldhatóvá válnak a nehéz problémák.

Itt a vizsga-fókuszú összefoglaló:

---

### 1. Az Alap: Visszalépéses Keresés (Backtracking) (3-8. dia)

Ez a CSP megoldások alapalgoritmusa.
*   **Működése:** Mélységi keresés (DFS), de:
    1.  Egyszerre csak egy változót köt le.
    2.  Azonnal ellenőrzi a kényszereket (ha rossz, visszalép).
*   **Kommutativitás:** A CSP-knél a változók sorrendje mindegy a végeredmény szempontjából (mindegy, hogy először `WA=piros` és aztán `NT=zöld`, vagy fordítva). Ezt kihasználva a keresési fa sokkal kisebb lesz.

**Hogyan tehetjük gyorsabbá?** Három fő területen avatkozhatunk be (10. dia – **EZT TUDD FEJBŐL**):
1.  **Szűrés (Filtering):** Előre kizárjuk a rossz értékeket.
2.  **Sorrendezés (Ordering):** Melyik változót/értéket válasszuk?
3.  **Struktúra:** A probléma gráfjának alakját használjuk ki.

---

### 2. Gyorsítás I: Szűrés (Filtering)

A cél: korán észrevenni, ha zsákutcában vagyunk, hogy ne kelljen feleslegesen keresgélni.

#### A) Előretekintő ellenőrzés (Forward Checking) (11-15. dia)
*   **Mikor fut?** Amikor hozzárendelünk egy értéket az éppen vizsgált $X$ változóhoz.
*   **Mit csinál?** Megnézi $X$ összes *még lekötetlen* szomszédját ($Y$), és törli $Y$ tartományából azokat az értékeket, amik ütköznének $X$ választott értékével.
*   **Korlátja:** Csak a *közvetlen* következményeket látja. Nem veszi észre, ha a jövőbeli változók egymással kerülnek konfliktusba (lásd 15. dia példája: NT és SA egymás szomszédai, de a Forward Checking nem veszi észre, hogy ha mindkettőnek csak a kék marad, az baj).

#### B) Élkonzisztencia (Arc Consistency - AC-3) (16-20. dia) – **NAGYON FONTOS**
Ez egy erősebb szűrés.
*   **Definíció:** Egy $X \to Y$ él konzisztens, ha $X$ tartományának *minden* értékéhez létezik $Y$ tartományában egy megfelelő pár.
*   **AC-3 Algoritmus:** Ez terjeszti a konzisztenciát. Ha $X$-ből törlünk egy értéket (mert nem volt párja $Y$-ban), akkor újra kell vizsgálni $X$ többi szomszédját is, hátha most náluk is baj van. Ez a **propagáció**.
*   **Előnye:** Sokkal hamarabb észreveszi a bajt, mint az előretekintő ellenőrzés. Futhat előfeldolgozásként vagy minden lépés után (MAC - Maintaining Arc Consistency).

---

### 3. Gyorsítás II: Sorrendezés (Ordering) (21-24. dia)

Ha a szűrés után még mindig több lehetőségünk van, hogyan döntsünk? Itt **heurisztikákat** használunk.

#### A) Melyik VÁLTOZÓT válasszuk? (Variable Ordering)
A cél a **"Fail First"** (Bukjunk el minél hamarabb) elv. Ha egy ág rossz, derüljön ki most, ne 3 óra múlva.

1.  **MRV (Minimum Remaining Values) – Legkevesebb fennmaradó érték:**
    *   Azt a változót választjuk, aminek a *legkevesebb* megengedett értéke maradt. (A legszigorúbban korlátozott változó).
    *   *Miért?* Mert itt van a legnagyobb esély a hibára, essünk túl rajta.
2.  **Fokszám heurisztika (Degree Heuristic):**
    *   Ez a "tie-breaker" (döntetlen esetén használjuk) az MRV mellé.
    *   Azt a változót választjuk, ami a legtöbb *másik, még lekötetlen* változóval áll kapcsolatban.
    *   *Miért?* Mert ennek a lekötése korlátozza a legjobban a jövőbeli lehetőségeket (gyorsítja a szűrést).

#### B) Melyik ÉRTÉKET válasszuk? (Value Ordering)
Itt a cél a **"Succeed First"** (Sikerüljön minél hamarabb). Mivel csak *egy* megoldást keresünk, próbáljuk megtalálni a jót.

1.  **LCV (Least Constraining Value) – Legkevésbé korlátozó érték:**
    *   Azt az értéket választjuk, ami a szomszédok számára a *legkevesebb* lehetőséget zárja ki.
    *   *Miért?* Hagyjunk minél több mozgásteret a többieknek.

**⚠️ Vizsga tipp:** Ne keverd össze!
*   **Változó** választásnál: A legkritikusabbat (szűk keresztmetszetet) vesszük előre (**Pesszimista** megközelítés).
*   **Érték** választásnál: A legmegengedőbbet vesszük előre (**Optimista** megközelítés).

---

### 4. Gyorsítás III: Struktúra (26-27. dia)

A gráf alakja sokat számít.
*   **Fa struktúra:** Ha a kényszergráf fa (nincs benne hurok/kör), akkor a probléma **lineáris időben** megoldható! (Sokkal gyorsabb, mint az exponenciális).
    *   *Módszer:* Topológiai rendezés + Élkonzisztencia.
*   **Általános gráf (körökkel):** Ez nehéz.
    *   *Trükk:* **Vágóhalmaz (Cutset) kondicionálás**. Kiveszünk néhány változót (vágóhalmaz), adunk nekik fix értéket. Ha ezeket kivéve a maradék gráf fa lesz, akkor azt már gyorsan megoldjuk.

---

### 5. Alternatíva: Lokális Keresés (Local Search) (28-31. dia)

Ha nem a pontos, szisztematikus keresés (backtracking) kell, hanem csak *valami* megoldás gyorsan (pl. 1 millió királynő probléma).

*   **Módszer (Iteratív javítás):**
    1.  Kezdőállapot: Minden változónak adunk egy véletlen értéket (sok kényszer sérül).
    2.  Kiválasztunk egy változót, ami konfliktusban van.
    3.  Átállítjuk az értékét úgy, hogy csökkenjen a hibák száma.
*   **Min-konfliktus heurisztika:** Válasszuk azt az értéket, ami a legkevesebb kényszert sérti meg.
*   **Tulajdonságai:**
    *   Meglepően hatékony (pl. N-királynőre szinte azonnal talál megoldást).
    *   Nem teljes (beragadhat lokális optimumba), de újraindítással javítható.

---

### Összefoglaló puska a vizsgára:

1.  **Backtracking:** A CSP alapértelmezett megoldása (DFS + 1 változó/szint).
2.  **Szűrés:**
    *   **Forward Checking:** Szomszédok ellenőrzése.
    *   **Arc Consistency (AC-3):** Kényszerek terjesztése az egész hálón (erősebb).
3.  **Heurisztikák (Keverhető fogalmak!):**
    *   *Változóhoz:* **MRV** (legkevesebb maradék) + **Fokszám** (legtöbb szomszéd). -> Cél: Hiba megtalálása gyorsan.
    *   *Értékhez:* **LCV** (legkevésbé zavaró). -> Cél: Megoldás megtalálása.
4.  **Struktúra:** A Fa-gráfú CSP könnyű ($O(n)$), a hurkos nehéz.
5.  **Lokális keresés:** Min-konfliktus módszer (nagy problémákra jó, de nem garantált).

Ez a diasor a CSP-k "lelkét" tartalmazza, a vizsgán szinte biztosan lesz kérdés az MRV/LCV különbségéről vagy az AC-3 működéséről.