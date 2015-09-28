.. _tut-classes:

*********
Osztályok
*********

A Python osztálymechanizmusa a nyelvhez a lehető legkevesebb
szintaxissal és szemantikával ad hozzá osztályokat. A Python osztályok a
C++--ban és a Modula-3--ban található  osztálymechanizmusok keveréke.
A Python osztályok rendelkeznek az objektumorientáltság minden
jellemzőjével: az öröklődési mechanizmus lehetővé teszi a több őstől
való származtatást;  a származtatott osztályok a szülők bármely
metódusát felül tudják írni; a metódusok ugyanazon a néven érhetik el a
szülőosztály metódusait.  Az objektumok tetszőleges számú és típusú
adatot tartalmazhatnak.

C++ szóhasználattal élve az osztály minden eleme (beleértve az adatokat
is) *publikus* (a kívételt lásd lejjebb :ref:`tut-private`), és minden
tagfüggvény *virtuális*. Nincsenek konstruktor és destruktor függvények.
A Modula-3--hoz hasonlóan nincsen rövidített hivatkozás az objektum
alkotóelemeire annak metódusaiból: az objektum függvényeinek
deklarálásakor első argumentumként az objektumot jelképező változót adjuk
meg, mely híváskor automatikusan átadásra kerül.
A Smalltalk-hoz hasonlóan az osztályok önmaguk is objektumok. Ez teremt
szemantikát az importáláshoz és átnevezéshez. 
A C++--tól és a Modula-3-tól eltérően a beépített típusok  szülőosztályokként
felhasználhatók. S végül a C++-hoz hasonlóan legtöbb, egyéni
szintaktikával bíró  beépített operátor (aritmetikai műveletek,
indexelés stb.) újradefiniálhatók az osztály példányaiban.

Nem lévén általánosan elfogadott szóhasználat az osztályok témakörére,
alkalmanként Smalltalk és C++ kifejezéseket fogok használni. (Szerettem volna
Modula-3 kifejezéseket alkalmazni, mert annak hasonlít leginkább az objektum-
orientáció értelmezése a Pythonhoz,  de sajnos nagyon kevesen ismerik ezt a
nyelvet.)

.. _tut-object:

Pár szó a nevekről és objektumokról
====================================

Az objektumoknak egyéni jellege van, és több nevet lehet kapcsolni
ugyanahhoz az objektumhoz (különböző névterekben). Ez a lehetőség
fedőnévként (aliasing) ismert más nyelvekben. Ezt a lehetőséget a
nyelvvel való első találkozáskor rendszerint nem becsülik meg --
egyébként a fedőnév használata nyugodtan mellőzhető megváltoztathatatlan
típusok használatakor (például számok, karakterláncok, tuple-ok
esetében).  Valójában a fedőnév használata valószínűleg meglepő módon
viselkedik megváltoztatható típusok esetében,  mint például a listák,
szótárak -- és a legtöbb más típus esetén.  A fedőnevek általában a
program hasznára válnak, mivel a mutatókhoz hasonlítanak néhány
vonatkozásban. Például egy objektum átadása kevés
erőforrásfelhasználással jár, mivel csak egy mutató fogmutatni az
objektumra. Ha a meghívott függvény módosítja a neki átadott objektumot,
a hívó látja a változást -- ez szükségtelenné teszi két különböző
argumentumátadási módszer használatát (mint például a Pascal-ban).

.. _tut-scopes-and-namespaces:

Hatókörök és névterek a Pythonban
=================================

Mielőtt megismerkednénk az osztályokkal, beszélnünk kell a hatókörökről. Az
osztálydefiníciók  néhány ügyes trükköt adnak elő a névterekkel, és
ismerned kell a névterek és hatókörök működését ahhoz, hogy teljesen
átlásd, mi is történik. Egyébként ennek a témakörnek az ismerete minden
haladó Python programozónak a hasznára válik.

Kezdetnek nézzünk meg néhány definíciót.

A *névtér* a nevekhez objektumokat rendel. A legtöbb névtér jelenleg Python
szótárakként van megvalósítva, de ez (a teljesítmény kivételével) normális esetben
nem észlelhető -- a jövőben ez egyébként változhat. Példák a névterekre: a
foglalt nevek listája (például az :func:`abs` függvény, vagy a beépített
kivételek nevei); a modulokban jelenlévő globális nevek; vagy a helyi nevek
függvényhívások során. Bizonyos értelemben egy objektum jellemzői is külön
névteret alkotnak. Fontos tudni, hogy különböző névterekben lévő két név között
semmilyen kapcsolat nem létezik.  Például ha két különböző modul egyaránt
definiálhat egy "maximum_beallitas" nevű függvényt bármilyen következmény
nélkül, mert a modulok használóinak a függvénynév előtagjában a modul nevével
egyértelműen jelezniük kell, hogy pontosan melyik függvényt fogják használni.

Apropó --- a *jellemző* jelzőt használom bármilyen névre, ami a pontot követi
--- például a ``z.real``  kifejezésben a ``real`` a ``z`` objektum egyik
jellemzője.

