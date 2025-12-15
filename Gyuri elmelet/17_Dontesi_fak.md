Szia! Ez az anyag a **Gépi Tanulás (Machine Learning)** kapuján vezet be, azon belül is a **Döntési Fák (Decision Trees)** világába.

Ez az egyik legkönnyebben érthető és legemberibb gondolkodású algoritmus. Ha játszottál már "Barkochbát", akkor már érted az alapelvét.

Íme a részletes, példákkal gazdagított magyarázat:

---

### 1. Hol vagyunk a térképen? (Gépi Tanulás alapjai) (4-10. dia)

Eddig mi írtuk meg a szabályokat (pl. "Ha bűz van, akkor ott a Wumpus"). De mi van, ha nem tudjuk a szabályokat, csak rengeteg adatunk van? Ekkor **tanulunk**.

**A tanulás fajtái (7-8. dia) – Vizsgaelmélet:**
1.  **Felügyelt tanulás (Supervised Learning):** Van egy "tanár", aki megmondja a helyes választ.
    *   *Példa:* Mutatunk a gépnek 1000 képet kutyákról és macskákról, és mindegyiknél megmondjuk: "Ez kutya", "Ez macska". A gépnek rá kell jönnie a különbségre. (A döntési fa ide tartozik!).
2.  **Felügyelet nélküli tanulás (Unsupervised Learning):** Nincs tanár, csak adat. A gépnek magának kell csoportokat (klasztereket) találnia.
    *   *Példa:* Vásárlói szokások elemzése – "Ezek a vevők hasonlítanak egymásra".
3.  **Megerősítéses tanulás (Reinforcement Learning):** Nincs tanár, de van "jutalom/büntetés". (Ezt vettük az előző anyagban a robotautóval).

**A cél (10. dia):** Van egy ismeretlen $f(x)$ függvény a valóságban (pl. ki fogja visszafizetni a hitelt?). Mi ezt akarjuk közelíteni egy $h(x)$ hipotézissel (modellel) a példák alapján úgy, hogy az **új, sosem látott esetekre** is jól működjön.

---

### 2. A Döntési Fa Felépítése (12-16. dia)

A döntési fa olyan, mint egy folyamatábra (flowchart).

**A "Várjunk-e az étteremben?" példa (13. dia):**
Éhesek vagyunk, de tele az étterem. Várjunk asztalra vagy menjünk máshova?
A döntésünk sok mindentől függ (ezek az **attribútumok**):
*   Van-e másik étterem a közelben? (*Alternatíva*)
*   Van-e bár, ahol várakozhatunk? (*Bár*)
*   Péntek/Szombat este van? (*Pén/Szom*)
*   Éhesek vagyunk? (*Éhes*)
*   Mennyien vannak? (*Kuncsaft*: Senki, Néhány, Tele)
*   ...stb.

**A fa részei (15. dia):**
1.  **Gyökér/Belső csomópont (Téglalap):** Egy kérdés (teszt) valamelyik attribútumra. Pl. "Hányan vannak?" (*Kuncsaft*).
2.  **Él (Nyíl):** A válasz a kérdésre. Pl. "Tele", "Néhány", "Senki".
3.  **Levél (Szürke doboz):** A végső döntés (osztály). Pl. "Igen, várjunk" vagy "Nem, menjünk el".

**Hogyan működik? (16. dia példája):**
Bejön egy új szituáció: *Tele van az étterem, Éhesek vagyunk, Olasz étterem.*
1.  Kérdés: *Kuncsaft?* -> Válasz: *Tele*. (Megyünk a középső ágon lefelé).
2.  Kérdés: *Várakozási idő?* -> Válasz: *30-60 perc*.
3.  Kérdés: *Van alternatíva?* -> Válasz: *Nincs*.
4.  Levél: **Várjunk (Igen)**.

---

### 3. A Döntési Fa Tanulása – Hogyan építsük fel? (19-24. dia)

Ez a legfontosabb rész! Van egy táblázatunk a múltbeli tapasztalatainkkal (14. dia: Tanítóhalmaz). Hogyan lesz ebből fa?

**A Mohó (Greedy) Algoritmus (ID3):**
Mindig azt a kérdést (attribútumot) tesszük fel először, ami a **legjobban szétválasztja** a példákat.

**Mit jelent a "legjobb"? (23-24. dia):**
*   **Tökéletes attribútum:** Ha aszerint vágom szét a halmazt, az egyik kupacban *csak* IGEN, a másikban *csak* NEM lesz. (Pl. *Kuncsaft* = "Néhány" -> Mindenki bement, tehát IGEN. Ez egy tiszta csoport).
*   **Haszontalan attribútum:** Ha szétvágom, a két kupacban ugyanúgy vegyesen vannak az igenek és nemek. (Pl. *Típus*: A francia és a thai étteremben is volt, hogy vártunk, meg volt, hogy nem. Ez nem segít a döntésben).

**A cél:** A lehető leggyorsabban eljutni a tiszta (homogén) levelekig.

---

### 4. Entrópia és Információ Nyereség (25-31. dia) – **A MATEK LELKE**

