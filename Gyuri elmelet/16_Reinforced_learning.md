Szia! Ez a diasor a **Megerősítéses Tanulásról (Reinforcement Learning - RL)** szól. Ez a gépi tanulás és az AI egyik legdinamikusabban fejlődő területe (ezzel tanult meg járni a Boston Dynamics robotkutyája, vagy verni meg az embert a Go játékban az AlphaGo).

Itt a lényeg az, hogy **nincs tanár**, aki megmondja a helyes választ. Csak "pofonok" (büntetés) és "cukorkák" (jutalom) vannak, amikből az ágensnek magának kell rájönnie, hogyan viselkedjen.

Lássuk részletesen, példákkal!

---

### 1. Az Alaphelyzet: Offline vs. Online (3-4. dia)

Emlékezz vissza az előző anyagra (MDP – Markov Döntési Folyamat). Ott tudtuk a játékszabályokat:
*   Tudtuk, hova vezet egy lépés ($T$: átmenet).
*   Tudtuk, hol van a jutalom ($R$).
*   Ez az **Offline tervezés**: Ülök a szobában a térkép felett, és kitalálom az útvonalat.

A **Megerősítéses Tanulásnál (RL)** viszont:
*   **Nem ismerjük a világot ($T$ ismeretlen):** Nem tudjuk, mi történik, ha megnyomjuk a piros gombot.
*   **Nem ismerjük a jutalmakat ($R$ ismeretlen):** Nem tudjuk, hol van a kincs, amíg bele nem botlunk.
*   Ez az **Online tanulás**: Be vagyunk dobva egy idegen bolygóra bekötött szemmel. **Cselekednünk kell**, és a tapasztalatokból (megégetem a kezem / találok egy almát) kell megtanulni a stratégiát.

---

### 2. A Két Fő Megközelítés (5-9. dia)

Hogyan álljunk neki a tanulásnak az ismeretlenben?

#### A) Modellalapú Tanulás (Model-Based)
*   **Logika:** Próbáljunk meg térképet rajzolni a fejünkben!
*   **Módszer:**
    1.  Megyünk körbe-körbe, és felírjuk a tapasztalatokat. ("Ha A-ból jobbra léptem, B-be jutottam 10-ből 8-szor" -> ezzel becsüljük a $T$-t. "És kaptam érte -1 pontot" -> ezzel becsüljük az $R$-t).
    2.  Ha már van egy (közelítő) modellünk a világról, használhatjuk a régi módszereket (pl. Értékiterációt) a megoldásra.
*   **Előny:** Hatékonyan használja az adatokat.
*   **Hátrány:** Bonyolult világban nehéz jó modellt építeni.

#### B) Modellmentes Tanulás (Model-Free) – **EZ A FONTOSABB!**
*   **Logika:** Nem érdekel, hogyan működik a fizika, csak azt akarom tudni, melyik állapot mennyire jó.
*   **Módszer:** Közvetlenül a hasznosságokat ($U$ vagy $Q$ értékeket) próbáljuk megtanulni a tapasztalatokból, anélkül, hogy felépítenénk a $T$ és $R$ mátrixokat.
*   **Analógia:** Ahhoz, hogy megtanulj biciklizni, nem kell ismerned a fizika törvényeit (modell), csak érezned kell, hogy ha dőlsz, merre kormányozz (modellmentes).

---

### 3. Passzív Tanulás: Mennyire jó a stratégiám? (10-18. dia)

Először tegyük fel, hogy a robot nem dönthet, csak megy egy előre megírt program (fix $\pi$ eljárásmód) szerint. A feladatunk csak annyi, hogy megmondjuk, mennyire jó ez a stratégia (kiszámoljuk az állapotok $V(s)$ értékét).

