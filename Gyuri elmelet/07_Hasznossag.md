Szia! Ez az anyag a **Döntéselmélet (Decision Theory)** alapjait fekteti le. Eddig a pontig az algoritmusaink (pl. A*, keresés) leginkább *célokat* próbáltak elérni (sikerült/nem sikerült).

A valóságban azonban a dolgok nem fekete-fehérek. Vannak bizonytalan kimenetelek (valószínűség) és vannak preferenciák (mennyire örülünk az eredménynek).

Ez az előadás a **Hasznosság (Utility)** fogalmát és a **Racionális Döntéshozatalt** járja körül.

Íme a vizsga-fókuszú összefoglaló:

---

### 1. Miért kell Hasznosság (Utility)? (3-6. dia)

A sima célállapot (goal state) nem elég, mert:
1.  Vannak bizonytalan kimenetelek (szerencse, kockázat).
2.  Nem minden siker egyforma (pl. nyerni 1 lépésben jobb, mint 100 lépésben).
3.  Választanunk kell a "biztos kevés" és a "bizonytalan sok" között.

**Hasznosságfüggvény ($U(s)$):** Egy függvény, ami a világ egy állapotához hozzárendel egy valós számot, ami azt fejezi ki, mennyire "jó" az az állapot az ágensnek.

**Kétféle döntési helyzet (5. dia) – FONTOS:**
*   **Minimax (Ellenséges):** Itt a hasznosságok **skálája nem számít**, csak a **sorrendje**. (Mindegy, hogy 10 vs 100 vagy 10 vs 10000, ha a nagyobb nyer, ugyanazt választom). Ez a *monoton transzformációkkal szembeni érzéketlenség*.
*   **Expectimax (Átlagos/Bizonytalan):** Itt a **skála (nagyságrend) igenis számít**! Mivel átlagolunk (várható értéket számolunk), nem mindegy, hogy a kockázatért 40 vagy 1600 pontot kaphatok.

---

### 2. A Maximális Várható Hasznosság (MEU) Elve (4. és 14-15. dia) – **KULCSANYAG**

Ez a racionális ágens definíciója bizonytalan környezetben.

*   **MEU (Maximum Expected Utility):** A racionális ágens mindig azt a cselekvést választja, amelyik maximalizálja a **várható** (átlagos) hasznosságot.
*   **Képlet:**
    $$EU(Action) = \sum_{i} P(Outcome_i | Action) \cdot U(Outcome_i)$$
    *(A kimenetelek valószínűsége szorozva a hasznosságukkal, összeadva.)*

---

### 3. A Racionalitás Axiómái (8-13. dia) – **VIZSGATÉTEL**

Hogyan döntjük el, hogy egy viselkedés "racionális"-e? A matematikusok (Ramsey, Neumann, Morgenstern) felállítottak szabályokat (axiómákat). Ha ezeket betartod, akkor racionális vagy, és a viselkedésed leírható egy hasznosságfüggvénnyel.

**Jelölések:**
*   $A \succ B$: $A$-t jobban szeretem, mint $B$-t (preferencia).
*   $A \sim B$: Mindegy, közömbös vagyok.
*   $L = [p, A; 1-p, B]$: Lottó (szerencsejáték), ahol $p$ eséllyel $A$-t kapom, $1-p$ eséllyel $B$-t.

**A 6 Axióma (Elég a lényegüket érteni):**
1.  **Rendezhetőség (Orderability):** Két dolog közül el tudom dönteni, melyik a jobb, vagy hogy egyformák-e. (Nem mondhatom, hogy "nem tudom összehasonlítani").
2.  **Tranzitivitás (Transitivity):** Ha $A > B$ és $B > C$, akkor $A > C$.
    *   **PÉNZPUMPA (Money Pump) - 11. dia:** Ez a tranzitivitás megsértésének következménye. Ha körbeverés van a preferenciáidban ($A > B > C > A$), akkor egy gonosz ágens folyamatosan cserélgetheti veled a tárgyakat úgy, hogy minden cseréért pénzt kér, te belemész (mert jobbat kapsz), de a végén ugyanott vagy, csak elfogyott a pénzed. **Ez irracionális.**
