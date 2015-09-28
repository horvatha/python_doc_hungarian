.. _tut-intro:

***************
Étvágygerjesztő
***************

Ha valaha is írtál hosszú shell szkriptet, feltehetően ismered azt az  érzést,
hogy amikor új alaptulajdonságot szeretnél hozzáadni, a program lassúvá és
bonyolulttá válik;  vagy az új tulajdonság magában foglal egy rendszerhívást
vagy más függvényt, amely csak C programból érhető el... Gyakran a probléma nem
olyan komoly, hogy a C-ben történő újraírást indokolná; talán a programnak
szüksége van változó hosszúságú karakterláncokra vagy más adattípusra (mint
amilyen a fájlnevek rendezett listája), melyet könnyű létrehozni a shell-ben, de
rengeteg munka C-ben, vagy talán nem ismered eléggé a C nyelvet.

Más helyzet: talán több különböző C könyvtárakkal kell dolgoznod, és a gyakori
írás/fordítás/tesztelés/újrafordítás ciklus túl lassú. Sokkal gyorsabban kellene
szoftvert fejlesztened. Talán írtál egy programot amely képes kiegészítő nyelv
használatára -- és te nem akarsz csak emiatt tervezni egy nyelvet, írni és
javítgatni egy fordítót (interpretert).

Ha valamelyik állítás igaz rád, a Python megfelelő nyelv lehet számodra. A
Pythont egyszerű kezelni, mégis igazi programnyelv, sokkal több szerkezetet
használ és több támogatást nyújt nagyméretű  programok számára, mint a shell.
Ezzel egyidőben sokkal több hibaellenőrzési lehetőséget  tartalmaz mint a C, és
-- lévén *nagyon magas szintű nyelv* -- magas szintű beépített adattípusai vannak,
úgymint rugalmasan méretezhető sorozatok és szótárak, amelyeket C-ben létrehozni
napokba tellene. Az általánosabban megfogalmazott adattípusaival a Python jóval
nagyobb problématerületen alkalmazható mint az *Awk* vagy akár a *Perl*,
ugyanakkor sok dolog legalább ugyanolyan könnyű Pythonban, mint ezekben a
nyelvekben.

A Python lehetővé teszi, hogy a programodat modulokra oszd fel, amelyek
felhasználhatók más Python programokban is.   A nyelvhez tartozó alap-könyvtár
alaposan kidolgozott modulgyűjteményt tartalmaz, melyeket a programod alapjául
használhatsz -- vagy példáknak a Python tanulásához. Vannak beépített modulok is
mint a fájl I/O, rendszerhívások, socket-ek kezelése és interfészek olyan
grafikus felülethez, mint a Tk.

A Python futási idő alatt értelmezett (interpretált) nyelv,  amely időt takarít
meg neked  a programfejlesztés alatt, mivel nincs szükség gépi kódra történő
fordításra és a gépi kódok összeszerkesztésére.   Az értelmező interaktívan is
használható,  lehet kísérletezni a nyelv tulajdonságaival, vagy  függvényeket
tesztelni az alulról felfelé történő programfejlesztés során. Egyben egy ügyes
asztali számológép is!

A Python nagyon tömör és olvasható programok írását teszi lehetővé. A Pythonban
írt programok általában sokkal rövidebbek mint a C vagy C++ megfelelőjük, mert:

* a magasszintű adattípusok lehetővé teszik egyetlen utasításban  egy összetett
  művelet kifejtését;

* az utasítások csoportosítása a sorok elejének egyenlő mértékű jobbra tolásával
  történik a kezdő és végzárójelek helyett;

* nem szükséges a változók és argumentumok deklarálása.

A Python *bővíthető*: ha tudsz C-ben programozni, akkor könnyű új beépített
függvényt vagy modult hozzáadni az értelmezőhöz, vagy azért, hogy a
kritikus eljárások a lehető leggyorsabban fussanak, vagy például olyan
könyvtárakra linkelni Pythonból, amelyek csak bináris formában érhetők el
(amilyenek a forgalmazóspecifikus grafikai programok).  Hogyha a nyelv valóban
mélyen megfogott, akkor a Python értelmezőt hozzákötheted egy C-ben írt
alkalmazáshoz, és azt a program kiterjesztéseként vagy parancs-nyelvként
használhatod.

A nyelv a ,,Monthy Python-ék Repülő Cirkusza" nevű BBC-s sóműsor után kapta a
nevét és semmi köze nincs a nyálas hüllőhöz... Utalásokat tenni a
dokumentációban a Monty Pythonra nemcsak szabad, hanem ajánlott!

Most, hogy már felkeltette az érdeklődésedet a Python, reméljük szeretnéd
megtekinteni valamivel részletesebben.  Mivel a nyelvtanulás legjobb módja annak
használata,  meghívunk Téged egy kis gyakorlásra.

A következő fejezetben az értelmező használatának minkéntjét magyarázzuk. Ezek
kissé unalmas dolgok, de szükségesek ahhoz, hogy a későbbiekeben mutatott
példákat kipróbálhasd.

Az oktató többi része példákon keresztül mutatja be a Python nyelv és a rendszer
sok-sok tulajdonságát, kezdve az egyszerű kifejezésekkel, utasításokkal és
adattípusokkal -- folytatva a függvényekkel és a modulokkal. Végül érinti a
legújabb programozási módszereket, mint például a kivételkezelés, és a
felhasználó által definiált osztályok.

