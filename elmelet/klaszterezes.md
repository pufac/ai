Szívesen! Ez a diacsomag a **Nem Felügyelt Tanulás (Unsupervised Learning)**, ezen belül is a **Klaszterezés** világába vezet be. Ez az az ága a mesterséges intelligenciának, ahol a gépnek "nincs tanára", hanem önállóan kell felfedeznie a rendet az adatokban.

Itt az összefoglaló, az alapfogalmaktól a konkrét algoritmusokig:

---

### 1. Az Alaphelyzet: Felügyelt vs. Nem Felügyelt (Slide 2-4)

A legfontosabb különbség a "címke" (a helyes válasz) megléte vagy hiánya.

*   **Felügyelt Tanulás:** Van egy $(x, y)$ párunk.
    *   $x$: Bemenet (pl. egy kép).
    *   $y$: Címke (pl. "Ez egy macska").
    *   **Cél:** Megtanulni, hogy $x$-ből $y$ lesz. (Ezt csináltuk eddig a Döntési fáknál, Neurális hálóknál).
*   **Nem Felügyelt Tanulás:** Csak $x$-ünk van.
    *   Nincs címke ($y$). Nem tudjuk, mi a "helyes".
    *   **Cél:** Megtalálni az adatok **belső szerkezetét**, csoportosítani a hasonló dolgokat.
    *   **Klaszterezés:** Az adatok felosztása olyan csoportokra (klaszterekre), hogy a csoporton belüli elemek hasonlítsanak, a csoportok közöttiek pedig különbözzenek.

*   **Miért jó ez?** (Slide 5)
    *   **Kilógó adatok (Outlier) keresése:** Ami egyik csoporthoz sem tartozik, az gyanús (pl. banki csalás).
    *   **Tipikus állapotok megtalálása:** Pl. vásárlói típusok azonosítása.
    *   **Hiánypótlás:** Ha valakiről kevés adatunk van, besoroljuk egy csoportba, és feltételezzük róla a csoport átlagos tulajdonságait.

---

### 2. A Két Fő Megközelítés (Slide 20, 29)

Hogyan építjük fel a csoportokat? Két irányból indulhatunk:

1.  **Bottom-Up (Agglomeratív):**
    *   **Kezdés:** Minden egyes adatpont egy saját, apró "klaszter". (N db klaszter).
    *   **Lépés:** Megkeressük a két legközelebbi klasztert, és **összeolvasztjuk** őket.
    *   **Vége:** Addig csináljuk, amíg el nem érjük a kívánt klaszterszámot (vagy amíg mindenki egyetlen óriásklaszterbe olvad).
    *   **Előny:** Természetes, "fa-szerű" hierarchiát (dendrogramot) ad.

2.  **Top-Down (Megosztó):**
    *   **Kezdés:** Mindenki egyetlen óriásklaszterben van.
    *   **Lépés:** A legnagyobb/legszélesebb klasztert **kettévágjuk**.
    *   **Vége:** Addig vágunk, amíg el nem érjük a célt (vagy mindenki egyedül nem lesz).
    *   **Hátrány:** Nehezebb dönteni a vágásról, mint az összeolvasztásról, és számításigényesebb.

---

### 3. Az Algoritmusok (Hogyan csináljuk?)

#### A) K-Means (K-közép) - A "Klasszikus"
*   **Működése (Slide 7):**
    1.  Kijelölünk $K$ darab véletlenszerű középpontot (centroidot).
    2.  Minden adatpontot ahhoz a középponthoz sorolunk, amelyikhez a legközelebb van.
    3.  Kiszámoljuk az így létrejött csoportok **új** súlypontját (átlagát).
    4.  A középpontokat áthelyezzük az új súlypontokba.
    5.  Ismételjük a 2-4. lépést, amíg a pontok már nem vándorolnak.
*   **Előny:** Gyors, egyszerű.
*   **Hátrány:** Előre meg kell mondani a $K$-t (klaszterek száma). Csak gömb alakú csoportokat talál meg jól. Érzékeny a kezdőpozícióra.

#### B) Hierarchikus Klaszterezés (Linkage módszerek) - A "Faépítő"
*   Ez a Bottom-Up módszer megvalósítása. A fő kérdés: **Hogyan mérjük két csoport távolságát?** (Slide 24-28)
    1.  **Single Linkage (Legközelebbi szomszéd):** A két legközelebbi pont távolsága. (Hajlamos "kígyózó", láncszerű klasztereket csinálni).
    2.  **Complete Linkage (Legtávolabbi szomszéd):** A két legtávolabbi pont távolsága. (Kompakt gömböket csinál).
    3.  **Average Linkage:** Az összes pontpár távolságának átlaga. (Kiegyensúlyozott).
    4.  **Centroid Linkage:** A két csoport súlypontjának távolsága.

#### C) DBSCAN (Sűrűség alapú) - A "Modern"
*   **Filozófia:** A klaszter egy **sűrű** pontfelhő, amit ritka tér választ el a többitől. (Slide 33-36)
*   **Paraméterek:** `Epsilon` (távolság), `MinPts` (hány szomszéd kell).
*   **Működése:**
    *   Keresünk egy pontot, aminek a környezetében sok másik pont van ("magpont").
    *   Ezt a sűrű régiót addig bővítjük, amíg lehet (láncreakció).
    *   Aki sehova nem tartozik (ritka térben van), az **Zaj (Outlier)**.
*   **Előny:** Bármilyen alakzatot (pl. kifli, gyűrű) felismer. Nem kell megadni a klaszterek számát. Kezeli a zajt.
*   **Hátrány:** Érzékeny a paraméterekre. Nem szereti, ha a sűrűség változó.

---

### 4. Távolságfüggvények (Mivel mérünk?)
Mivel a hasonlóság a lényeg, kritikus, hogyan mérjük a távolságot. (Slide 13-16)

*   **Euklideszi (L2):** "Légvonalban". A klasszikus távolság.
*   **Manhattan (L1):** "Városi séta". Csak tengelyirányban (rácsokon) lehet menni.
*   **Minkowski:** Az előző kettő általánosítása.
*   **Jaccard:** Halmazokhoz (pl. "hány közös ismerősünk van?").
*   **Hamming:** Szövegekhez/Binárishoz (pl. "hány betűben tér el a két szó?").
*   **Koszinusz:** Vektorok szögét méri (pl. szövegelemzésnél, mennyire "egy irányba" mutat a téma).

---

### Összefoglalva a vizsgára:

*   **Nem felügyelt tanulás:** Nincs címke, csak a struktúrát keressük.
*   **K-Means:** Centroidokat tologatunk. Gyors, de gömböket keres és kell a K.
*   **Hierarchikus:** Fát épít (dendrogram). A Linkage (Single/Complete/Average) dönti el az összevonást.
*   **DBSCAN:** Sűrűséget keres. Alaktartó, zajtűrő, nem kell K.
*   **Távolságok:** Az adatok típusától függ, melyiket használjuk (Euklideszi a leggyakoribb, de szövegre Jaccard/Koszinusz/Hamming jobb).