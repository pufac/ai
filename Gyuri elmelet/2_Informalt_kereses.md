Szia! Ez a diasor az **Informált Kereséseket (Informed Search)** tárgyalja. Ez a logikus folytatása az előző anyagnak (ahol a „vak” keresésekről volt szó).

Itt már nem céltalanul bolyongunk a sötétben, hanem van egy „térképünk” vagy „iránytűnk”, ami segít eldönteni, merre érdemes menni. Ez a téma **kritikus fontosságú a vizsgán**, különösen az **A* algoritmus** és a **heurisztikák tulajdonságai**.

Íme a részletes, vizsga-fókuszú magyarázat:

---

### 1. Mi az a Heurisztika? ($h(n)$) (6-10. dia)

A vak keresések (pl. szélességi) csak azt tudják, hogy „ez szomszédos azzal”. Az informált keresések viszont kapnak egy **becslést**.

*   **Definíció:** A heurisztikus függvény, jelölése **$h(n)$**, megmondja, hogy az $n$ állapotból **becslésünk szerint** mekkora költség (pl. távolság) eljutni a célba.
*   **Lényeg:**
    *   Ez csak egy *becslés* (nem biztos, hogy pontos).
    *   Probléma-specifikus (más kell sakkhoz, más útvonaltervezéshez).
*   **Példa (Útvonaltervezés):**
    *   Szeretnék eljutni Aradról Bukarestbe.
    *   $h(n)$ = **Légvonalbeli távolság** Bukarestig. Ezt könnyű kiszámolni, és segít irányban maradni.

---

### 2. Mohó Keresés (Greedy Search) (11-17. dia)

Ez a legegyszerűbb informált keresés.

*   **Stratégia:** Mindig azt a csomópontot fejti ki következőnek, amelyik a heurisztika szerint a **legközelebb** van a célhoz.
*   **Döntés alapja:** Minimalizálja $h(n)$-t.
*   **Működése:** Olyan, mintha mindig a toronyirányt követnéd.
*   **Tulajdonságai (Vizsgán kérdezik!):**
    *   **Nem Optimális:** Simán bevisz egy zsákutcába vagy egy hosszabb útra, csak azért, mert az elején jónak tűnt (lásd 16. dia ábráját: átmegy a folyón, majd vissza kell jönnie hídért).
    *   **Nem Teljes:** Végtelen ciklusba kerülhet (ha nincs detektálva az ismétlődés).
    *   **Idő/Tár:** $O(b^m)$ (akár az összes csomópontot bejárhatja rossz esetben).
*   **Összegzés:** Gyors lehet, de buta, mert nem veszi figyelembe, hogy mennyit utaztunk már, csak azt nézi, milyen messze *hiszi* a célt.

---

### 3. Az A* (A-csillag) Keresés (18-22. dia) – **A LEGFONTOSABB ALGORITMUS**

Az A* egyesíti az **Egyenletes Költségű Keresést (UCS)** (ami a múltat nézi) és a **Mohó Keresést** (ami a jövőt nézi).

*   **A Képlet (Kívülről tudd!):**
    $$f(n) = g(n) + h(n)$$
    *   **$g(n)$**: A kezdőponttól az $n$-ig megtett út **tényleges költsége** (múlt).
    *   **$h(n)$**: Az $n$-től a célig hátralévő út **becsült költsége** (jövő).
    *   **$f(n)$**: A teljes út becsült költsége a starttól a célig, $n$-en keresztül.

*   **Stratégia:** Mindig a legkisebb $f(n)$ értékű csomópontot választja a peremről.
*   **Mikor áll meg? (21. dia - Becsapós kérdés!):**
    *   NEM akkor, amikor a cél bekerül a sorba (peremre).
    *   HANEM akkor, amikor a célt **kivesszük a sorból és kifejtjük**. (Azért, mert lehet, hogy találtunk egy utat a célhoz, de még van a sorban egy másik ígéretes út, ami rövidebb lehet).

---

### 4. A Heurisztika Tulajdonságai: Elfogadhatóság (Admissibility) (23-30. dia)

Ahhoz, hogy az A* garantáltan megtalálja a legjobb (optimális) megoldást, a heurisztikának "jól viselkedőnek" kell lennie.

**Fa-keresés esetén (Tree Search):**
A heurisztikának **Elfogadhatónak (Admissible)** kell lennie.
*   **Definíció:** $0 \le h(n) \le h^*(n)$
    *   Ahol $h^*(n)$ a *valós* költség a célig.
