Rendben, vettem a kérést! Ez a diasorozat a **Döntéselmélet (Decision Theory)** gyakorlati alkalmazásáról szól. Korábban (az előző előtti anyagban) átvettük a "Hasznosság" (Utility) elméleti hátterét, most viszont megmutatjuk, hogyan lehet ezt egy hálóba beépíteni, és konkrétan kiszámolni, mit tegyen a robot.

Ez a téma köti össze a valószínűségi hálókat a cselekvéssel. Eddig csak *megfigyeltünk* (következtetés), most már *döntünk* is.

Íme a részletes, vizsga-fókuszú magyarázat:

---

### 1. Döntési Háló (Decision Network) / Befolyási Diagram (Influence Diagram) (3-5. dia) – **KULCSFOGALOM**

Ez a Bayes-hálók "nagytestvére". Nemcsak valószínűségeket tartalmaz, hanem döntéseket és hasznosságokat is.

**A háromféle csomópont (5. dia) – EZT MUSZÁJ TUDNI:**
1.  **Valószínűségi csomópont (Ováis/Kör):** Ugyanaz, mint a Bayes-hálókban. Ezek a bizonytalan események (pl. *Weather* - Időjárás, *Forecast* - Előrejelzés). Van FVT-jük (feltételes valószínűségi táblájuk).
2.  **Döntési csomópont (Téglalap):** Ezeket mi (az ágens) irányítjuk. Itt választanunk kell egy cselekvést (pl. *Umbrella* - Viszünk-e ernyőt?). **Nincs szülőjük**, mert mi döntjük el, mit teszünk (ez nem valószínűségi változó, hanem beavatkozás).
3.  **Hasznosság csomópont (Gyémánt/Rombusz):** Ez mondja meg, mennyire "jó" a végkimenetel.
    *   *Szülei:* A döntéseink és a véletlen események (pl. vittünk-e ernyőt ÉS esik-e az eső).
    *   *Tartalma:* Egy táblázat, ami minden kombinációhoz hozzárendel egy számot (hasznosságot).

---

### 2. A Számítás Menete (MEU) a Hálóban (6-10. dia) – **VIZSGAPÉLDA**

Hogyan dönti el a robot, mit tegyen? A **Maximális Várható Hasznosság (MEU)** elvét követi.

**A példa (Esernyős probléma - 7-9. dia):**
*   **Változók:**
    *   $W$: Időjárás (Sun/Rain). $P(Sun)=0.7, P(Rain)=0.3$.
    *   $A$: Döntés (Take/Leave - Visz/Hagy).
    *   $U$: Hasznosság.
        *   Ha süt a nap és nincs ernyő: 100 (Szuper).
        *   Ha esik és van ernyő: 20 (Elmegy).
        *   Ha esik és nincs ernyő: 0 (Bőrig ázol - Rossz).
        *   Ha süt a nap és van ernyő: 70 (Cipeled feleslegesen).

**A számítás (10. dia):**
1.  **"Mi van, ha OTTHAGYOM az ernyőt?" (Leave):**
    *   $EU(Leave) = P(Sun) \cdot U(Leave, Sun) + P(Rain) \cdot U(Leave, Rain)$
    *   $EU(Leave) = 0.7 \cdot 100 + 0.3 \cdot 0 = 70$.
2.  **"Mi van, ha VISZEM az ernyőt?" (Take):**
    *   $EU(Take) = P(Sun) \cdot U(Take, Sun) + P(Rain) \cdot U(Take, Rain)$
    *   $EU(Take) = 0.7 \cdot 70 + 0.3 \cdot 20 = 49 + 6 = 55$.
3.  **Döntés:** Mivel $70 > 55$, a racionális döntés: **Leave (Nem visz ernyőt).**

**Fontos:** Ha lenne *Előrejelzés (Forecast)* csomópont is (mint a 3. dián), akkor először frissíteni kellene a valószínűségeket az előrejelzés alapján (Bayes-tétel: $P(Weather \mid Forecast)$), és azzal számolni a várható értéket.

---

### 3. Egylépéses vs. Többlépéses Döntés (11-12. dia)

Ez a különbségtétel nagyon fontos a bonyolultabb rendszereknél.

**A) Egylépéses (One-shot) Döntés (11. dia):**
*   Csak egyet lépünk, megkapjuk a jutalmat, és vége. (Mint az esernyős példa).
*   **Képlet:** $MEU(a^*) = \max_{a} \sum_{s} P(s \mid a) \cdot U(s)$
    *   Végignézzük az összes lehetséges $a$ cselekvést.
    *   Minden cselekvéshez kiszámoljuk a várható hasznosságot (a lehetséges $s$ kimenetelek valószínűségével súlyozva).
    *   Azt választjuk, ahol ez a legnagyobb.
*   **Determinisztikus eset:** Ha a cselekvés biztosan egy adott állapothoz vezet, akkor egyszerűen a legnagyobb hasznosságú állapotot választjuk. (Nem kell átlagolni).

**B) Többlépéses (Szekvenciális) Döntés (12. dia):**
*   Itt a döntésünk nemcsak azonnali jutalmat ad, hanem megváltoztatja a jövőt is, és újabb döntési helyzetbe kerülünk. (Pl. Sakk, Robot navigáció).
*   Ez sokkal nehezebb! Miért? Mert nem elég a *pillanatnyi* jót nézni. Lehet, hogy most beáldozok egy gyalogot (kis veszteség), hogy 5 lépés múlva mattot adjak (óriási nyereség).
*   **A hasznosságfüggvény ($U_t(s)$) változik:**
    *   $U(s)$: Az állapot *közvetlen* haszna (pl. ettem egy almát).
    *   $U^{t+1}(s_i)$: A jövőbeli állapotok várható haszna (a hátralévő életem boldogsága).
    *   **Képlet:** $U^t(s) = U(s) + \text{Várható jövőbeli haszon}$.
    *   Ez a **Bellman-egyenlet** alapja (később, a Megerősítéses Tanulásnál lesz kulcsfontosságú). A lényeg: a jelenbeli döntés értékét a *teljes jövőbeli fa* várható értéke adja.

---

### Összefoglaló a vizsgára (Mit kell tudnod?):

1.  **Döntési háló elemei:** Téglalap (Döntés), Ováis (Véletlen), Gyémánt (Hasznosság). Tudd, melyik mire való és kinek lehet szülője. (Döntésnek nincs szülője!).
2.  **MEU számítás:** Tudj végigszámolni egy egyszerű példát (mint az esernyős).
    *   Lépések: 1. Valószínűségek frissítése (ha van megfigyelés). 2. Várható érték számolása minden döntésre ($P \cdot U$). 3. Maximum kiválasztása.
3.  **Egylépéses vs. Többlépéses:**
    *   Egylépéses: Csak a közvetlen kimenetelt nézzük.
    *   Többlépéses: A döntés befolyásolja a jövőbeli döntési helyzeteket is (stratégia kell).

Ha érted az esernyős példa levezetését, akkor a döntési hálók alapjait érted. A következő anyag valószínűleg a szekvenciális döntések (MDP - Markov Decision Process) lesz, ami a többlépéses döntést formalizálja.