3.  **Folytonosság (Continuity):** Ha $A > B > C$, akkor van egy olyan valószínűségű keveréke (lottó) $A$-nak és $C$-nek, ami pont ugyanolyan jó, mint a biztos $B$.
4.  **Helyettesíthetőség (Substitutability):** Ha $A$ és $B$ nekem egyforma, akkor egy lottóban is kicserélhetem őket, a lottó értéke nem változik.
5.  **Monotonitás:** Ha $A > B$, akkor azt a lottót választom, ahol nagyobb eséllyel kapom meg $A$-t.

**Tétel (14. dia):** Ha egy ágens betartja ezeket az axiómákat, akkor **LÉTEZIK** egy $U$ hasznosságfüggvény, amire igaz, hogy az ágens mindig a várható hasznosságot ($EU$) maximalizálja.

---

### 4. Emberi Hasznosság és a Pénz (16-21. dia)

Hogyan mérjük az emberek hasznosságát?

*   **Hasznosság skálázása:** Mivel a hasznosság egy elméleti szám, transzformálható. $U'(x) = k_1 U(x) + k_2$ (ahol $k_1 > 0$) ugyanazt a viselkedést eredményezi.
*   **Normalizálás:** Gyakran a legrosszabb esetet 0-nak ($u_{\bot}$), a legjobbat 1-nek ($u_{\top}$) vesszük.
*   **Mérés ("Standard Lottery" - 17. dia):** Hogyan mérjük meg, mennyit ér neked egy állapot ($S$)?
    *   Kérdés: Választanád a **biztos S**-t, vagy egy lottót, ahol $p$ eséllyel a **Legjobb** dolgot kapod, $1-p$ eséllyel a **Legrosszabbat**?
    *   Addig állítgatjuk a $p$-t, amíg azt nem mondod: "mindegy". Ekkor $U(S) = p$.

**Pénz vs. Hasznosság (Kockázat):**
A pénz hasznossága **NEM lineáris**!
*   **Kockázatkerülő (Risk Averse):** A görbe **konkáv** (púpos).
    *   *Jelentése:* Az első milliónak sokkal jobban örülök, mint a tizediknek. (Logaritmikus jellegű).
    *   Jobban szeretem a **biztos** 500 Ft-ot, mint 50-50% eséllyel 0 vagy 1000 Ft-ot.
*   **Bizonyossági egyenérték (Certainty Equivalent - 20. dia):** Az a biztos pénzösszeg, amiért cserébe lemondok a kockázatos játékról.
    *   *Példa:* [50% 1000$, 50% 0$]. Ennek a matematikai átlaga (EMV) 500$. De a legtöbb ember inkább elfogadna **biztos 400$-t**, mintsem kockáztasson.
    *   A különbség (500 - 400 = 100$) a **Biztosítási díj (Insurance Premium)**.
*   **Miért léteznek biztosítók? (21. dia):**
    *   Te kockázatkerülő vagy (félted a kocsidat, házadat).
    *   A biztosító (mivel sok ügyfele van és sok pénze) **kockázatsemleges** (lineáris a görbéje).
    *   Neked megéri fizetni egy kicsit a biztonságért, nekik megéri beszedni a pénzt, mert átlagban ők nyernek.

---

### Összefoglaló a vizsgára:

1.  **MEU elv:** Racionális ágens = Max Várható Hasznosság.
2.  **Tranzitivitás:** Ha $A>B$ és $B>C$, akkor $A>C$. Ha nem teljesül -> **Pénzpumpa**.
3.  **Várható érték számolás:** Tudd kiszámolni egy lottó értékét ($p \cdot \text{érték}_1 + (1-p) \cdot \text{érték}_2$).
4.  **Kockázatkerülés:** Tudd, hogy az emberi hasznossággörbe általában konkáv (a biztosat preferáljuk az átlaggal szemben).
5.  **Minimax vs Expectimax:** Minimaxnál csak a sorrend számít, Expectimaxnál a skála (arányok) is.

Ez a diasor az elméleti alapozása a döntéseknek, a következő valószínűleg a Bayes-hálókról vagy konkrét döntési hálókról fog szólni.