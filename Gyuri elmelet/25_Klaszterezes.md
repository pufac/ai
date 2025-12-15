Szia! Ez az anyag a **Nem Felügyelt Tanulás (Unsupervised Learning)**, ezen belül is a **Klaszterezés (Clustering)** témakörét veszi át.

Ez egy nagyon izgalmas terület, mert itt a gépnek **nincs tanára**. Nem mondjuk meg neki, mi a helyes válasz, csak odaöntjük elé az adatokat, és azt mondjuk: *"Találj benne valami rendszert!"*

Itt a részletes, vizsga-fókuszú összefoglaló:

---

### 1. Felügyelt vs. Nem Felügyelt Tanulás (2-4. dia)

Ez az alapvető különbségtétel.

*   **Felügyelt (Supervised):** Van **bemenet ($x$)** és van hozzá helyes **kimenet/címke ($y$)**.
    *   *Példa:* Tanítunk egy gyereket. Mutatunk egy képet ($x$), és megmondjuk: "Ez egy kutya" ($y$). A cél, hogy később felismerje a kutyát ($P(y|x)$).
*   **Nem Felügyelt (Unsupervised):** Csak **bemenetünk ($x$)** van, címkék nincsenek.
    *   *Példa:* Egy kisgyerek játszik a kockákkal. Senki nem mondja meg neki, melyik milyen színű, de magától rájön, hogy a pirosakat egy kupacba, a kékeket egy másikba rakhatja.
    *   **Cél:** Az adatok szerkezetének, eloszlásának ($P(x)$) megismerése.

---

### 2. Miért jó a Klaszterezés? (5. dia)

A klaszterezés csoportosítást jelent. Mire használjuk a való életben?
1.  **Kilógó adatok (Outlier detection):** Ami egyik csoporthoz sem hasonlít, az gyanús. (Pl. bankkártya csalás: minden vásárlásod Magyarországon volt, hirtelen jön egy Japánból -> ez kilóg a klaszterből -> RIASZTÁS).
2.  **Tipikus állapotok:** Rájövünk, hogy a rendszernek vannak stabil állapotai. (Pl. egy gép vagy "Működik", vagy "Melegszik", vagy "Leállt").
3.  **Hiánypótlás:** Ha egy adat hiányos, megnézzük, melyik csoporthoz hasonlít a legjobban, és a csoport átlagával pótoljuk a hiányt.

---

### 3. Távolságmértékek (Distance Metrics) (13-16. dia) – **ALAPVETŐ MATEK**

Ahhoz, hogy csoportosítsunk, tudnunk kell, mi van "közel" és mi "távol". Ehhez kellenek a képletek.

1.  **Euklideszi távolság ($L_2$):** A legrövidebb út légvonalban.
    *   *Képlet:* $\sqrt{(x_2-x_1)^2 + (y_2-y_1)^2}$. (Pitagorasz-tétel).
    *   *Mikor jó?* Geometriai problémáknál, fizikai távolságnál.
2.  **Manhattan távolság ($L_1$):** Városi közlekedés. Csak rácsban (vízszintesen/függőlegesen) mozoghatsz.
    *   *Képlet:* $|x_2-x_1| + |y_2-y_1|$.
    *   *Mikor jó?* Rács alapú világban, vagy nagyon sok dimenziónál.
3.  **Minkowski:** Az előző kettő általánosítása ($p$ paraméterrel).
4.  **Koszinusz hasonlóság:** Két vektor által bezárt szög.
    *   *Mikor jó?* **Szövegeknél!** (Pl. Google keresés). Nem az számít, milyen hosszú a dokumentum, hanem hogy ugyanabba az irányba mutat-e (ugyanazok a szavak vannak-e benne).
5.  **Jaccard:** Halmazok hasonlósága. (Metszet / Unió).
    *   *Példa:* Mennyire hasonlít két vásárló kosara?
6.  **Hamming:** Hány betűben tér el két szó? (Pl. "alma" és "álma" távolsága 1).

---

### 4. K-Közép (K-Means) Algoritmus (7-9. dia) – **A LEGISMERTEBB**

Ez a legegyszerűbb, leggyorsabb módszer, de vannak korlátai.

**Működése (Iteratív):**
1.  **Inicializálás:** Véletlenszerűen ledobunk $K$ darab középpontot (centroidot). (Nekünk kell megmondani, mennyi a $K$!).
2.  **Hozzárendelés:** Minden adatpontot a hozzá legközelebbi centroidhoz sorolunk. (Színezzük a pontokat).
3.  **Frissítés:** Kiszámoljuk az új csoportok súlypontját (átlagát), és oda toljuk a centroidot.
4.  **Ismétlés:** A 2-3. lépést addig csináljuk, amíg a centroidok már nem mozdulnak.

**Előnye:** Gyors, egyszerű.
**Hátránya:**
*   Előre meg kell mondani a $K$-t (hány csoportot keresünk).
*   Csak **gömb alakú** klasztereket talál meg jól.
*   Érzékeny arra, hova dobjuk le az elején a pontokat (beragadhat rossz helyre).

