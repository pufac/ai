![alt text](/img/megeros_tanulas.png)


Szuper, megérkeztek a képek! Ez egy klasszikus **Értékiteráció (Value Iteration)** feladat Markov-döntési folyamatokban (MDP).

Itt vannak a megoldások, a levezetés és a magyarázat.

---

### 1. MEGOLDÁSOK

**a.)**
*   **a1 cselekvés várható hasznossága:** **6.4**
*   **a2 cselekvés várható hasznossága:** **1.0**

**b.)**
*   **Optimális cselekvés:** **a1**
*   **Miért:** Mert az a1 cselekvéshez tartozó várható jövőbeli érték (6.4) magasabb, mint az a2-é (1.0).

**c.)**
*   **Új hasznosság ($U_{k+1}(s2)$):** **3.4**

---

### 2. LEVEZETÉS LÉPÉSRŐL LÉPÉSRE

**Adatok:**
*   Aktuális állapot: **s2**
*   Jutalom minden állapotban: $R(s) = 0.2$
*   Leszámítolási tényező: $\gamma = 0.5$
*   Régi hasznosságok ($U_k$): $s1=-1, s2=-5, s3=5, s4=10$

**a.) A várható hasznosságok kiszámítása ($\sum T \cdot U$)**

Itt azt számoljuk ki, hogy "átlagosan" milyen értékű mezőre lépünk az adott cselekvéssel. A képlet a szumma része: $\sum_{s'} T(s, a, s') U_k(s')$

*   **a1 cselekvés vizsgálata (Bal oldali gráf, s2-ből induló nyilak):**
    *   $s2 \to s1$ (0.1 eséllyel): $0.1 \cdot U(s1) = 0.1 \cdot (-1) = -0.1$
    *   $s2 \to s3$ (0.5 eséllyel): $0.5 \cdot U(s3) = 0.5 \cdot 5 = 2.5$
    *   $s2 \to s4$ (0.4 eséllyel): $0.4 \cdot U(s4) = 0.4 \cdot 10 = 4.0$
    *   **Összeg (a1):** $-0.1 + 2.5 + 4.0 = \mathbf{6.4}$

*   **a2 cselekvés vizsgálata (Jobb oldali gráf, s2-ből induló nyilak):**
    *   $s2 \to s1$ (0.5 eséllyel): $0.5 \cdot U(s1) = 0.5 \cdot (-1) = -0.5$
    *   $s2 \to s2$ (0.1 eséllyel): $0.1 \cdot U(s2) = 0.1 \cdot (-5) = -0.5$
    *   $s2 \to s3$ (0.4 eséllyel): $0.4 \cdot U(s3) = 0.4 \cdot 5 = 2.0$
    *   **Összeg (a2):** $-0.5 - 0.5 + 2.0 = \mathbf{1.0}$

**b.) Optimális cselekvés kiválasztása**

A racionális ágens mindig a **maximumot** választja a lehetőségek közül.
*   $a1$ értéke: 6.4
*   $a2$ értéke: 1.0
*   Mivel $6.4 > 1.0$, a választás: **a1**.

**c.) Az új hasznosság kiszámítása**

Behelyettesítünk a teljes Bellman-egyenletbe:
$U_{k+1}(s2) = R(s2) + \gamma \cdot \max(a1\_érték, a2\_érték)$

*   $R(s2) = 0.2$
*   $\gamma = 0.5$
*   $\max(6.4, 1.0) = 6.4$

$U_{k+1}(s2) = 0.2 + 0.5 \cdot 6.4$
$U_{k+1}(s2) = 0.2 + 3.2 = \mathbf{3.4}$

---

### 3. ELMÉLETI HÁTTÉR ÉS ÉRTELMEZÉS

Ez a feladat az **MDF (Markov Döntési Folyamat)** megoldását mutatja be **Értékiterációval**.

**Mi a lényeg?**
Egy robot (vagy ágens) mászkál egy 4 szobás ($s1-s4$) világban. Nem tudja biztosan, hova lép (ezért vannak a százalékok a nyilakon). A célja, hogy megtalálja, mennyire "értékes" az $s2$ szoba.

Egy szoba értéke két dologból áll össze:
1.  **Azonnali jutalom ($R$):** Amit akkor kap, amikor belép oda (itt ez mindenhol 0.2).
2.  **Jövőbeli ígéret:** Hova tud innen továbbmenni? Ha innen könnyen eljuthat a "Kincseskamrába" ($s4$, ami 10 pontot ér), akkor ez a szoba is értékes.