Az igazat megvallva egy modulbeli névre való hivatkozás  egy
jellemző-hivatkozás: a ``modname.funcname`` kifejezésben a ``modname``
egy modul objektum és a  ``funcname`` annak egy jellemzője.  Ebben az
esetben egyenes hozzárendelés történik a modul jellemzői és a
modulban definiált globális nevek között: ezek egy névtéren osztoznak.
[#]_

A jellemzők lehetnek csak olvashatóak, vagy írhatóak is. Az utóbbi esetben
értéket rendelhetünk a jellemzőhöz --  például így: ``modname.the_answer =
42``. Az írható jellemzők a :keyword:`del` utasítással törölhetők is,
például a ``del modname.the_answer`` törli a  a ``modname`` objektum
:attr:`the_answer` jellemzőjét.

A névterek különböző időpontokban születnek, és élettartamuk is változó.
Tartalmazzák a Python értelmező beépített neveit, melyek nem törölhetők. A
modulok részére a globális névtér a  modul definíció olvasásakor jön létre --
általános esetben a  modul névterek az értelmezőből való kilépésig megmaradnak.
Az utasításokat, amelyet az értelmező felső szintje futtat le,
vagy egy szkriptfájlból kiolvasva, vagy interaktív módon, azokat
:mod:`__main__` modul részének tekinti a Python, ezért saját névtérrel
rendelkeznek. (A beépített nevek szintén a modulban léteznek,
:mod:`builtin` név alatt.).

A függvények helyi névtere a függvény hívásakor keletkezik,  és a függvény
lefutásakor, vagy le nem kezelt kivételek létrejöttekor szűnnek meg. (Talán a
felejtés szó pontosabb jelző lenne a törlés helyett.) Természetesen a rekurzív
hívások mindegyike saját, helyi névtérrel rendelkezik.

A *hatókör (scope)* a Python kód azon szöveges része ahol a névtér közvetlenül
elérhető. A közvetlen elérhetőség itt azt  jelenti, hogy a név a teljes elérési
útjának kifejtése nélkül elérhető a névtérben. (például a ``z.real``-ben a .
jelzi, hogy a  ``z`` objektumhoz tartozó jellemzőről van szó, ez itt most
teljesen kifejtett.)

Ámbár a névterek meghatározása statikus, dinamikusan használjuk őket. Bármikor a
program futása során legalább három közvetlenül elérhető névtér létezik: a belső
névtér, amiben az értelmező először keres, és helyi neveket  valamint függvények
neveit tartalmazza (a függvényeket mindig a legközelebbi zárt névtérben keresi
az értelmező) --- a másodszor vizsgált középső névtér, ami az aktuális modul
globális változóinak neveit tartalmazza --- valamint az utoljára vizsgált külső
névtér, ami a beépített neveket tárolja.

Ha egy változónév globálisan deklarált, minden hivatkozás és művelet közvetlenül
a középső névtérben keres, mert ott találhatók a modul globális nevei. Fontos
tudni, hogy a belső névtéren kívül található nevek csak olvashatóak.

Rendszerint a helyi névtér a szövegkörnyezetben található helyi változókra
hivatkozik az aktuális függvényben. A függvényeken kívül a helyi névtér a
globális névtérhez hasonlóan egyben az aktuális modul névtere is. Az
osztálydefiníciók pedig újabb névtereket helyeznek el a helyi névtérben.

Tudatosítani kell, hogy a névterek a szövegkörnyezet által meghatározottak: a
modulban definiált függvény globális névtere a modul névterében jön létre -- a
függvény nevei kizárólag itt elérhetők.

Másrészről az aktuális nevek keresése még dinamikusan, futásidőben történik, --
a nyelvi definíció akármennyire is törekszik a fordításkori, statikus
névfeloldásra, szóval hosszútávon ne számíts a dinamikus névfeloldásra! (Igazság
szerint a helyi változók már statikusan meghatározottak)

A Python egy különleges tulajdonsága, hogy a hozzárendelés mindig  a belső
névtérben történik. A hozzárendelés nem másol adatokat --- csak kötést hoz létre
a nevek és az objektumok között. A törlésre ugyanez igaz: a ``del x`` utasítás
eltávolítja az ``x`` kötését a helyi névtér nyilvántartásából,

Valójában minden művelet, ami új név használatát vezeti be, a helyi névteret
használja -- például utasítások importálása, és függvénydefiníciók modulbeli
létrehozása vagy kötése.

A :keyword:`global` kulcsszóval jelezheted hogy bizonyos  változók a
globális névtérben léteznek és itt újra kell csatolni, a
:keyword:`nonlocal` kulcsszó  azt jelzi, hogy a szóban forgó változó a
helyi hatókörhöz tartozik, és ott kell újracsatolni.

.. xxx

.. _tut-scopeexample:

Hatókör és névtér példa
-----------------------------

Itt van egy példa, amely bemutatja, hogyan lehet hivatkozni a különböző
hatókörökre és névterekre, és hogyan hat a 
:keyword:`global` és :keyword:`nonlocal` a változócsatolásokra::

   def scope_test():
       def do_local():
           spam = "local spam"
       def do_nonlocal():
           nonlocal spam
           spam = "nonlocal spam"
       def do_global():
           global spam
           spam = "global spam"
       spam = "test spam"
       do_local()
       print("After local assignment:", spam)
       do_nonlocal()
       print("After nonlocal assignment:", spam)
       do_global()
       print("After global assignment:", spam)

   scope_test()
   print("In global scope:", spam)

A példakód kimenete::

.. code-block:: none

   After local assignment: test spam
   After nonlocal assignment: nonlocal spam
   After global assignment: nonlocal spam
   In global scope: global spam

Jegyezd meg, hogy a *helyi* értékadás (amely az alapértelmezett) nem
változtatja meg a *scope_test* függvény  *spam* csatolását. A
:keyword:`nonlocal` értékadás a *scope_test* *spam* nevének csatolását
változtatja meg, a :keyword:`global`  értékadás pedig a modulszintű
csatolást.

Azt is érdemes észrevenni, hogy nem csatoltunk értéket a *spam*-hez a 
:keyword:`global` értékadás előtt.


.. _tut-firstclasses:

Első találkozás az osztályokkal
===============================

Az osztályok használatához szükségünk van új szintaxisra: három új
objektumtípusra, és némi új szemantikára.


.. _tut-classdefinition:

Az osztálydefiníció szinaxisa
-----------------------------

A legegyszerűbb osztálydefiníció így néz ki::

   class OsztalyNev:
       <utasitas-1>
       .
       .
       .
       <utasitas-N>

Az osztálydefiníciók hasonlítanak a függvények definíciójára  (:keyword:`def`
statements) abból a szempontból, hogy az osztály  deklarációjának meg kell
előznie az első használatot. (Osztálydefiníciót elhelyzehetsz egy :keyword:`if`
utasítás valamely ágában is, vagy egy függvénybe beágyazva.)

A gyakorlatban az osztályokon belüli utasítások többsége általában
függvénydefiníció, de bármilyen más utasítás is megengedett, és néha
hasznos is -- erre még később visszatérünk.  Az osztályon belüli
függvényeknek normál esetben egyedi argumentumlistájuk (és  hívási
módjuk) van az osztály metódusainak hívására vontakozó megállapodás
szerint --  ezt szintén később fogjuk megvizsgálni.

Egy osztálydefinícióba való belépéskor új névtér jön létre és válik a
helyi hatókörré -- ebből kifolyólag minden helyi változóra történő
hivatkozás ebbe az új névtérbe kerül.  A gyakorlatban általában az új
függvények csatolásai kerülnek ide.

Az osztálydefiníciókból való normális kilépéskor (amikor elérünk a
végéhez) egy *osztályobjektum* jön lére. Ez lényegében egybefoglalja,
beburkolja az osztálydefiníciókor létrejött új névtér tartalmát -- az
osztályobjektumokról a következő alfejezetben fogunk többet tanulni. Az
eredeti helyi névtér (az osztálydefinícióba való belépés előtti
állapotában) helyreállítódik, és az osztályobjektum neve is a helyi
névtér része lesz (az :class:`OsztalyNev` a példában).

.. _tut-classobjects:

Osztályobjektumok
-----------------

Az osztályobjektumok a műveletek kétféle típusát támogatják: a
jellemzőhivatkozást és a példányosítás. A *jellemzőhivatkozások* az
általánosan használt Python jelölésmódot használják:
``objektum.jellemzőnév``.  Az összes név érvényes jellemzőnév ami az
osztály névterében volt  az osztályobjektum létrehozásakor.  Ha egy
osztály definíció valahogy így néz ki::

   class Osztalyom:
       "Egy egyszerű példa osztály"
       i = 12345
       def f(self):
           return 'hello világ'

akkor ``Osztalyom.i`` és ``Osztályom.f`` egyaránt érvényes
jellemzőhivatkozás -- egy egész számmal illetve egy függvényobjektummal
térnek vissza. Az osztályjellemzőknek ugyanúgy adhatunk értéket mint egy
normális változónak (``Osztalyom.i = 2``). A :attr:`__doc__` metódus is
érvényes attribútum, ami az osztály dokumentációs karakterláncával tér
vissza: ``"Egy egyszerű példa osztály"``

Egy osztály *példányosítása* a függvények jelölésmódját használja.
Egyszerűen úgy kell tenni, mintha az osztályobjektum egy argumentum
nélküli függvény lenne, amit meghívva az osztály egy új példányát kapjuk
visszatérési értékként. Például (a fenti osztályt alapul véve)::

   x = Osztalyom()

létrehoz egy új *példányt* az osztályból, és hozzárendeli a visszatérési
értékként kapott objektumot az ``x`` helyi változóhoz.

A példányosítás művelete (az objektum ,,hívása'') egy üres objektumot
hoz létre.  Elképzelhető, hogy az új példányt egy ismert kezdeti
állapotba állítva szeretnénk létrehozni. Ezt egy különleges metódussal,
az :meth:`__init__`-el tudjuk elérni::

   def __init__(self):
       self.data = []

Ha az osztály definiálja az :meth:`__init__` metódust, egy új egyed
létrehozásakor annak :meth:`__init__` metódusa automatikusan lefut. Lássunk egy
példát egy új, inicializált  egyedre::

   x = Osztalyom()

Természetesen az :meth:`__init__` metódusnak argumentumokat is
átadhatunk a nagyobb rugalmasság kedvéért. Az argumentumok az osztály
példányosítása során az  inicializáló metódushoz jutnak.  Lássunk egy
példát::

   >>> class Complex:
   ...     def __init__(self, realpart, imagpart):
   ...         self.r = realpart
   ...         self.i = imagpart
   ... 
   >>> x = Complex(3.0, -4.5)
   >>> x.r, x.i
   (3.0, -4.5)

.. _tut-instanceobjects:

A létrehozott egyedek
---------------------

És most mihez tudunk kezdeni a példányobjektumokkal? A példányobjektumok
csak a jellemzőhivatkozás műveletet ismerik. Két lehetséges  jellemzőnév
van: adatjellemzők és metódusok.

A *adatjellemzők* fogalma megegyezik  a Smalltalk
,,példányváltozók'' fogalmával, és a C++ ,,adattagok'' fogalmával. Az
adatjellemzőket nem kell a használatuk előtt deklarálni, a helyi
változókhoz hasonlatosan működnek -- az első használatukkor
automatikusan létrejönnek.  Például ha ``x`` az :class:`Osztalyom`  egy
példánya, a következő kódrészlet 16-ot fog kiírni::

   x.szamlalo = 1
   while x.szamlalo < 10:
       x.szamlalo = x.szamlalo * 2
   print(x.szamlalo)
   del x.szamlalo

A másik fajta jellemző a metódus (más néven tagfüggvény).
A metódus egy objektumhoz ,,tartozó'' függvényt jelöl.  (A
Pythonban a metódus kifejezés nem kizárólag egy osztály példányának
metódusát jelenti -- más objektum típusok is rendelkezhetnek
metódusokkal. Például a listaobjektumoknak vannak saját metódusai:
append, insert, remove, sort, és így tovább. Az alábbi sorokban a
metódus kifejezést kizárólag egy osztály metódusaira értjük, hacsak
nincs külön kihangsúlyozva, hogy most egy másik objektum metódusáról van
szó.

.. index:: object: method

A létrehozott objektum metódusainak neve az osztályától függ.
Meghatározás szerint minden felhasználó által definiált metódust
az adott (létező) példány nevével kell hívni.  Például ``x.f`` egy
érvényes függvényhivatkozás, ha az ``Osztalyom.f`` függvény létezik
(``x`` objektum az ``Osztalyom`` példánya), de ``x.i`` nem érvényes ha
``Osztalyom.i`` változót nem hoztuk létre az osztály definiálásakor.
Fontos, hogy  ``x.f`` nem ugyanaz, mint ``Osztalyom.f`` --- ez egy
*metódusobjektum*, nem egy függvényobjektum.

.. _tut-methodobjects:

Az metódusobjektumok
---------------------

Többnyire a metódusokat rögtön meghívjuk::

   x.f()

Példánkban ``x.f`` a ``'hello világ'`` string-el tér vissza. Ezt a függvényt nem
csak közvetlenül hívhatjuk meg:  ``x.f`` egy objektum metódus, tárolható és
később is hívható, például így::

   xf = x.f
   while True:
       print(xf())

Ez a kód az örökkévalóságig a ``hello világ`` üzenetet írja ki.

Pontosan mi történik egy objektummetódus hívásakor? Lehet, hogy már  észrevetted
hogy a ``x.f()``-t a fenti példában argumentum nélkül hívtuk meg - annak
ellenére, hogy :meth:`f` függvénydefiníciója  egy argumentum használatát előírja.
Mi van ezzel a argumentummal? Szerencsére a Pythonban ha egy argumentumot
igénylő függvényt argumentum nélkül próbálunk meghívni, kivételdobás
történik.

Lehet hogy már kitaláltad a választ: az a különleges a metódusokban, hogy
hívásukkor az őket tartalmazó osztálypéldányt megkapják az első változóban. A
példánkban ``x.f()`` hívása pontosan ugyanaz, mintha ``Osztalyom.f(x)`` metódust
hívnánk. Általában metódusok hívása *n* argumentummal ugyanaz, mintha az
osztálydefiníció függvényét hívnánk meg úgy, hogy a legelső argumentum
elé az aktuális példány nevét beillesztjük.

Ha  nem értenél valamit a metódusok működéséről, nézz meg kérlek néhány
gyakorlati példát. Amikor egy példányjellemzőjére hivatkozol, és az nem
létezik a változók között, az értelmező az osztálydefinícióban fogja keresni. Ha
a név egy érvényes osztályjellemzőre mutat, ami egy függvény, a fenti példában
szereplő folyamat történik: az értelmező az ``x.f()`` hívást átalakítja -- az
argumentumokat kigyűjti, majd első argumentumként ``x``-et tartalmazva létrehoz
egy új argumentumlistát és meghívja  a ``Osztalyom.f(x, argumentum1, arg2...)``
függvényt. 

.. _tut-remarks:

Mindenféle megjegyzés
========================

Az adatjellemzők felülírják az ugyanolyan nevű metódusokat;  a névütközések
elkerülése végett (amelyek nagyon nehezen megtalálható programhibákhoz
vezethetnek) érdemes betartani néhány elnevezési szabályt, melyekkel
minimalizálható az ütközések esélye. Ezek a szabályok például a metódusok
nagybetűvel írását, az adatjellemzők kisbetűs írását -- vagy alsóvonás
karakterrel kezdését jelentik; vagy igék használatát a metódusokhoz, és
főnevekét az adatjellemzőkhöz.

Az adatjellemzőkre a metódusok is hivatkozhatnak, éppúgy mint az  objektum
hagyományos felhasználói. Más szavakkal az osztályok nem használhatók csupasz
absztrakt adattípusok megvalósítására. Valójában a Pythonban jelenleg semmi
sincs, ami az adatrejtés elvét biztosítani tudná -- minden az elnevezési
konvenciókra épül. Másrészről az eredeti C alapú Python képes teljesen elrejteni
a megvalósítási  részleteket és ellenőrizni az objektum elérését, ha szükséges;
ehhez egy C nyelven írt kiegészítést kell használni.

A kliensek az adatjellemzőket csak óvatosan használhatják, mert
elronthatják azokat a variánsokat, amelyeket olyan eljárások tartanak
karban, amelyek időpontbélyeggel dolgoznak. Az objektum
felhasználói saját adatjellemzőiket bármiféle ellenőrzés nélkül
hozzáadhatják az objektumokhoz amíg ezzel nem okoznak névütközést --
az elnevezési konvenciók használatával elég sok fejfájástól
megszabadulhatunk!

A metódusokban nem használhatunk rövidítést az adatjellemzőkre (vagy más
metódusokra). Én úgy látom, hogy ez növeli a metódusok olvashatóságát,
és nem hagy esélyt a helyi és a példányosított változók összekeverésére,
mikor a metódus forráskódját olvassuk.

A hagyományokhoz hűen a metódusok első argumentumának neve rendszerint  ``self``.
Ez valóban csak egy szokás: a ``self`` névnek semmilyen speciális
jelentése nincs a Pythonban. Azért vegyük figyelembe, hogy ha eltérünk a
hagyományoktól, akkor a program nehezebben olvashatóvá válik, és a
*osztályböngésző* is a tradicionális változónevet használja.

Az osztály definíciójában megadott függvények az osztály példányai  számára
hoznak létre metódusokat (a példányhoz tartozó függvényeket). Nem szükségszerű
azonban hogy egy függvénydefiníció kódja az osztálydefiníció része legyen: egy
definíción kívüli függvény helyi változóhoz való rendelése is megfelel a célnak.
Például::

   # Egy osztályon kívül definiált függvény
   def f1(self, x, y):
       return min(x, x+y)

   class C:
       f = f1
       def g(self):
           return 'hello világ'
       h = g

Most ``f``, ``g`` és ``h`` egyaránt :class:`C` osztály jellemzői
(gyakorlatilag objektumhivatkozások) -- következésképpen :class:`C`
osztály minden példányának metódusai is -- ``h`` és ``g`` pedig
valójában ugyanazt a függvényt jelentik. Azért ne feledjük, hogy a fenti
példa használata a program olvasóját összekavarhatja!

Az osztályon belüli metódusok egymást is hívhatják  a ``self`` argumentum
használatával::

   class Taska:
       def __init__(self):
           self.tartalom = []
       def belerak(self, x):
           self.tartalom.append(x)
       def belerak_ketszer(self, x):
           self.belerak(x)
           self.belerak(x)

A metódusok a globális névtérben lévő függvényekre is hasonlóképp
hivatkozhatnak. (Maguk az osztálydefiníciók soha nem részei a globális
névtérnek!)  Míg egy kivételes esetben a globális névtér változóinak használata
jól jöhet, több esetben is jól jöhet a globális névtér elérése: a globális
névtérbe importált függvényeket és modulokat az adott osztálymetódusból is
használhatjuk, mintha az  adott függvényben vagy osztályban definiálták volna
azokat. Rendszerint az osztály az önmaga által definiált metódust a globális
névtérben tartja, és a következő részben meglátjuk majd, miért jó ha a metódusok
a saját osztályukra hivatkozhatnak!

.. _tut-inheritance:

Öröklés
=======

Természetesen az öröklés támogatása nélkül nem sok értelme lenne az osztályok
használatának. A származtatott osztályok definíciója a következőképpen néz ki::

   class SzarmaztatottOsztalyNeve(SzuloOsztalyNeve):
       <utasitas-1>
       .
       .
       .
       <utasitas-N>

a :class:`SzuloOsztalyNeve` névnek a származtatott osztály névterében léteznie
kell. Abban az esetben, ha a szülőosztály másik modulban lett definiálva, a
modulnev.szuloosztalynev formát is használhatjuk::

   class SzarmaztatottOsztalyNeve(modulnev.SzuloOsztalyNeve):

A származtatott osztály definíciójának feldolgozása hasonló a szülőosztályokéhoz
--- az osztályobjektum létrehozásakor a szülőosztály is a példány része lesz. Ha
egy osztályjellemzőre hivatkozunk, és az nincs jelen az osztályban, az
értelmező a szülőosztályban keresi azt --- és ha az szintén származtatott
osztály, akkor annak a  szülőjében folytatódik a keresés, rekurzívan.

A származtatott osztályok példányosításában nincs semmi különleges:
``SzarmaztatottOsztalyNeve()`` létrehozza az osztály új példányát. A metódusok
nyilvántartását a következőképp oldja meg az értelmező: először az aktuális
osztály megfelelő nevű osztály-objektumait vizsgálja meg, majd ha nem találja a
keresett metódust, elindul a szülőosztályok láncán. Ha a keresési folyamat
eredményes, tehát valamelyik szülőosztályban megtalálta a keresett nevű
objektumot, az adott objektumnév hivatkozása érvényes lesz: a talált objektumra
mutat.

A származtatott osztályok felülírhatják a szülőosztályok metódusait.  A
metódusoknak nincsenek különleges jogaik más, ugyanabban az objektumban lévő
metódusok hívásakor. A szülőosztály egy metódusa ``/a_eredeti()/``, amely
ugyanazon szülőosztály egy másik metódusát hívja ``/b()``, lehet hogy nem fog
működni, ha a származtatott osztályból felülírják ``/a_uj()/, ami nem hívja már
b()-t``. (C++ programozóknak: a Pythonban minden metódus valójában
:keyword:`virtuális` típusú.

A származtatott osztály metódusa, amely felülírja a szülőosztály egy metódusát,
valójában inkább kiterjeszti az eredeti metódust, és nem egyszerűen csak
kicseréli. A szülőosztály metódusára így hivatkozhatunk:
``SzuloOsztalyNev.metodusnev(self, argumentumok)``. Ez néha jól jöhet.  (Fontos,
hogy ez csak akkor működik, ha a szülőosztáy a globális névtérben lett
létrehozva, vagy közvetlenül beimportálva.)

.. _tut-multiple:

Többszörös öröklés
------------------

A python korlátozottan támogatja a többszörös öröklést is. Egy ilyen osztály
definíciója a következőképp néz ki::

   % class DerivedClassName(Base1, Base2, Base3):
   %     <statement-1>
   class SzarmaztatottOsztalyNeve(Szulo1, Szulo2, Szulo3):
         <utasitas-1>

       .
       .
       .
   %     <statement-N>
         <utasitas-N>

Az egyedüli nyelvtani szabály amit ismerni kell, az osztályjellemzők
feloldásának a szabálya. Az értelmező először a mélyebb rétegekben keres, balról
jobbra. A fenti példában ha a jellemző nem található meg  a
:class:`SzarmaztatottOsztaly`-ban, akkor először a :class:`Szulo1`-ben
keresi  azt, majd rekurzívan a :class:`Szulo2`-ben, és ha ott nem találja,
akkor lép tovább rekurzívan a többi szülőosztály felé.

Néhányan első pillanatban arra gondolnak, hogy a :class:`Szulo2` -ben és a
:class:`Szulo3`-ban kellene előbb keresni, a :class:`Szulo1` előtt ---
mondván hogy ez természetesebb lenne. Ez az elgondolás viszont igényli  annak
ismeretét, hogy mely jellemzőt  definiáltak a :class:`Szulo1`-ben vagy annak
egyik szülőosztályában, és csak ezután tudod elkerülni a  :class:`Szulo2`-ben
lévő névütközéseket. A mélyebb először szabály nem tesz különbséget a helyben
definiált és öröklött változók között.

Ezekből gondolom már látszik, hogy az átgondolatlanul használt többszörös
öröklődés a program karbantartását rémálommá teheti -- a névütközések
elkerülése végett pedig a Python csak a konvenciókra támaszkodhat.  A többszörös
öröklés egyik jól ismert problémája ha a gyermekosztály két szülőosztályának egy
közös nagyszülő osztálya van. Ugyan egyszerű kitalálni  hogy mi történik ebben
az esetben (a nagyszülő adat jellemzőinek egypéldányos  változatát használja
a gyermek) -- az még nem tisztázott, hogy ez a nyelvi kifejezésmód minden
esetben használható-e.

.. _tut-private:

Privát változók
===============

Egyedi azonosítók létrehozását az osztályokhoz a Python korlátozottan támogatja.
Bármely azonosító, amely így néz ki: ``__spam``  (legalább két bevezető
alsóvonás, amit legfeljebb egy alsóvonás követhet)
szövegesen kicserélődik a ``_classname__spam`` formára, ahol a  ``classname`` az
aktuális osztály neve egy alsóvonással bevezetve. Ez a csere végrehajtódik az
azonosító pozíciójára való tekintet nélkül, úgyhogy használható osztály-egyedi
példányok, osztályváltozók, metódusok  definiálására ---  még akkor is, ha más
osztályok példányait saját privát változói közé veszi fel (???Ford: ellenőrizni!)

Ha a cserélt név hosszabb mint 255 karakter, az értelmező  csonkíthatja az új
nevet.  Külső osztályoknál, vagy ahol az osztálynév következetesen
alsóvonásokból áll??? nem történik csonkítás.   A névcsonkítás célja az, hogy az
osztályoknak egyszerű megoldást biztosítson a "private" változók és metódusok
definiálására --- anélkül, hogy aggódnunk kellene a származtatott osztályokban
megjelenő privát változók miatt, vagy esetleg a származtatott osztályban már
meglévő változó privát változóval való felülírása miatt. Fontos tudni, hogy a
névcsonkítási szabályok elsősorban a problémák  elkerülését célozzák meg ---
így még mindig leheséges annak, aki nagyon akarja, hogy elérje vagy módosítsa a
privátnak tartott változókat.

Ez speciális körülmények között nagyon hasznos lehet, például  hibakeresésnél,
és ez az egyik oka annak, hogy ezt  a kibúvót még nem szüntették meg.

(Buglet (duda): származtatás egy osztályból  a szülőosztály nevével megegyező
néven -- a szülőosztály privát változóinak  használata ekkor lehetséges lesz.)

Fontos, hogy a  ``exec``, ``eval()`` vagy ``evalfile()`` által végrehajtott kód
nem veszi figyelembe a hívó osztály nevét az aktuális osztály esetében --- ez
hasonló a ``global`` változók működéséhez, előre lefordított byte-kód esetében.
Hasonló korlátozások léteznek a ``getattr()``, ``setattr()`` és ``delattr()``
esetében, ha közvetlenül hívják meg a ``__dict__`` utasítást.

.. _tut-odds:

Egyebek...
==========

Alkalomadtán hasznos lehet a Pascal "record", vagy a C "struct" adattípusaihoz
hasonló szerkezetek használata -- egybefogni néhány  összetartozó adatot. A
következő üres osztálydefiníció ezt szépen  megvalósítja::

   class Alkalmazott:
       pass

   john = Alkalmazott() # Egy üres alkalmazott rekordot hoz létre

   # A rekord mezőinek feltöltése
   john.nev = 'John Doe'
   john.osztaly = 'számítógépes labor'
   john.fizetes = 1000

Ez a kis kódrészlet ami egyéni adat típusokat kezel, gyakran követ
adatszerkezetek tárolására való osztálydefiníciókat.

Az egyes objektumpéldányok saját jellemzőkkel is rendelkeznek:
``m.__self__`` az :meth:`m` metódussal rendelkező objektumpéldány,
``m.__func__`` a hoz tartozó függvényobjektum.

.. _tut-exceptionclasses:

Kivételek alkalmazása az osztályokban
=====================================

A felhasználói kivételek az osztályokban is működnek --- használatukkal egy
bővíthető, hierarchikus kivétel-struktúrát építhetünk fel.

A ``raise`` utasításnak kétféle használati módja lehetséges::

   raise Osztaly, peldany

   raise peldany

Az első esetben ``peldany``-nak az Osztaly-ból kell származnia. A második eset
egy rövidítés::

   raise peldany.__class__, peldany

Az except záradékban lévő class megegyezik egy kifejezéssel ha az ugyanaz az
osztály vagy a szülőosztály (de nem másik kerülő úton --- az except záradékban
lévő származtatott osztály figyelése nem egyezik meg a szülőosztály
figyelésével.) Például a következő kód  B, C, D kimenetet fog produkálni::


   class B:
       pass
   class C(B):
       pass
   class D(C):
       pass

   for c in [B, C, D]:
       try:
           raise c()
       except D:
           print("D")
       except C:
           print("C")
       except B:
           print("B")

Note that if the except clauses were reversed (with ``except B`` first), it
would have printed B, B, B --- the first matching except clause is triggered.

Fontos, hogy ha az except záradékot megfordítjuk (``except B`` van először), B,
B, B lesz a kimenet --- az első except-re való illeszkedés után a többit már nem
vizsgálja az értelmező.

Mikor egy kezeletlen kivétel miatt hibaüzenetet küld az értelmező, és a hiba egy
osztályból származik, a kimenetben szerepel az osztály  neve, egy kettőspont,
egy space, és befejezésül a példányt stringgé  konvertálja az értelmező a
beépített :func:`str` függvénnyel.

.. _tut-iterators:

Iterátorok
============

Valószínűleg már észrevetted, hogy a legtöbb tároló objektum bejárható a
:keyword:`for` ciklusutasítás használatával::

   for element in [1, 2, 3]:
       print(element)
   for element in (1, 2, 3):
       print(element)
   for key in {'one':1, 'two':2}:
       print(key)
   for char in "123":
       print(char)
   for line in open("myfile.txt"):
       print(line)

Az elérés ilyen módja tiszta, tömör és kényelmes. Az iterátorok
használata áthatja és egységesíti a Pythont. A színfalak mögött a
:keyword:`for` utasítás meghívja a :func:`iter()` függvényt  a bejárandó
tárolóra. Ez a függvény egy iterátor-objektummal tér vissza, amely
definiálja a :meth:`__next__()` metódust, amellyel a tároló elemeit
lehet elérni, egyszerre egy elemet. Ha nincs több elem, a
:meth:`__next__()` metódus :exc:`StopIteration` kivételt dob, amely a
:keyword:`for` ciklust megszakítja.  A :func:`next()` beépített
függvénnyel is meghívhatom a  :meth:`__next__()` metódust, ahogy a
következő példában látható::

   >>> s = 'abc'
   >>> it = iter(s)
   >>> it
   <iterator object at 0x00A1DB50>
   >>> next(it)
   'a'
   >>> next(it)
   'b'        
   >>> next(it)
   'c'        
   >>> next(it)

   Traceback (most recent call last):
     File "<pyshell#6>", line 1, in <module>
       next(it)
   StopIteration

A ciklusok működésébe bepillantva már könnyű saját iterátorprotokollt
adni egy osztályhoz. Definiálni kell az :meth:`__iter__` metódust, ami a
:meth:`__next__()` függvény eredményeképp létrejövő objektummal tér
vissza. Ha az osztály definiálja a :meth:`__next__` metódust, akkor az
:meth:`__iter__` egyszerűen a ``self`` objektummal tér vissza::


   >>> class Reverse:
       "Iterator for looping over a sequence backwards"
       def __init__(self, data):
           self.data = data
           self.index = len(data)
       def __iter__(self):
           return self
       def __next__(self):
           if self.index == 0:
               raise StopIteration
           self.index = self.index - 1
           return self.data[self.index]

   >>> for char in Reverse('spam'):
   	print(char)

   m
   a
   p
   s

.. xxx

.. _tut-generators:

Generátorok
===========

A generátorok egyszerű és hatékony eszközök iterátorok készítésére.  A
normális függvényekhez hasonló a felépítésük, de a  :keyword:`yield`
utasítást használják ha adatot szeretnének visszaadni. A
:meth:`__next__` metódus minden hívásakor a generátor ott folytatja az
adatok feldolgozását, ahol az előző hívásakor befejezte (emlékszik
minden változó értékére, és hogy melyik utasítást hajtotta végre
utoljára). Lássunk egy példát arra, hogy milyen egyszerű egy generátort
készíteni::

   >>> def reverse(data):
           for index in range(len(data)-1, -1, -1):
               yield data[index]

   >>> for char in reverse('golf'):
           print(char)

   f
   l
   o
   g	

Bármi, amit egy generátorral megtehetsz, osztály alapú bejárókkal is
kivitelezhető -- az előző részben leírtak szerint.  Ami a generátorokat
tömörré teszi, hogy a  :meth:`__iter__` és a :meth:`__next__` metódusok
automatikusan létrejönnek.

Egy másik fontos lehetőség hogy a helyi változók, és a végrehajtás állapota
automatikusan tárolódik a generátor két futása között.

Az automatikus metóduslétrehozáson és programállapot-mentésen felül,
amikor a generátor futása megszakad, ez az esemény automatikusan
:exc:`StopIteration` kivételt vált ki.  Egymással kombinálva ezek a
nyelvi szolgáltatások egyszerűvé teszik az iterátorok készítését néhány
függvény megírásának megfelelő -- viszonylag kis erőfeszítéssel.

.. _tut-genexps:

Generátorkifejezések
=====================

Néhány egyszerű generátort egyszerűen lehet kódolni egy olyan
kifejezéssel, amely a listaértelmezés szintaktikáját használja, de kerek
zárójellel a szögletes helyett. Ezeket a kifejezéseket arra a helyzetre tervezték,
amikor a generátor közvetlenül egy függvényben található. A
generátorkifejezés jóval tömörebb, de kevésbé rugalmasak, mint a teljes
generátordefiníciók és általában jóval memóriakímélőbbek, mint a
megfelelő listaértelmezések.

Példák::

   >>> sum(i*i for i in range(10))                 # négyzetek összege
   285

   >>> xvec = [10, 20, 30]
   >>> yvec = [7, 5, 3]
   >>> sum(x*y for x,y in zip(xvec, yvec))         # belsőszorzat
   260

   >>> from math import pi, sin
   >>> sin_tabla = {x: sin(x*pi/180) for x in range(0, 91)}

   >>> egyedi_szavak = set(szo  for sor in oldal  for szo in sor.split())

   >>> valedictorian = max((student.gpa, student.name) for student in graduates)

   >>> adat = 'golf'
   >>> list(adat[i] for i in range(len(adat)-1, -1, -1))
   ['f', 'l', 'o', 'g']



.. rubric:: Lábjegyzet

.. [#] Except for one thing.  Module objects have a secret read-only attribute called
   :attr:`__dict__` which returns the dictionary used to implement the module's
   namespace; the name :attr:`__dict__` is an attribute but not a global name.
   Obviously, using this violates the abstraction of namespace implementation, and
   should be restricted to things like post-mortem debuggers.