**1. Közvetlen Kiértékelés (Direct Evaluation):**
*   Lejátszunk sok epizódot.
*   Minden állapotnál felírjuk, mennyi volt a *tényleges* végső jutalomösszeg onnantól kezdve.
*   A végén átlagolunk.
*   *Baj:* Nem veszi észre az összefüggéseket. Ha „A” állapot mindig „B”-be vezet, akkor „A” értékének hasonlónak kéne lennie „B”-hez. A közvetlen kiértékelés ezt nem tanulja meg, mindenkit külön kezel. Lassú.

**2. Időbeli Különbség Tanulás (Temporal Difference - TD Learning) – KULCSFOGALOM!**
*   Ez a modern RL alapja.
*   **Ötlet:** Nem kell megvárni a játék végét, hogy tanuljunk!
*   Ha eljutottam $s$-ből $s'$-be, és kaptam érte $r$ jutalmat, akkor $s$ értéke legyen kb. annyi, mint a jutalom + $s'$ értéke.
*   **Mozgóátlag (17. dia):** Nem írjuk felül a régi tudásunkat teljesen, csak kicsit módosítjuk az új tapasztalat alapján.
    *   $$V(s) \leftarrow (1-\alpha)V(s) + \alpha(r + \gamma V(s'))$$
    *   $\alpha$ (alfa): Tanulási ráta. Mennyire hiszünk az új információnak a régivel szemben.

---

### 4. Aktív Tanulás: Q-Learning (20-25. dia) – **A LEGFONTOSABB ALGORITMUS**

Itt már a robot döntéseket is hoz, és az **optimális stratégiát** keresi.

**Mi a gond a sima $V(s)$ értékekkel?**
Ahhoz, hogy $V(s)$-ből döntést hozzunk, tudnunk kéne a hatásokat ($T$), de azt nem tudjuk (modellmentesek vagyunk).

**Megoldás: Q-értékek ($Q(s, a)$)**
*   Nem azt tanuljuk meg, hogy "milyen jó itt lenni" ($V(s)$), hanem azt, hogy "**milyen jó itt lenni ÉS ezt cselekedni**" ($Q(s, a)$).
*   Ha ismerjük a Q-értékeket, a döntés triviális: abban az állapotban azt a cselekvést választom, aminek a legnagyobb a Q-értéke! Nem kell tudni, hova vezet, csak azt, hogy az a lépés "sokat ér".

**A Q-Learning Képlete (22. dia - TUDD FEJBŐL!):**
$$Q(s, a) \leftarrow (1-\alpha)Q(s, a) + \alpha \left[ R(s, a, s') + \gamma \max_{a'} Q(s', a') \right]$$
*   Magyarázat: A régi tudásunkat ($Q(s,a)$) frissítjük azzal, amit most tapasztaltunk:
    *   Megkaptam a jutalmat ($R$).
    *   Megérkeztem $s'$-be, ahonnan a *legjobb* folytatás értéke $\max Q(s', a')$.
*   Ez egy ún. **Off-policy** tanulás: Akkor is megtanulja az *optimális* stratégiát, ha éppen össze-vissza (szuboptimálisan) cselekszik a tanulás közben.

---

### 5. Felfedezés vs. Kizsákmányolás (Exploration vs. Exploitation) (26-30. dia)

Ez a tanuló ágens legnagyobb dilemmája.
*   **Kizsákmányolás (Exploitation):** Azt a lépést választom, amiről *jelenleg* úgy tudom, hogy a legjobb. (Biztosra megyek).
*   **Felfedezés (Exploration):** Kipróbálok valami újat, hátha találok egy még jobb utat (vagy beleesem a szakadékba).

**Megoldások:**
1.  **$\epsilon$-mohó ($\epsilon$-greedy) stratégia:**
    *   Dobjunk egy érmét.
    *   Kis eséllyel ($\epsilon$): Lépjünk teljesen véletlenszerűen (Felfedezés).
    *   Nagy eséllyel ($1-\epsilon$): Lépjük a legjobbat (Kizsákmányolás).
2.  **Felfedezési függvények (28. dia):**
    *   Legyünk optimisták! Ha egy állapotban még keveset jártunk, feltételezzük róla, hogy szuper jó. Ez odavonzza az ágenst. Ha kiderül, hogy mégse jó, akkor legközelebb már nem megy oda.

---

### 6. Közelítő Q-tanulás (Approximate Q-Learning) (32-36. dia) – **PACMAN PÉLDA**

A valóságban (pl. Pacman, önvezető autó) túl sok állapot van. Nem lehet minden egyes (x, y, szellem_helye, pöttyök_helye...) kombinációhoz tárolni egy számot egy táblázatban. Soha nem érnénk a végére.

**Megoldás: Jellemzők (Features) használata.**
*   Nem az állapotot magát jegyezzük meg, hanem a tulajdonságait.
*   Pl. Pacman-ben nem azt tanuljuk meg, hogy "az (5,3) koordináta jó", hanem olyan szabályokat tanulunk, mint:
    *   $f_1$: "Ha közel a szellem, az ROSSZ".
    *   $f_2$: "Ha közel a kaja, az JÓ".
*   **Lineáris függvény:**
    $$Q(s, a) = w_1 \cdot f_1(s, a) + w_2 \cdot f_2(s, a) + \dots$$
*   **Tanulás:** Nem a táblázatot töltjük ki, hanem a **súlyokat ($w$)** tanuljuk meg. (Pl. megtanulja, hogy a $w_1$ (szellem közelsége) nagyon nagy negatív szám legyen).
*   **Előny:** Ez lehetővé teszi az **általánosítást**. Ha a robot megtanulja, hogy a tűz forró, akkor egy *új, sosem látott* szobában is kerülni fogja a tüzet, mert a "tűz közelsége" feature ott is aktív.

---

### 7. Eljárásmód-keresés (Policy Search) (37-39. dia)

Ez egy alternatív megközelítés.
*   A Q-learning a Q-értékeket (hasznosságot) akarja pontosan megtanulni.
*   De nekünk nem a pontos szám kell, hanem csak a döntés!
*   A Policy Search közvetlenül a paramétereket (súlyokat) hangolja úgy, hogy a végső jutalom maximális legyen, anélkül, hogy a pontos Q-értékekkel törődne. (Pl. hegymászó algoritmussal állítgatja a súlyokat).

---

### 8. Alkalmazások (40-42. dia)

*   **Ipari hűtés (Google):** Az RL ágens megtanulta úgy kapcsolgatni a hűtést az adatközpontokban, hogy 40%-kal csökkentette a számlát.
*   **Robotika:** Tárgyak megfogása. A robot szimulációban tanul (mert a valóságban lassú és drága összetörni), majd a tudást átviszik az igazi robotra.
*   **Tőzsde:** Részvények adás-vétele az optimális profit érdekében.

---

### Összefoglaló a vizsgára (Ezeket tanuld meg!):

1.  **RL vs. MDP:** RL-ben nem ismerjük a modellt ($T, R$), tapasztalatból (próba-szerencse) tanulunk.
2.  **TD Tanulás:** Mozgóátlaggal frissítjük a tudásunkat minden lépés után.
3.  **Q-Learning:**
    *   Tudni kell a képletét és a logikáját.
    *   Tudni kell, hogy ez **modellmentes** (nem kell $T, R$) és **off-policy** (akkor is megtanulja az optimumot, ha bénázunk közben).
4.  **Exploration vs. Exploitation:**
    *   $\epsilon$-greedy módszer lényege.
5.  **Közelítő Q-tanulás (Features):**
    *   Miért kell? Mert a táblázat túl nagy lenne.
    *   Hogyan működik? Súlyokat ($w$) tanulunk jellemzőkhöz, nem állapotokat. Ez adja az általánosítást.

Ha a **Q-learning frissítési képletét** és a **jellemző alapú általánosítást** (Pacman példa) megérted, akkor a legnehezebb részek megvannak!