**A képlet (Bellman-egyenlet) magyarázata:**
$$ U_{k+1}(s) \leftarrow R(s) + \gamma \cdot \max_a \sum_{s'} T(s, a, s') U_k(s') $$

*   **$R(s)$:** "A pénz, amit most a kezembe kapok."
*   **$\gamma$ (gamma):** "Mennyire vagyok türelmetlen?" (0.5 azt jelenti, a holnapi pénz feleannyit ér, mint a mai).
*   **$\max_a$:** "A legjobb döntést hozom." (Megnézem, merre érdemesebb indulni: $a1$ vagy $a2$ felé?).
*   **$\sum T(\dots)U(\dots)$:** "Várható érték". Mivel a lépés bizonytalan (csúszós a padló), átlagoljuk a lehetséges érkezési helyek értékét a valószínűségükkel.

**Mit csináltunk a feladatban?**
Lényegében egyetlen iterációs lépést (frissítést) hajtottunk végre az $s2$ állapotra. Megnéztük, mi történne, ha az $a1$-et választanánk, mi, ha az $a2$-t, kiválasztottuk a jobbat, és ebből kiszámoltuk az $s2$ új, pontosított értékét.

# egyéb szarok
## 2 másik feladat tipus igazabol, de elvileg a felső nagyfeladatban mind3 "benne van"

Igen, tökéletesen emlékszel! Nagyon fontos különbséget vettél észre.

Azon a papíron, amit korábban mutattál (ahol a ZH kérdések voltak), szerepelt az **Eljárásmód-iteráció (Policy Iteration)** két lépése is. Ezekben nincs (vagy máshogy van) a `max`.

Nézzük meg, mi a különbség a feladatban és a képletben!

---

### 1. Amikor NINCS benne `max`: Eljárásmód Kiértékelése (Policy Evaluation)

Ez a képlet így néz ki:
$$ U_{k+1}(s) \leftarrow R(s) + \gamma \cdot \sum_{s'} T(s, \pi(s), s') U_k(s') $$

**Miben más a feladat?**
*   **Értékiterációnál (amit most oldottunk meg):** Azt kérdezzük: *"Mi a **legjobb**, amit tehetek?"* Ezért kell a `max`, hogy kiválasszuk a győztes cselekvést (a1 vagy a2).
*   **Kiértékelésnél:** Azt kérdezzük: *"Mennyit ér ez a szoba, ha **kötelezően** ezt a térképet ($\pi$) követem?"*
    *   Itt **nincs választás**. A robotnak meg van parancsolva: "Ha s2-ben vagy, lépd az a1-et!"
    *   Ezért nem kell a `max`. Egyszerűen behelyettesítjük az előírt cselekvés (pl. a1) számait.

**Példa feladat szövege erre:**
*"Számítsa ki az s2 állapot hasznosságát, feltéve, hogy a robot rögzített stratégiája az, hogy s2-ben mindig az **a1** cselekvést választja!"*

---

### 2. Amikor `argmax` van benne: Eljárásmód Javítása (Policy Improvement)

Ez a képlet így néz ki:
$$ \pi_{i+1}(s) \leftarrow \underset{a}{\text{argmax}} \sum_{s'} T(s, a, s') U(s') $$

**Miben más a feladat?**
*   Itt nem számot (hasznosságot) keresünk, hanem **irányt (cselekvést)**.
*   A kérdés: *"Oké, kiszámoltam a pontokat. Melyik a nyerő lépés?"*
*   Az `argmax` azt jelenti: "Ne az értéket add vissza (pl. 6.4), hanem a cselekvés nevét (pl. **a1**), ami ezt az értéket adta."

**Példa feladat szövege erre:**
*"Adja meg az új, javított eljárásmódot ($\pi$) az s2 állapotban a kiszámolt hasznosságok alapján!"* (Ez lényegében a b.) feladatod volt az előző példában).

---

### Összefoglaló táblázat (Hogy lásd a különbséget)

| Módszer | Képlet jellemzője | Mit kérdez a feladat? | Van választás? |
| :--- | :--- | :--- | :--- |
| **Értékiteráció** (Ezt csináltuk most) | Van `max` | "Mennyit ér az állapot, ha **okosan** játszom?" | Igen (a legjobbat). |
| **Eljárásmód Kiértékelés** | **Nincs** `max` | "Mennyit ér az állapot, ha a **parancsot** követem?" | Nincs (fix lépés). |
| **Eljárásmód Javítás** | `argmax` van | "Melyik **lépés** a legjobb?" (Nem a pontszám kell, hanem a betűjele). | Igen. |