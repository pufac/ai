Rendben, koncentráljunk a **Megerősítéses Tanulás (Reinforcement Learning - RL)** elméleti hátterére, ahogy a diák bemutatják. Ez a terület arról szól, hogyan tanulhat meg egy ágens (pl. robot, program) cselekedni a világban próbálkozások és visszajelzések (jutalmak/büntetések) alapján, anélkül, hogy előre megmondanánk neki a pontos szabályokat.

---

### 1. A Nagy Kép: Mi a különbség az MDP és az RL között?

Ez a legfontosabb megkülönböztetés, amit a diák az elején tisztáz. (Slide 3, 4)

*   **Offline Megoldás (MDP):**
    *   Itt mindent **előre tudunk**. Ismerjük a világ teljes térképét: a lehetséges állapotokat (S), a cselekvések hatását (T - átmeneti modell) és a jutalmakat (R).
    *   A feladatunk "offline", egy íróasztal mellett megoldható: a **tervezés**. Az Értékiteráció és Eljárásmód-iteráció ilyen tervezési algoritmusok.
    *   *Analógia:* A robot leül, megnézi a labirintus tervrajzát, és kigondolja a legjobb stratégiát, mielőtt egyetlen lépést is tenne.

*   **Online Tanulás (RL - Megerősítéses Tanulás):**
    *   Itt **nem tudunk semmit** a világról. Nincs térképünk. Nem ismerjük a **T**-t (hogy egy lépés hova visz) és az **R**-t (hogy miért mennyi pont jár).
    *   A feladatunk "online", a világban való **cselekvés közben** történik: a **tanulás**.
    *   *Analógia:* A robotot bedobják egy sötét labirintusba. Neki kell kitapasztalnia, hogy a falak merre vannak (T), és hol vannak a csapdák és kincsek (R).

---

### 2. A két fő RL megközelítés

Hogyan tanul egy robot a sötétben? A diák két alapvető filozófiát mutatnak be. (Slide 5-9)

#### A) Modellalapú Tanulás (Model-Based RL)
*   **Filozófia:** "Próbáljunk meg térképet rajzolni!"
*   **Működése:**
    1.  **Modelltanulás:** A robot mászkál a világban, és statisztikát vezet. Megszámolja, hogy ha az 'A' mezőn "fel"-t lépett, hányszor került a 'B' mezőre és hányszor a 'C'-re. Ebből **megbecsüli a T-t és az R-t**.
    2.  **Tervezés:** Miután van egy (valószínűleg pontatlan) térképe, úgy tesz, mintha az lenne a valóság. Lefuttatja rajta a már ismert MDP algoritmusokat (pl. Értékiterációt), és kitalálja a legjobb stratégiát a **becsült modellre**.
*   **Előnye:** "Adathatékony". Kevés tapasztalatból is tud általánosítani, mert egy modellt épít.
*   **Hátránya:** Ha a modell rossz, a stratégia is rossz lesz. A modellépítés maga is bonyolult lehet.

#### B) Modellmentes Tanulás (Model-Free RL)
*   **Filozófia:** "Ne pazaroljuk az időt térképrajzolásra! Inkább csak azt jegyezzük meg, melyik cselekvés volt jó."
*   **Működése:** A robot nem próbálja megérteni a világ *fizikáját* (a T és R szabályokat). Helyette közvetlenül azt tanulja meg, hogy egy adott helyen egy adott cselekvés mennyire "jó".
*   Két fő típusa van:
    1.  **Passzív RL:** A robotnak van egy **fix stratégiája** (pl. "mindig menj előre"), amit követ. A célja csak annyi, hogy megtanulja, mennyire jók az állapotok (`U(s)`) ezen fix stratégia mentén. (Pl. TD-tanulás).
    2.  **Aktív RL:** A robotnak **nincs stratégiája**, neki kell kitalálnia. Itt már nemcsak az állapotok értékét (`U(s)`), hanem a cselekvés-állapot párok értékét (`Q(s,a)`) tanulja. Ez a **Q-tanulás**.

