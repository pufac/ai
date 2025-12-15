Rendben, vettem a kérést! Mostantól részletesebben, mélyebb összefüggésekkel magyarázom el az anyagot, hogy ne csak "bemagold", hanem értsd is a miérteket. Ez a diasor a **Szimbolikus Mesterséges Intelligencia** szívét, a **Logikát és a Következtetést** tárgyalja.

Ez egy vízválasztó téma: eddig (keresés, CSP) a gép "csak" próbálgatott vagy optimalizált. Most viszont **tudást ábrázol** és **új ismereteket vezet le** a meglévőkből.

Íme a részletes, vizsga-fókuszú feldolgozás:

---

### 1. Bevezetés: Tudásalapú Rendszerek (5-11. dia)

A klasszikus MI (kb. 1970-1990) csúcsa a **Szakértői Rendszerek** voltak.
*   **Alapelv:** Az emberi szakértő tudását (pl. orvos, mérnök) szabályok formájában rögzítjük.
*   **Felépítés (9-10. dia):**
    1.  **Tudásbázis (Knowledge Base - TB):** Tények és szabályok halmaza. Ez a "lexikális tudás".
    2.  **Következtető Gép (Inference Engine):** A logika motorja. Ez a program, ami a szabályokat alkalmazza a tényekre, hogy újat mondjon.
*   **Működési ciklus:** Érzékelés $\to$ Új tények $\to$ Illesztés (melyik szabály alkalmazható?) $\to$ Konfliktuskezelés (ha több is, melyiket válasszuk?) $\to$ Szabály elsütése (új tény a memóriába/cselekvés).

**Példa (MYCIN - 7. dia):** Orvosi diagnosztikai rendszer.
*   *Szabály:* HA (baktérium gram-negatív) ÉS (pálcika alakú) ... AKKOR (ez E.coli 0.6-os valószínűséggel).

---

### 2. A Következtetés Típusai (13-14. dia) – **FONTOS ELMÉLET**

Hogyan jutunk A-ból B-be gondolatban? Három fő út van:

1.  **Dedukció (Logikai levezetés):**
    *   *Működés:* Általános szabályból az egyedire.
    *   *Példa:* Minden ember halandó. Szókratész ember. $\to$ Szókratész halandó.
    *   *Jellemző:* **Igazságtartó.** Ha a premisszák (feltételek) igazak, a konklúzió *biztosan* igaz. A matematika és a logikai ágensek erre épülnek.

2.  **Indukció (Tanulás):**
    *   *Működés:* Egyedi esetekből az általános szabályra.
    *   *Példa:* Láttam 1000 hattyút, mind fehér volt. $\to$ Minden hattyú fehér.
    *   *Jellemző:* **Nem garantáltan igaz** (jöhet egy fekete hattyú), de ez a gépi tanulás alapja (adatból modellt építünk).

3.  **Abdukció (Diagnózis / "Belátás"):**
    *   *Működés:* Az okozatból következtetünk az okra.
    *   *Példa:* Vizes a fű (okozat). $\to$ Valószínűleg esett az eső (ok). (De lehet, hogy locsoltak).
    *   *Jellemző:* Ez az orvosi diagnózis alapja. Nem matematikailag biztos, de ez a legvalószínűbb magyarázat keresése.

---

### 3. Ítéletkalkulus (Propositional Logic) (18-26. dia)

Ez a "butábbik" logika, de a modern rendszerek alapja. Itt **mondatok** vannak, amik vagy Igazak, vagy Hamisak.

**Alapfogalmak, amiket keverni szoktak (tisztázzuk!):**
*   **Szintaktika:** Hogyan *írjuk le* helyesen? (Formai szabályok). Pl. $A \land B$ helyes, $A \lor \to$ helytelen.
*   **Szemantika:** Mit *jelent*? (Igazságtartalom). Pl. $A \land B$ akkor igaz, ha A is és B is igaz.
*   **Vonzat (Entailment, $TB \models \alpha$):** Azt jelenti, hogy $\alpha$ **logikai következménye** a tudásbázisnak.
    *   *Definíció:* Minden olyan lehetséges világban (modellben), ahol a TB igaz, ott $\alpha$-nak is igaznak kell lennie.
