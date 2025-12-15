Szia! Ez egy újabb nagyon fontos és jól elkülöníthető témakör: a **Kényszerkielégítési Problémák (CSP - Constraint Satisfaction Problems)**.

Míg az előző anyagokban (A*, keresés) az **útvonal** volt a lényeg (hogyan jutok el A-ból B-be a legolcsóbban), itt a **célállapot** a lényeg (hogy nézzen ki a megoldás, függetlenül attól, milyen sorrendben raktam össze).

Itt a strukturált, vizsgafókuszú összefoglaló:

---

### 1. Mi a különbség a sima keresés és a CSP között? (3. dia)

Ez alapvető koncepcionális kérdés.
*   **Hagyományos keresés (pl. útvonaltervezés):** A célhoz vezető *útvonal* és annak költsége számít. A világ egy „fekete doboz”, csak lépkedünk benne.
*   **Kényszerkielégítési probléma (CSP):** Nem érdekel az útvonal! Csak a **végső állapot** érdekel. A cél az, hogy találjunk egy olyan állapotot, ami megfelel minden szabálynak (kényszernek).
    *   *Példa:* Sudoku. Nem érdekel, milyen sorrendben írtad be a számokat, csak az, hogy a végén minden szabály stimmeljen.

---

### 2. A CSP Definíciója: $\{X, D, C\}$ (4-5. dia) – **VIZSGA KULCSFOGALOM**

Minden CSP problémát három komponenssel írunk le. Ezt a jelölést tudnod kell:

1.  **$X$ (Variables) – Változók:** $\{X_1, X_2, \dots, X_n\}$.
    *   Ezek azok a dolgok, amiknek értéket kell adnunk. (Pl. Ausztrália államai: WA, NT, Q...).
2.  **$D$ (Domains) – Tartományok:** $\{D_1, D_2, \dots, D_n\}$.
    *   Azoknak az értékeknek a halmaza, amit egy változó felvehet. (Pl. színek: {piros, zöld, kék}). Minden változónak lehet saját tartománya.
3.  **$C$ (Constraints) – Kényszerek:**
    *   Szabályok, amik korlátozzák, hogy a változók milyen értékeket vehetnek fel *egymáshoz képest*. (Pl. WA $\neq$ NT, azaz két szomszéd nem lehet azonos színű).

**Megoldás:** Egy olyan állapot, ahol **minden** változóhoz rendeltünk egy értéket, és **minden** kényszer teljesül.

---

### 3. Példák CSP-re (6-11. dia)

Ezeket a példákat szokták használni a fogalmak illusztrálására:
*   **Térképszínezés:** (Klasszikus példa). Szomszédos országok színe nem lehet egyforma.
*   **N-királynő probléma:** $N$ darab királynőt kell felrakni a sakktáblára úgy, hogy ne üssék egymást. (Kényszer: nem lehetnek egy sorban, oszlopban, átlóban).
*   **Betűrejtvény (Cryptarithmetic):** TWO + TWO = FOUR. Minden betű egy számjegy. (Kényszer: M $\neq$ O, és a matematikai összeadásnak stimmelnie kell).
*   **Sudoku:** A legismertebb CSP.

---

### 4. A CSP Típusai (12-13. dia)

Csoportosíthatjuk a problémákat a változók és a kényszerek típusa szerint.

**A) Változók szerint:**
*   **Diszkrét (Véges):** Pl. logikai változók, színek. (Ezekkel foglalkozunk leginkább). *Megjegyzés: A Boole-féle kielégíthetőség (SAT) NP-teljes probléma (nagyon nehéz).*
*   **Diszkrét (Végtelen):** Pl. egész számok.
*   **Folytonos:** Pl. időpontok, fizikai paraméterek.

**B) Kényszerek szerint (Fontos!):**
1.  **Unáris (Unary):** Egyetlen változóra vonatkozik.
    *   Pl.: `SA ≠ zöld` (Dél-Ausztrália nem lehet zöld). Ezeket könnyű kezelni, egyszerűen kivesszük az értéket a tartományból.
2.  **Bináris (Binary):** Két változó viszonyát szabályozza.
    *   Pl.: `SA ≠ WA` (Dél-Ausztrália nem lehet olyan színű, mint Nyugat-Ausztrália). A legtöbb algoritmus ezekre van optimalizálva.
3.  **Magasabb rendű:** 3 vagy több változót érint. (Pl. Sudokuban 9 számjegynek kell különbözőnek lennie).