*   **Jelentése:** A heurisztika **soha nem becsülheti túl** a valós költséget. Legyen **optimista**.
    *   *Példa:* A légvonalbeli távolság sosem lehet hosszabb, mint a kanyargós úton mért távolság. Tehát a légvonal elfogadható heurisztika.
*   **Miért fontos?** Ha a heurisztika túlbecsülné a távolságot, az A* azt hihetné egy jó útról, hogy rossz, és nem vizsgálná meg. Az elfogadhatóság garantálja az optimalitást Fa-keresésnél.

---

### 5. Fa-keresés vs. Gráf-keresés (44-50. dia) – **KRITIKUS KÜLÖNBSÉG**

Az előző anyagban tanultuk: a Gráf-keresés abban különbözik a Fa-kereséstől, hogy van egy **Zárt Halmaz (Closed Set)**, tehát megjegyzi, hol járt már, és nem megy oda vissza.

**A probléma (49. dia):**
Ha sima "Elfogadható" heurisztikát használunk Gráf-keresésnél, az A* **elveszítheti az optimalitást**. (Előfordulhat, hogy egy csomópontot először egy rosszabb úton érünk el, betesszük a zárt halmazba, és később, amikor megtaláljuk a rövidebb utat oda, már nem foglalkozunk vele).

**A megoldás: Konzisztencia (Consistency) (50. dia):**
Gráf-keresésnél erősebb feltétel kell a heurisztikára!
*   **Definíció (Monotonitás):** $h(n) \le c(n, a, n') + h(n')$
    *   Magyarul: A heurisztika értéke két szomszédos csomópont között nem csökkenhet jobban, mint a köztük lévő út költsége. (Háromszög-egyenlőtlenség).
*   **Következmény:** Ha a heurisztika konzisztens, akkor az $f(n)$ értékek az útvonal mentén sosem csökkennek.
*   **Szabály:** Minden konzisztens heurisztika egyben elfogadható is.

**Összefoglalva a vizsgára:**
*   **Fa-kereséshez** elég, ha $h(n)$ **Elfogadható**.
*   **Gráf-kereséshez** (zárt halmazzal) kell, hogy $h(n)$ **Konzisztens** legyen.

---

### 6. Heurisztikák Készítése (36-43. dia)

Hogyan találjunk ki jó heurisztikát?
*   **Relaxált Probléma:** Veszünk az eredeti problémát, és eltörlünk szabályokat (könnyítünk rajta). A könnyített probléma megoldása lesz a heurisztika.
*   **Példa: 8-as kirakó (Sliding Puzzle):**
    *   *Szabály:* Csak a lyukba lehet tolni a szomszédot.
    *   *Relaxáció 1:* A kockák bárhová áthelyezhetők. -> Heurisztika ($h_1$): **Rossz helyen lévő elemek száma**.
    *   *Relaxáció 2:* A kockák bárhová tolhatók (egymáson át is), de csak lépésenként. -> Heurisztika ($h_2$): **Manhattan távolság** (vízszintes + függőleges távolságok összege).
*   **Dominancia:** Ha $h_2(n) \ge h_1(n)$ minden esetben (és mindkettő elfogadható), akkor $h_2$ **dominálja** $h_1$-et. A nagyobb értékű heurisztika a jobb (mert közelebb van a valósághoz, így kevesebb csomópontot kell kifejteni).

---

### Összefoglaló puska a vizsgára:

1.  **Mohó Keresés:** Csak $h(n)$-t néz. Gyors, de nem optimális, nem teljes.
2.  **A* Keresés:** $f(n) = g(n) + h(n)$-t minimalizál.
3.  **Elfogadható (Admissible) $h(n)$:** $h(n) \le$ valós költség. (Optimista). -> Kell a **Fa-keresés** optimalitásához.
4.  **Konzisztens (Consistent) $h(n)$:** $h(n) - h(n') \le$ lépésköltség. (Monoton). -> Kell a **Gráf-keresés** optimalitásához.
5.  **Relaxáció:** Szabályok elhagyása, hogy heurisztikát generáljunk (pl. légvonalbeli távolság, mert ott átmehetsz a falon).
6.  **A* viselkedése:** Ha $h(n)=0$, akkor A* = Egyenletes Költségű Keresés (UCS).

Ez a diasor magja. Ha érted az A* képletét, és a különbséget az elfogadható és konzisztens között, akkor rendben leszel! Küldheted a következőt!