*   **Levezetés (Inference, $TB \vdash \alpha$):** Az a konkrét algoritmus/számítás, amivel a gép megpróbálja bebizonyítani, hogy a vonzat fennáll.

**A két legfontosabb tulajdonság egy algoritmusnál:**
1.  **Helyesség (Soundness):** Csak olyat bizonyít be, ami tényleg igaz. (Nem hazudik).
2.  **Teljesség (Completeness):** Ha valami igaz, azt be is tudja bizonyítani. (Mindent megtalál).

---

### 4. Rezolúció (Resolution) (3-5. és 28-32. dia) – **VIZSGATÉTEL**

A gépi bizonyítás "Svájci bicskája". Miért szeretjük? Mert **cáfolat-teljes**. Ez azt jelenti, hogy ha van ellentmondás a rendszerben, azt garantáltan megtalálja.

**Az alapötlet (Cáfolatos bizonyítás):**
Nem azt próbáljuk bizonyítani, hogy $TB \to Állítás$ igaz, hanem feltesszük, hogy az Állítás **HAMIS** ($\neg Állítás$), hozzáadjuk a tudásbázishoz, és megnézzük, vezet-e ez ellentmondáshoz (összeomlik-e a világ). Ha igen, akkor az eredeti állításnak igaznak kellett lennie.

**A Rezolúciós Lépés:**
$$ (A \lor B) \quad \text{és} \quad (\neg B \lor C) \implies (A \lor C) $$
*   Magyarázat: Vagy $B$ igaz, vagy nem.
    *   Ha $B$ igaz, akkor a második tagból $(\neg B \lor C)$ a $\neg B$ hamis, tehát $C$-nek muszáj igaznak lennie.
    *   Ha $B$ hamis, akkor az első tagból $(A \lor B)$ az $A$-nak muszáj igaznak lennie.
    *   Tehát vagy $A$, vagy $C$ biztosan igaz. $B$ és $\neg B$ "kiütik" egymást.

**Algoritmus (31. dia):**
1.  A tudásbázist és a negált célt átalakítjuk **Klóz formára (CNF)**. (Csak VAGY és NEM kapcsolatok lehetnek, ÉS-sel összefűzve).
2.  Választunk két klózt, amiben van egy ellentétes pár (pl. $P$ és $\neg P$).
3.  Összevonjuk őket (a párt töröljük).
4.  Ezt ismételjük.
5.  Ha **üres klózt** ($\emptyset$) kapunk, az az ellentmondás! $\to$ Bizonyítva.

---

### 5. Elsőrendű Logika (First-Order Logic - FOL) (7-11. dia)

Az ítéletkalkulus gyenge, mert nem tudja kezelni az objektumokat. Nem tudod leírni, hogy "Minden tanuló fáradt", csak úgy, hogy "Jóska fáradt", "Pista fáradt", "Mari fáradt"... Ez végtelen hosszú lenne.

Az FOL bevezeti:
*   **Objektumok:** János, alma, BME.
*   **Predikátumok (Relációk):** $Ember(x)$, $Szeret(x, y)$. Ezek az állítások, amik lehetnek igazak vagy hamisak.
*   **Függvények:** $Apja(x)$. Ez nem állítás, hanem egy objektumra mutat! (János apja).
*   **Kvantorok (EZT NAGYON ÉRTSD!):**
    *   $\forall x$ (Univerzális): "Minden x-re igaz, hogy..."
    *   $\exists x$ (Egzisztenciális): "Létezik (legalább egy) olyan x, amire igaz, hogy..."

**Veszélyes hiba a vizsgán:**
*   A $\forall$ mellé általában $\to$ (implikáció) kell. (Minden ember halandó: $\forall x (Ember(x) \to Halando(x))$). Ha ÉS-t ($\land$) használsz, azt mondod: "A világon mindenki ember és mindenki halandó".
*   Az $\exists$ mellé általában $\land$ (konjunkció) kell. (Van piros alma: $\exists x (Alma(x) \land Piros(x))$). Ha nyilat ($\to$) használsz, az nagyon gyenge állítás lesz (pl. igaz lesz akkor is, ha nincs is alma a világon).

---

