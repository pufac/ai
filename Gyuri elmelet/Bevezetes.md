Szia! Örömmel segítek. Ez egy bevezető anyag a Mesterséges Intelligencia (MI/AI) alapjaiba a BME-ről. Nagyon fontos alapozó előadás, mert itt definiálják azokat a fogalmakat (ágens, környezet, racionalitás), amelyekre a félév során építkezni fogtok.

Íme a diasor részletes, vizsga-fókuszú feldolgozása, strukturálva a könnyebb tanulhatóság érdekében.

---

### 1. Mi az az Intelligencia? (Definíciók és Megközelítések)

A legfontosabb elméleti keret az MI definíciójára egy **2x2-es mátrix** (5-8. dia). Ez gyakori vizsgakérdés szokott lenni: hogyan csoportosíthatjuk az MI megközelítéseket?

A mátrix két tengelye: **Gondolkodás vs. Cselekvés** és **Emberi vs. Racionális**.

1.  **Emberi módon gondolkodó (Thinking Humanly):**
    *   **Cél:** Az emberi elme belső működésének másolása.
    *   **Eszköz:** Kognitív modellezés, neurobiológia.
2.  **Emberi módon cselekvő (Acting Humanly):**
    *   **Cél:** Úgy viselkedni, hogy ne lehessen megkülönböztetni az embertől.
    *   **Teszt:** **Turing-teszt** (természetes nyelv, tudásábrázolás, következtetés, tanulás).
3.  **Racionálisan gondolkodó (Thinking Rationally):**
    *   **Cél:** Helyes következtetések levonása logikai alapon.
    *   **Eszköz:** Logika, szillogizmusok (pl. "Minden ember halandó...").
    *   *Probléma:* Nem minden írható le logikával, és a "gondolkodás" önmagában nem elég, ha nem vezet cselekvéshez.
4.  **Racionálisan cselekvő (Acting Rationally) – **Ez a mérnöki megközelítés (a tárgy fókusza)!****
    *   **Cél:** A **„legjobb”** (optimális) eredmény elérése a rendelkezésre álló információk alapján.
    *   **Kulcsszó:** Teljesítmény-maximalizálás.

**⚠️ Fontos kiegészítés (Russell & Norvig könyv alapján):** A modern MI kurzusok szinte mindig a **4. pontra (Racionális Ágens)** fókuszálnak. Azért, mert az emberi viselkedés nem mindig tökéletes (hibázunk), a tiszta logika pedig túl lassú vagy lehetetlen a való világban. A racionalitás itt nem "mindenhatóságot" jelent, hanem azt, hogy a *rendelkezésre álló tudás alapján* a legjobbat lépjük.

---

### 2. Az MI Története és Korszakai (9-11. dia)

Ezt elég nagyvonalakban tudni, de a **három fő korszakot** érdemes megjegyezni:

1.  **Számítás (keresés) alapú MI (1950-70-es évek):**
    *   Logika, sakkprogramok, keresési algoritmusok.
    *   Dartmouth konferencia (1956): Itt született meg az "Artificial Intelligence" kifejezés.
2.  **Tudásalapú MI (1970-90-es évek):**
    *   Szakértői rendszerek. Emberi szabályokat (pl. orvosi diagnózis szabályai) tápláltak a gépbe. "Ha lázas -> akkor..."
3.  **Adatvezérelt / Gépi tanulás alapú MI (1990-től napjainkig):**
    *   Nem mi írjuk a szabályokat, hanem a gép "tanulja meg" őket adatokból.
    *   Ide tartozik a **Deep Learning (Mélytanulás)** és a neurális hálók újbóli felemelkedése.

**Fogalmi hierarchia (11. dia):**
*   **AI (MI):** A nagy ernyőfogalom.
*   **ML (Machine Learning):** Az AI azon része, ami tanul (nem csak fix programot hajt végre).
*   **DL (Deep Learning):** Az ML azon része, ami mély neurális hálókat használ.

---

### 3. Az MI Típusai (Erősség szerint) (12-13. dia)

*   **Gyenge/Szűk MI (Narrow AI):** Egyetlen speciális feladatra jó (pl. sakkozó gép, arcfelismerő, ChatGPT). *Ma itt tartunk.*
*   **Erős/Általános MI (AGI - Artificial General Intelligence):** Bármilyen szellemi feladatot képes megoldani, amit egy ember (tanul, alkalmazkodik új területeken). *Ez még sci-fi / jövőkutatás.*
*   **Szuperintelligencia:** Mindenben meghaladja az embert.

---

### 4. Az Ágens (Agent) Koncepció – **VIZSGA KULCSANYAG** (21-30. dia)

Ez a mérnöki megközelítés alapja. Meg kell értened a felépítését és a matematikai absztrakciót.

**Az Ágens felépítése:**
1.  **Szenzorok (Érzékelés):** Információt gyűjt a környezetből (pl. kamera, mikrofon).
2.  **Feldolgozás (A "doboz" belül):** A belső állapot és a szabályok/tanulás alapján döntést hoz.
    *   Matematikailag: egy $f$ függvény, ami az érzékelések történetét ($P^*$) leképezi egy cselekvésre ($A$). $f: P^* \rightarrow A$.