Hogyan mérjük számmal, hogy mennyire "jó" egy attribútum? Az **Információelméletet** hívjuk segítségül.

**1. Entrópia ($H$): A bizonytalanság mértéke.**
*   Ha egy érmét feldobsz (50-50% fej/írás), az **nagy entrópia** (bizonytalanság). (H=1 bit).
*   Ha az érme cinkelt (99% fej), az **kis entrópia** (szinte biztos vagy a kimenetelben). (H közel 0).
*   Ha az érme mindkét oldala fej (100%), az **nulla entrópia** (nincs bizonytalanság).

**2. Információ Nyereség (Information Gain):**
Azt méri, hogy mennyivel *csökkent* a bizonytalanság (entrópia) egy kérdés feltétele után.
*   **Képlet:**
    $$Nyereség(A) = \text{Eredeti Entrópia} - \text{Maradék Entrópia az A szerinti vágás után}$$

**Példa számítás (31. dia) - Ezt nagyon szokták kérdezni!**
Van 12 példánk (6 Igen, 6 Nem). Eredeti entrópia: 1 bit (teljes bizonytalanság).

*   **"Kuncsaft" attribútum tesztelése:**
    *   *Senki* (2 példa): Mindkettő NEM. (Entrópia = 0, tiszta).
    *   *Néhány* (4 példa): Mind a 4 IGEN. (Entrópia = 0, tiszta).
    *   *Tele* (6 példa): 2 Igen, 4 Nem. (Kevert, még van entrópia).
    *   **Eredmény:** A Kuncsaft nagyon jól szétválogatta az adatokat, **nagy a nyereség**.

*   **"Típus" attribútum tesztelése:**
    *   Francia, Thai, Burger, Olasz... mindegyik ágon vegyesen vannak Igenek és Nemek. A bizonytalanság alig csökkent.
    *   **Eredmény:** A Típus **kis nyereséget** ad, nem ezzel kezdünk.

**Konklúzió:** A fát úgy építjük, hogy a gyökérbe tesszük a legnagyobb nyereségű attribútumot (*Kuncsaft*), aztán az ágakon tovább haladva megint keressük a legjobbat a maradékból (*Éhes?*), amíg el nem fogynak a példák.

---

### 5. Túltanulás és Nyesés (Overfitting & Pruning) (40-45. dia)

Ha túl sokáig növesztjük a fát, baj lesz.

**Túltanulás (Overfitting - 40. dia):**
*   A fa annyira bonyolult lesz, hogy megtanulja a "zajt" vagy a véletlen hibákat a tanító adatokban.
*   Olyan, mint amikor a diák bemagolja a példatár megoldásait, de a vizsgán megbukik, mert nem érti az összefüggést.
*   **Tünete:** A tanító adatokon a hiba 0%, de az új adatokon (teszthalmaz) a hiba nagy.

**Megoldás: Nyesés (Pruning):**
Nem engedjük a fát teljesen megnőni, vagy utólag visszavágjuk.

1.  **Korai leállás (Pre-pruning):**
    *   Ha egy elágazásnál az Információ Nyereség túl kicsi (statisztikailag nem szignifikáns, csak véletlennek tűnik a különbség), akkor **nem vágjuk tovább**, hanem megállunk, és többségi szavazással döntünk (pl. "Itt már 60% Igen, legyen Igen").
    *   Ezt hívják **Chi-négyzet ($\chi^2$) tesztnek** (43. dia).

2.  **Utólagos nyesés (Post-pruning) - (44. dia):**
    *   Hagyjuk a fát teljesen megnőni.
    *   Utána alulról felfelé megnézzük: "Ha ezt az alsó elágazást levágnám és egyetlen levéllel helyettesíteném, mennyivel nőne a hiba?"
    *   Ha a hiba alig nő, de a fa sokkal egyszerűbb lesz, akkor levágjuk. (Komplexitás vs. Pontosság kompromisszum).

---

### Összefoglaló a vizsgára (Mit kell tudnod?):

1.  **Döntési fa elemei:** Csomópont (teszt), Él (válasz), Levél (kimenet).
2.  **ID3 algoritmus elve:** Mohó stratégia, mindig a legnagyobb Információ Nyereséget adó attribútumot választjuk.
3.  **Entrópia:** A tisztaság/bizonytalanság mérőszáma. (Csak egyféle példa = 0 entrópia. Fele-fele = 1 entrópia).
4.  **Túltanulás:** Amikor a modell túl bonyolult és a zajt is megtanulja. (Megoldás: Nyesés).
5.  **Nyesés (Pruning):**
    *   $\chi^2$-teszt: Van-e statisztikai értelme a vágásnak? (Ha nincs, stop).
    *   Utólagos nyesés: Ha a vágás egyszerűsít, de nem ront sokat a hibán, akkor vágunk.

Ez a diasor a gépi tanulás egyik legfontosabb, "fehér doboz" (átlátható) modelljét mutatja be. Ha érted, miért a "Kuncsaft"-tal kezdtük a fát és nem a "Típus"-sal (mert jobban szétválasztotta az Igen/Nem-eket), akkor érted a lényeget!