---

### 3. A Q-tanulás: A Modellmentes Tanulás Királya

A Q-tanulás a legfontosabb algoritmus ebben a témakörben. (Slide 21-25)

*   **A Cél:** Megtanulni a **Q-értékeket**. $Q(s,a)$ azt jelenti: "Mennyi a várható összes jövőbeli jutalmam, ha az 's' állapotban az 'a' cselekvést választom, és utána optimálisan játszom?"
*   **Miért jó ez?** Ha ismerjük a Q-értékeket, a döntés triviális: minden állapotban azt a cselekvést választjuk, amelyikhez a legmagasabb Q-érték tartozik! Nem kell modellt ismernünk.
*   **A Tanulási Szabály:** Minden egyes élmény (`s, a, r, s'`) után frissítjük a `Q(s,a)` értékét egy "mozgóátlag" képlettel:
    $$ Q(s,a) \leftarrow (1-\alpha) \cdot Q(s,a) + \alpha \cdot (\text{új információ}) $$
    Az "új információ" a kapott jutalom ($r$) és a legjobb jövőbeli lehetőség leszámítolt értéke ($\gamma \max Q(s',a')$).
*   **Off-Policy:** A Q-tanulás zsenialitása, hogy akkor is megtanulja az optimális Q-értékeket, ha közben **teljesen buta, véletlenszerű lépéseket teszünk**. Ezért hívják "eljárásmódon kívüli" (off-policy) tanulásnak.

---

### 4. A Dilemma: Felfedezés vs. Kizsákmányolás (Exploration vs. Exploitation)

Ez az aktív RL legfontosabb elméleti problémája. (Slide 26-29)

*   **Kizsákmányolás (Exploitation):** Ha már tudom, hogy a kedvenc éttermem jó, mindig odamegyek. Ez a "mohó" stratégia: mindig azt teszem, amiről **jelenleg** azt hiszem, hogy a legjobb.
*   **Felfedezés (Exploration):** Kipróbálok egy új éttermet. Lehet, hogy rossz lesz (rövid távú veszteség), de lehet, hogy sokkal jobb, mint a régi (hosszú távú nyereség).

Egy jó RL ágensnek egyensúlyoznia kell a kettő között. A diák két módszert említ:

1.  **$\epsilon$-mohó ($\epsilon$-greedy):**
    *   Nagy valószínűséggel (1-$\epsilon$) a legjobb ismert lépést választjuk (kizsákmányolás).
    *   Kis valószínűséggel ($\epsilon$) egy teljesen véletlen lépést választunk (felfedezés).
2.  **Felfedezési Függvények:**
    *   Olyan cselekvéseket részesítünk előnyben, amiket még kevésszer próbáltunk ki. Egy "optimista" becslést adunk az ismeretlenre, pl. $f(u, n) = u + k/n$, ahol `n` a látogatások száma. Minél kisebb `n`, annál nagyobb a bónusz, amit a Q-értékhez adunk.

---

### Összefoglaló a vizsgára:

*   **MDP vs. RL:** Az MDP-nél **ismerjük** a világ szabályait (T, R) és tervezünk. Az RL-nél **nem ismerjük** őket és tanulunk.
*   **Modellalapú vs. Modellmentes:** A modellalapú megpróbálja **megtanulni a szabályokat (T, R)**, a modellmentes **közvetlenül az értékeket (U vagy Q)** tanulja.
*   **Q-tanulás:** A legfontosabb modellmentes algoritmus. **Q(s,a) értékeket** tanul, és **off-policy** (akkor is működik, ha rossz lépéseket teszünk).
*   **Felfedezés vs. Kizsákmányolás:** Az RL ágensnek egyensúlyoznia kell aközött, hogy a már bevált dolgokat csinálja, vagy új dolgokat próbál ki a jobb jutalom reményében.