**Kényszergráf (14. dia):** A bináris CSP-k ábrázolása. A csomópontok a változók, az élek a kényszerek. Ez segít látni a probléma struktúráját.

---

### 5. Hogyan oldjuk meg? – Kereséssel (17-20. dia)

Mivel diszkrét problémákról beszélünk, használhatunk keresőfát. De van egy trükk!

**A Naiv próbálkozás (Sima mélységi keresés):**
*   Próbáljuk meg az összes variációt.
*   Ha van $n$ változó és $d$ lehetséges érték: A fa mélysége $n$, minden szinten $n, n-1, \dots$ elágazás.
*   Ez borzasztó lassú ($n! \cdot d^n$).

**A Trükk: Kommutativitás (19. dia):**
*   Rájövünk, hogy a **sorrend nem számít**. Mindegy, hogy először WA-t színezzük pirosra, aztán NT-t zöldre, VAGY fordítva. Az eredmény ugyanaz az állás.
*   Ezért minden szinten rögzítjük: **"Most csak a következő változóval foglalkozunk"**.
*   Ezzel a keresési fa mérete drasztikusan csökken ($d^n$-re).

---

### 6. Visszalépéses Keresés (Backtracking Search) (20-25. dia) – **A LEGFONTOSABB ALGORITMUS**

Ez a CSP megoldások alapköve. Ez tulajdonképpen egy **mélységi keresés (DFS)**, két speciális tulajdonsággal:
1.  Egyszerre csak egy változóhoz rendel értéket.
2.  **Azonnal ellenőrzi a kényszereket.**

**Működése:**
1.  Kiválasztunk egy még üres változót.
2.  Kiválasztunk neki egy értéket a tartományából.
3.  **Ellenőrzés:** Ha az érték ütközik valamelyik kényszerrel a már lerakott változók közül, akkor **azonnal eldobjuk** és jöhet a következő érték. (Nem megyünk mélyebbre hibás ágon).
4.  Ha nincs jó érték, **visszalépünk (backtrack)** az előző változóhoz, és ott próbálunk mást.

**Pszeudókód lényege (25. dia):**
```
function VISSZALÉPÉSES-KERESÉS(csp)
   if minden változó kész: return megoldás
   var <- VÁLTOZÓ-KIVÁLASZTÁSA(csp)
   for each érték in TARTOMÁNY-RENDEZÉSE(var)
      if érték konzisztens (nem sért szabályt):
         hozzáad(var = érték)
         eredmény <- VISSZALÉPÉSES-KERESÉS(csp)
         if eredmény != hiba: return eredmény
         eltávolít(var = érték) // Visszavonás, ha zsákutca volt
   return hiba
```

---

### 7. Hogyan tegyük gyorsabbá? (26-27. dia)

A sima visszalépéses keresés még mindig lehet lassú (lásd 26. dia példáját, ahol sokat dolgozik feleslegesen, mielőtt észrevenné a hibát).
Három módszerrel lehet okosítani (ezek lesznek valószínűleg a következő előadás témái, itt csak felsorolás van):

1.  **Szűrés (Filtering):** Előre kizárni azokat az értékeket, amik biztosan rosszak lesznek (pl. ha WA piros, akkor NT-ből a pirosat már most kihúzhatjuk).
2.  **Sorrendezés (Ordering):**
    *   *Melyik változót válasszuk?* (Pl. azt, aminek már csak 1 színe maradt – legszűkebb tartomány).
    *   *Melyik értéket válasszuk?* (Pl. azt, ami a legkevesebb szomszédot zavarja).
3.  **Struktúra:** A probléma gráfjának elemzése (pl. szétvágható-e a probléma kisebb, független részekre?).

---

### Összefoglaló a vizsgára:

1.  **Definíció:** Tudd felírni a CSP-t formálisan: $\{X, D, C\}$.
2.  **Kényszer típusok:** Tudd a különbséget Unáris és Bináris között.
3.  **Backtracking:** Értsd meg, hogy ez egy mélységi keresés, ami kihasználja a kommutativitást (nem permutálja a változókat) és vágást alkalmaz (ha sérül a szabály, azonnal visszalép).
4.  **Keresési fa mérete:** Értsd meg, miért jobb a CSP keresési fa ($d^n$) a naiv fanál ($n! \cdot d^n$).

Ez a diasor az alapozás volt, a következőben valószínűleg a "tuningolásról" (heurisztikák, előretekintés) lesz szó részletesen. Küldheted a következőt, ha van!