3.  **Beavatkozók (Cselekvés):** Hatást gyakorol a környezetre (pl. motor, hangszóró, képernyő).

**A Környezet (Environment):**
Minden, ami az ágens hatókörén kívül van, de interakcióba lép vele.

**Állapottér és Trajektória (Matematikai modell):**
*   $S_K$: A környezet lehetséges állapotai.
*   $S_A$: Az ágens belső állapotai.
*   **Trajektória:** Az az út, amit az ágens bejár az állapottérben a **Kezdőállapotból** ($S_{kezdeti}$) a **Célállapotba** ($S_{cél}$).
*   A cél az, hogy olyan cselekvéseket válasszunk, amelyekkel a trajektória eljut a célállapotba (pl. a robot eljut a töltőhöz).

---

### 5. Racionalitás vs. Korlátozott Racionalitás (31. dia)

*   **Tökéletes Racionalitás:** Mindig azt lépem, ami maximalizálja a várható hasznot. A valóságban ez sokszor lehetetlen, mert:
    *   Nincs elég idő kiszámolni az összes lehetőséget (pl. sakkban minden lépést előre).
    *   Nincs elég információnk.
*   **Korlátozott Racionalitás (Bounded Rationality):** Megpróbálunk "elég jól" cselekedni a rendelkezésre álló idő és erőforrás korlátai között.

---

### 6. A Környezet Típusai (33-34. dia) – **NAGYON FONTOS**

Ez szinte biztosan előkerül vizsgán. A környezet tulajdonságai határozzák meg, milyen nehéz dolga van az MI-nek.

A környezet dimenziói:

1.  **Hozzáférhető (Observable) vs. Nem hozzáférhető:**
    *   *Teljesen hozzáférhető:* Látom az egész pályát (pl. sakk, "kiterített kártyák").
    *   *Nem hozzáférhető (Részlegesen):* Nem látok mindent (pl. póker, önvezető autó ködben, vagy nem lát a fal mögé). -> **Ez a nehezebb.**
2.  **Determinisztikus vs. Sztochasztikus (Nem determinisztikus):**
    *   *Determinisztikus:* Ha megnyomom a gombot, biztosan megtörténik az esemény (pl. számológép). A következő állapotot csak a jelenlegi állapot és a cselekvésem határozza meg.
    *   *Sztochasztikus:* Van véletlen faktor. Ha rúgok egyet a labdába, a szél miatt nem biztos, hogy oda megy, ahova szántam. -> **Ez a nehezebb.**
3.  **Epizódszerű vs. Sorozatos (Sequential):**
    *   *Epizódszerű:* A mostani döntésem nem befolyásolja a jövőt (pl. egy képfelismerő program: felismeri a képet, kész, jöhet a következő. Nem számít mit ismert fel előtte).
    *   *Sorozatos:* A mostani lépésem befolyásolja a jövőbeli lehetőségeimet (pl. sakk, autóvezetés). -> **Ez a nehezebb.**
4.  **Statikus vs. Dinamikus:**
    *   *Statikus:* A világ nem változik, amíg én gondolkodom (pl. sakk, keresztrejtvény).
    *   *Dinamikus:* A világ változik, miközben gondolkodom (pl. önvezető autó, valós idejű stratégiai játék). -> **Ez a nehezebb.**
5.  **Diszkrét vs. Folytonos:**
    *   *Diszkrét:* Véges számú állapot és lépés van (pl. sakkmezők).
    *   *Folytonos:* Végtelen sok állapot (pl. kormány elfordítása 32.4 fokkal, sebesség). -> **Ez a nehezebb.**
6.  **Egy ágens vs. Több ágens:**
    *   Van-e más intelligens szereplő? Ha igen, ők lehetnek *kooperatívak* (segítenek) vagy *versengők* (ellenfelek).

**Összegzés (A legnehezebb eset):**
Ha a környezet **nem hozzáférhető, nem determinisztikus, sorozatos, dinamikus, folytonos és több ágens van**. (Ez lényegében a való világ, pl. autót vezetni Budapesten).

**Az Ágens ellenségei (korlátai):**
1.  Véges erőforrás (idő, memória).
2.  Információhiány (nem látunk mindent).
3.  Változó környezet.

---

### Összefoglaló a vizsgára (Mit vigyél haza ebből a PDF-ből?):

1.  Tudd a **4 definíciót** (Emberi/Racionális x Gondolkodás/Cselekvés) és hogy mi a **Racionális Ágens**.
2.  Értsd az **Ágens felépítését**: Szenzor -> [f: P*->A] -> Beavatkozó.
3.  Tudd felsorolni és értelmezni a **Környezet tulajdonságait** (pl. mi a különbség determinisztikus és sztochasztikus között).
4.  Tudd, mi a különbség **Gyenge és Erős MI** között.

Ez lefedi a PDF teljes lényegi tartalmát. Ha bármelyik pont nem tiszta, kérdezz rá nyugodtan! Küldheted a következő PDF-et is.