### 6. Bizonyítás Elsőrendű Logikában (16-30. dia)

Itt is a rezolúciót használjuk, de van egy csavar: a **változók ($x, y$)**.
Hogyan "ütjük ki" egymást a $Férfi(János)$ és a $\neg Férfi(x)$ állításokkal?
Úgy, hogy az $x$-et behelyettesítjük $János$-ra.

**Unifikálás (Egyesítés - 19. dia):**
Ez az a folyamat, amikor keressük azt a behelyettesítést ($\theta$), amitől két kifejezés egyformává válik.
*   Pl. $Ismer(János, x)$ és $Ismer(y, Kati)$ $\to$ Akkor egyeznek, ha $y=János$ és $x=Kati$.

**Klóz formára hozás FOL-ban (20. dia) – Ez egy 10 lépéses recept, gyakori vizsgafeladat!**
A legtrükkösebb lépés a **Skolemizálás** (4. lépés):
*   Az egzisztenciális kvantorok ($\exists$) eltüntetése.
*   Ha azt mondom: "Létezik valaki, aki ellopta az autót", a logikában ezt úgy írjuk át, hogy adunk neki egy nevet (konstanst), pl. $TolvajÚr$.
*   Ha a létezés függ valamitől (pl. "Mindenkinek van egy anyja" - $\forall x \exists y Anya(y, x)$), akkor nem konstanst, hanem **függvényt** vezetünk be: $Anya(anyja(x), x)$.

---

### 7. Wumpus Világ Példa (37-50. dia)

Ez a gyakorlati alkalmazás.
*   **Helyzet:** Ágens egy rácsban.
*   **Szabályok:**
    *   $B_{x,y} \iff (W_{x-1,y} \lor W_{x+1,y} \lor \dots)$ (Bűz van $\iff$ szomszédban Wumpus).
    *   $S_{x,y} \iff (C_{x-1,y} \lor C_{x+1,y} \lor \dots)$ (Szellő van $\iff$ szomszédban Csapda).
*   **Következtetés menete:**
    1.  [1,1]-ben vagyunk, nincs bűz ($\neg B_{1,1}$).
    2.  Szabály: $\neg B_{1,1} \to \neg W_{1,2} \land \neg W_{2,1}$.
    3.  Tehát tudjuk, hogy [1,2] és [2,1] biztonságos a szörnytől.
    4.  Ha [2,1]-ben szellőt érzünk, de [1,1]-ben nem volt, akkor kikövetkeztetjük, hogy a csapda nem lehet [1,1]-ben, tehát [2,2]-ben vagy [3,1]-ben kell lennie.

---

### 8. Ontológiák (35-40. dia)

Ez már a tudás strukturálása. Ha sok szabályunk van, rendszerezni kell őket.
*   **Ontológia:** Egy szakterület (domain) fogalmainak és kapcsolatainak formális leírása. (Pl. Orvosi ontológia, Gén ontológia).
*   **Különbség az adatbázishoz képest:** Az adatbázis csak adatot tárol, az ontológia a *jelentést* (szemantikát) és a logikai kapcsolatokat (pl. "A kutya az egy állat", tehát ha valami kutya, akkor örökli az állat tulajdonságait).

---

### Vizsga összefoglaló (Mit kell tudni?):

1.  **Rezolúciós bizonyítás papíron:** Adnak 2-3 mondatot (pl. "Aki tanul, az átmegy", "Jóska tanul"). Át kell írnod klózokra (CNF), negálni a célt ("Jóska nem megy át"), és rezolúcióval levezetni az üres klózt.
2.  **Unifikálás:** Adnak két kifejezést, meg kell mondani, milyen változó-behelyettesítéssel lesznek egyformák.
3.  **Skolemizálás:** Tudd, hogyan kell eltüntetni az $\exists$ jelet (konstansra vagy függvényre cserélni).
4.  **Helyesség vs. Teljesség:** Definíciók.
5.  **Horn-klózok:** Tudd, hogy ezek egyszerűbbek (csak 1 pozitív állítás), ezért gyorsabbak (lineáris idő), de nem mindent lehet így leírni.

Ha a rezolúciós levezetést megérted a példák alapján (24-30. dia), akkor a vizsga nehezén túl vagy!