---

### 5. Hierarchikus Klaszterezés (20-32. dia)

Itt nem kell megmondani előre a csoportszámot, mert egy **fát (dendrogramot)** építünk.

**A) Bottom-Up (Agglomeratív) – A gyakoribb:**
*   *Kezdés:* Minden pont egy külön klaszter.
*   *Lépés:* Megkeressük a **két legközelebbi** klasztert, és összeolvasztjuk őket.
*   *Vége:* Addig csináljuk, amíg egyetlen nagy klaszter marad.
*   A dendrogramon (fán) utólag elvághatjuk a vonalat ott, ahol nekünk tetszik a csoportszám.

**Hogyan mérjük két CSOPORT távolságát? (Linkage - 25-28. dia):**
Ez kritikus kérdés!
1.  **Single Linkage (Legközelebbi szomszéd):** A két csoport legközelebbi pontjainak távolsága.
    *   *Hatás:* **Láncosodás (Chaining).** Hosszú, kígyózó klasztereket talál meg.
2.  **Complete Linkage (Legtávolabbi szomszéd):** A két csoport legtávolabbi pontjainak távolsága.
    *   *Hatás:* Kompakt, gömbölyű kis gombócokat csinál.
3.  **Average Linkage:** Az átlagos távolság. (Középút).
4.  **Centroid:** A tömegközéppontok távolsága.

**B) Top-Down (Divisive):**
*   Fordítva: Egy nagy csoportból indulunk, és vágjuk ketté, amíg el nem fogy. (Ritkábban használják, mert számításigényesebb).

---

### 6. DBSCAN (Sűrűség alapú) (33-39. dia) – **A MODERN KEDVENC**

A K-Means nem talál meg fura alakzatokat (pl. kifli alakú csoportot), a Hierarchikus meg lassú. Itt jön a DBSCAN.

**Alapötlet:** A klaszter ott van, ahol **sűrűn** vannak a pontok. Ahol ritkák, az zaj.

**Két paramétere van:**
1.  **$\epsilon$ (Epsilon):** Mekkora sugarú körben nézelődünk?
2.  **MinPts:** Hány pontnak kell lennie a körben, hogy "sűrűnek" hívjuk?

**A pontok típusai:**
*   **Mag (Core):** Van legalább *MinPts* szomszédja az $\epsilon$ sugarú körben. (Ez a sűrű közepe).
*   **Határ (Border):** Nincs elég szomszédja, de elérhető egy Mag pontból. (A klaszter széle).
*   **Zaj (Noise):** Nincs elég szomszédja, és nem is ér el Mag pontot. (Különálló pont, kuka).

**Működése:**
Választunk egy pontot. Ha Mag pont, akkor "megfertőzi" a szomszédait, és a szomszédai is a szomszédaikat... így a klaszter "végigfolyik" a sűrű részeken, bármilyen alakja is van (akár egy spirál vagy kifli).

**Előnye:**
*   Nem kell megadni a csoportszámot!
*   Bármilyen alakzatot felismer.
*   Kiszűri a zajt (nem sorol be mindent kötelezően).
**Hátránya:**
*   Nehéz beállítani az Epsilont és a MinPts-t.
*   Ha változó a sűrűség (az egyik csoport sűrű, a másik ritkább), akkor nem működik jól.

---

### 7. Condorcet / Szavazás alapú (40-44. dia)

Ez egy ritkábban tárgyalt, speciális módszer.
*   **Lényeg:** Olyan, mint egy választás. A változók (vagy különböző klaszterezési eredmények) "szavaznak" arról, hogy két objektum (A és B) közül melyik a "jobb" vagy hogy egy csoportba tartoznak-e.
*   A mátrix (41. dia) azt mutatja: a sorban lévő jelölt hányszor győzte le az oszlopban lévőt.
*   Ez inkább **Ensemble Clustering** (több módszer eredményének összefésülése) esetén kerül elő.

---

### Összefoglaló a vizsgára (Ezeket tanuld meg!):

1.  **K-Means:** Centroidok mozgatása. $K$-t előre meg kell adni. Csak gömböket talál.
2.  **Hierarchikus (Agglomeratív):** Összeolvasztás. Dendrogram.
    *   *Linkage típusok:* Single (lánc), Complete (gömb), Average.
3.  **DBSCAN:** Sűrűség alapú. $\epsilon$ és $MinPts$. Bármilyen alakot megtalál, kezeli a zajt.
4.  **Távolságmértékek:** Tudd, mikor kell Euklideszi (térbeli), mikor Manhattan (rács), és mikor Koszinusz (szöveg/irány).
5.  **Klaszterszám meghatározása:** Nincs rá tökéletes módszer (hüvelykujj szabályok vannak, vagy pl. a "könyök módszer" a K-meansnél - bár ez a dián nincs, de jó tudni).

Ha érted, miért jobb a **DBSCAN** egy bonyolult, kanyargós adatnál, mint a **K-Means**, akkor érted a lényeget!