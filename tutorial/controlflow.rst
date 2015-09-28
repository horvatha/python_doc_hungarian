.. _tut-morecontrol:

**************************
További vezérlő utasítások
**************************

Az imént említett :keyword:`while` utasítás mellett a Python ismeri a más
nyelvekben szereplő leggyakoribb vezérlő utasításokat is --  némi
változtatással.


.. _tut-if:

Az :keyword:`if` utasítás
=========================

Talán a legjobban ismert utasítástípus az :keyword:`if` utasítás. Példa::

   >>> x = int(input("Írjon egy számot: "))
   >>> if x < 0:
   ...      x = 0
   ...      print('Negatív, lecseréltem nullára')
   ... elif x == 0:
   ...      print('Nulla')
   ... elif x == 1:
   ...      print('Egy')
   ... else:
   ...      print('Egynél több.')
   ... 

Hiányozhat, de lehet egy vagy akár egynél több :keyword:`elif` rész, a
:keyword:`else` rész szintén elmaradhat.  Az ':keyword:`elif`' kulcsszó -- amely
az 'else if' rövidítése -- hasznos a felesleges behúzások elkerülésre. Egy
:keyword:`if` ...\ :keyword:`elif` ... :keyword:`elif` ... sor helyettesíti a
más nyelvekben található :keyword:`switch` és :keyword:`case` utasításokat.


.. _tut-for:

A :keyword:`for` utasítás
=========================

A :keyword:`for` utasítás különbözik attól, amely a C-ben és a Pascalban
található. Ahelyett, hogy mindig egy számtani sorozattal dolgozna (mint a
Pascalban), vagy hogy megadná a lehetőséget a felhasználónak, hogy saját maga
határozza meg mind az iterációs lépést, mind a kilépési feltételt (ahogy a C-ben
van) a Python :keyword:`for` utasítása végighalad a sorozat (pl. szövegek
listája) összes elemén olyan sorrendben, ahogy a listában szerepelnek. Például::

   >>> # Megmérjük a szavak hosszát:
   ... a = ['cat', 'window', 'defenestrate']
   >>> for x in a:
   ...     print(x, len(x))
   ... 
   cat 3
   window 6
   defenestrate 12

Nem biztonságos megváltoztatni a sorozatot, amelyen ciklussal
végighaladunk (ez csak megváltoztatható sorozattal, például listával
történhet meg).   Ha mégis szükséges megváltoztatnod a listát, akkor  a
másolatát használd a for ciklusban.  A szelet (slice) jelölési móddal
ezt kényelmesen megteheted::

   >>> for x in a[:]:  # egy másolatot csinál az eredeti listáról
   ...    if len(x) > 6: a.insert(0, x)
   ... 
   >>> a
   ['defenestrate', 'cat', 'window', 'defenestrate']


.. _tut-range:

A :func:`range` függvény
========================

Ha egy számsorozaton kell végighaladnunk, a :func:`range` beépített függvény
lehet szolgálatunkra.  Ez egy számtani sorozatot állít elő::

    >>> for i in range(5):
    ...     print(i)
    ...
    0
    1
    2
    3
    4

A megadott végpont sohasem része a listának; ``range(10)`` 10 értéket hoz
létre, pontosan egy tízelemű sorozat indexeit.  Lehetőség van rá, hogy a sorozat
más számmal kezdődjön, vagy hogy más lépésközt adjunk meg (akár negatívat is)::


    range(5, 10)
       5-től 9-ig

    range(0, 10, 3)
       0, 3, 6, 9

    range(-10, -100, -30)
      -10, -40, -70

Ha egy sorozat indexein akarunk végighaladni, használjuk a  :func:`range` és
:func:`len` függvényeket a következőképpen::

   >>> a = ['Mary', 'had', 'a', 'little', 'lamb']
   >>> for i in range(len(a)):
   ...     print(i, a[i])
   ... 
   0 Mary
   1 had
   2 a
   3 little
   4 lamb

.. _tut-break:

A :keyword:`break` és a :keyword:`continue` utasítások, az :keyword:`else` ág a ciklusokban
===========================================================================================

A :keyword:`break` utasítás -- ahogy a C-ben is -- a :keyword:`break`-et
tartalmazó legbelső  :keyword:`for` vagy :keyword:`while` ciklusból ugrik ki.

A ciklusszervező utasításoknak lehet egy ``else`` águk. Ez akkor hajtódik
végre, ha a ciklus végighaladt a listán (:keyword:`for` esetén), illetve ha a
feltétel hamissá vált (:keyword:`when` esetén), de nem hajtódik végre, ha a
ciklust a :keyword:`break` utasítással szakítottuk meg. Ezt a következő példával
szemléltetjük, amely a prímszámokat keresi meg::

   >>> for n in range(2, 10):
   ...     for x in range(2, n):
   ...         if n % x == 0:
   ...            print(n, 'felbontható:', x, '*', n//x)
   ...            break
   ...     else:
   ...         # a ciklus nem talált osztót
   ...         print(n, 'prímszám.')
   ... 
   2 prímszám.
   3 prímszám.
   4 felbontható: 2 * 2
   5 prímszám.
   6 felbontható: 2 * 3
   7 prímszám.
   8 felbontható: 2 * 4
   9 felbontható: 3 * 3

(Igen, ez a helyes kód. Nézd meg alaposan: az ``else`` ág a
:keyword:`for` ciklushoz, és **nem** az :keyword:`if` utasításhoz
tartozik.)

Egy ciklusban az ``else`` ág inkább a :keyword:`try` utasítás ``else``
ágára hasonlít, mint az :keyword:`if` utasításéra: a :keyword:`try`
utasítás  ``else`` ága akkor fut le, ha nincs kivétel, egy ciklus
``else`` ága pedig akkor, ha nem hajtódik végre ``break``.  Bővebben a
:keyword:`try` utasításról és a kivételekről: :ref:`tut-handling`.

A :keyword:`continue` utasítás, amely szintén a C-ből származik, a
következő iterációval folytatja a ciklust::

    >>> for num in range(2, 10):
    ...     if num % 2 == 0:
    ...         print("Páros számot találtam:", num)
    ...         continue
    ...     print("Számot találtam:", num)
    Páros számot találtam: 2
    Számot találtam: 3
    Páros számot találtam: 4
    Számot találtam: 5
    Páros számot találtam: 6
    Számot találtam: 7
    Páros számot találtam: 8
    Számot találtam: 9

.. _tut-pass:

A :keyword:`pass` utasítás
==========================

A :keyword:`pass` utasítás nem csinál semmit. Akkor használható, ha
szintaktikailag szükség van egy utasításra, de a programban nem kell semmit sem
csinálni. Például::

   >>> while True:
   ...       pass # Elfoglalt - billentyűzetről érkező megszakításra (Ctrl+C) vár.
   ... 

Gyakran használjuk arra, hogy minimális osztályt hozzunk létre::

   >>> class UresOsztalyom:
   ...     pass
   ...


A :keyword:`pass` utasítást hely fenntartására is használhatod függvény
vagy feltételes rész esetén, amikor új kódon dolgozol, lehetővé téve,
hogy jóval elvontabb szinten gondolkodj.  A :keyword:`pass` kulcsszót
figyelmen kívül hagyja a programfutás::

   >>> def initlog(*args):
   ...     pass   # Ne feljetsem el implementálni!
   ...

.. _tut-functions:

Függvények definiálása
======================

Létrehozhatunk egy függvényt, amely egy megadott értékig írja ki a
Fibonacci--sorozatot::

   >>> def fib(n):
   ...     "Kiír egy Fibonacci-sorozatot n-ig."
   ...     a, b = 0, 1
   ...     while b < n:
   ...         print(b, end="")
   ...         a, b = b, a+b
   ... 
   >>> # Hívjuk meg a függvényt amit éppen létrehoztunk:
   ... fib(2000)
   1 1 2 3 5 8 13 21 34 55 89 144 233 377 610 987 1597

.. index::
   single: dokumentációs karakterlánc
   single: karakterláncok, dokumentáció

A :keyword:`def` kulcsszó a függvény *definícióját* jelzi. Ezt egy
függvénynévnek, majd zárójelben a paraméterek listájának kell követnie. Az
utasítások -- amelyek a definíció testét alkotják -- a következő sorban
kezdődnek, és behúzással kell kezdeni azokat.

A függvény testének első utasítása lehet egy literális karakterlánc
is; ez a karakterlánc a függvény dokumentációs karakterlánca, angolul
röviden :dfn:`docstring`.
(Bővebben a docstring-ről a következő fejezetben: :ref:`tut-docstrings`.)
Vannak eszközök, amelyek a :dfn:`docstring`-et használják ahhoz, hogy az
online vagy a nyomtatott dokumentációt automatikusan elkészítsék, vagy
hogy a felhasználót segítsék a kódban történő interaktív böngészéshez.
Jó szokás, hogy a docstringet beleírjuk a kódba,  kérünk téged hogy te
is szokjál rá.

A függvény *végrehajtása* egy új szimbólumtáblát hoz létre a függvény helyi
változói számára.  Pontosabban: minden értékadás a függvényben a helyi
szimbólumtáblában tárolódik; a változókra való hivatkozások esetén először a
Python helyi szimbólumtáblában, aztán a globális szimbólumtáblában,
végül a beépített nevek táblájában keresgél. Így globális változóknak nem
adhatunk közvetlenül értéket egy függvényben (hacsak nem nevezzük meg egy
:keyword:`global` utasításban), jóllehet hivatkozhatunk rá.

A függvényhívás aktuális paraméterei (argumentumai) bekerülnek a hívott
függvény helyi szimbólumtáblájába amikor azt meghívjuk,  így az
argumentumok mindig *értékeket* adnak át (ahol az *érték* mindig az
objektumra történő *hivatkozás*, nem az objektum értéke).  [#]_ Ha a
függvény egy másik függvényt hív, akkor  az új híváshoz egy új helyi
szimbólumtábla jön létre.

A függvénydefiníció a függvény nevét beírja az aktuális szimbólumtáblába. A
függvénynév értékének van egy típusa, amelyet a fordító a felhasználó által
definiált függvényként ismer fel.  Ezt az értéket társíthatjuk egy másik
változóhoz, amely ekkor szintén függvényként használható. Ez egy
általános átnevezési eljárásként szolgál::

   >>> fib
   <function fib at 10042ed0>
   >>> f = fib
   >>> f(100)
   1 1 2 3 5 8 13 21 34 55 89

Más nyelvektől jőve kifogásolhatja valaki, hogy a ``fib`` nem függvény,
hanem eljárás, mivel nem tér vissza semmilyen értékkel.

Valójában azok a függvények is, amelyekben nincs :keyword:`return`
utasítás, visszaadnak egy értéket, bár egy elég unalmasat. Ez az érték a
``None`` (egy beépített név).  A ``None`` érték kiírását általában
elnyomja az értelmező,  ha csak ezt az értéket kell kiírnia.  Erről
meggyőződhetünk, ha akarunk a :func:`print` függvény használatával::

   >>> print(fib(0))
   None

Könnyen írhatunk olyan függvényt, amely visszatér a Fibonacci-sorozat értékeit
tartalmazó listával ahelyett, hogy kiíratná azokat::

   >>> def fib2(n): # Visszaadja a Fibonacci-sorozatot n-ig 
   ...     "A Fibonacci-sorozat n-nél kisebb elemeit adja vissza egy listában."
   ...     eredmeny = []
   ...     a, b = 0, 1
   ...     while b < n:
   ...         eredmeny.append(b)    # lásd lejjebb
   ...         a, b = b, a+b
   ...     return eredmeny
   ... 
   >>> f100 = fib2(100)    # hívjuk meg
   >>> f100                # írjuk ki az eredményt
   [1, 1, 2, 3, 5, 8, 13, 21, 34, 55, 89]

Ez a példa néhány új vonását mutatja a Pythonnak:

* A :keyword:`return` utasítás egy értékkel tér vissza a függvény futásának
  befejezésekor. A :keyword:`return` utasítás argumentum nélkül ``None`` értéket
  ad vissza. Ha egy függvény lefutott, és nem hajtott végre :keyword:`return`
  utasítást, akkor is ``None`` értékkel tér vissza.

* A ``eredmeny.append(b)`` utasítás meghívja az ``eredmeny`` listaobjektum
  egy metódusát.  A metódus egy olyan függvény, amely egy objektumhoz ,,tartozik",
  ``obj.metódusnév`` alakban írjuk, ahol az ``obj`` valamelyik objektum (lehet egy
  kifejezés), és a ``metódusnév`` egy olyan metódus neve, amelyet az objektumtípus
  definiál.  Különböző típusoknak különböző metódusai vannak.  Különböző
  típusoknak lehet azonos nevű metódusa mindenféle kétértelműség veszélye nélkül.
  (Lehetőség van rá, hogy definiáljunk saját objektumokat és metódusokat
  *osztályok* használatával, lásd :ref:`tut-classes`)

  A példában szereplő :meth:`append` metódus a lista-objektumokra lett definiálva;
  ez hozzáfűz egy új elemet a lista végéhez.  Ebben az esetben azonos az
  ``eredmeny = eredmeny + [b]`` alakkal, de sokkal hatékonyabb.


.. _tut-defining:

A függvények definiálásáról bővebben
==============================================

Lehetőségünk van függvényeket definiálni változó számú argumentummal. Ennek három
formája van, amelyek variálhatók.


.. _tut-defaultargs:

Alapértelmezett  (default) argumentumértékek
--------------------------------------------

A leghasznosabb alak az, ha  egy vagy több argumentumnak is meghatározott
alapértéket adunk meg (azaz egy olyan értéket, amit ez az argumentum felvesz, ha
nem adunk értéket neki). Ez így egy olyan függvényt hoz létre, amelyet kevesebb
argumentummal is meghívhatunk, mint amennyivel definiáltuk::

   def ask_ok(szoveg, probalkozasok=4, hibauzenet='igen vagy nem!'):
       while 1:
           ok = input(szoveg)
           if ok in ('i', 'igen','I','IGEN'):
               return True
           if ok in ('n', 'nem', 'N','NEM'):
               return False
           probalkozasok = probalkozasok - 1
           if probalkozasok < 0:
               raise IOError('értelmetlen felhasználó')
           print(hibauzenet)

Ez a függvény többféle módon hívható meg:

* megadhatjuk csak a kötelező argumentumot:
  ``ask_ok('Valóban ki akarsz lépni?')``
* csak egy elhagyható argumentumot adunk meg:
  ``ask_ok('Felülírhatom a fájlt?', 2)``.
* minden argumentumot megadunk:
  ``ask_ok('Felülírhatom a fájlt?', 2, 'igen-nel vagy nem-mel válaszolj!')``.

Az előző program egyben példa az :keyword:`in` kulcsszó használatára is. Így
tesztelhetjük, hogy a sorozat vajon tartalmaz-e egy adott értéket, vagy nem.

Az alapértékeket a fordító akkor határozza meg, amikor  a függvény
definíciójával először találkozik,  emiatt ezek kiszámítása csak egyszer
történik meg! Így például a következő program::

   i = 5

   def f(arg=i):
       print(arg)

   i = 6
   f()

``5``-öt ír ki.

**Fontos figyelmeztetés:**  Az alapértékeket a fordító **csak egyszer**
határozza meg!  Emiatt különbség van, ha az alapérték megváltoztatható objektum,
mint amilyen a lista, szótár vagy a legtöbb példányosodott osztály.  Például az
alábbi függvény összegyűjti az egymás utáni hívások során neki adott
paramétereket::

   def f(a, L=[]):
       L.append(a)
       return L

   print(f(1))
   print(f(2))
   print(f(3))

A program kimenete::

   [1]
   [1, 2]
   [1, 2, 3]

Ha nem akarod az alapértékeket láthatóvá tenni az egymást követő hívások
számára, akkor inkább ehhez hasonlóan írd a függvényt::

   def f(a, L=None):
       if L is None:
           L = []
       L.append(a)
       return L


.. _tut-keywordargs:

Kulcsszavas argumentumok
------------------------

A függvényeket akár ``kulcsszó=érték`` formában megadott, úgynevezett
kulcsszavas argumentumok használatával is meghívhatunk. Például a
következő függvény::

   def parrot(voltage, state='a stiff', action='voom', type='Norwegian Blue'):
       print("-- This parrot wouldn't", action,)
       print("if you put", voltage, "Volts through it.")
       print("-- Lovely plumage, the", type)
       print("-- It's", state, "!")

meghívható az összes alábbi módon::

   parrot(1000)                                          # 1 hely szerinti argumentum
   parrot(voltage=1000)                                  # 1 kulcsszavas argumentum
   parrot(voltage=1000000, action='VOOOOOM')             # 2 kulcsszavas argumentum
   parrot(action='VOOOOOM', voltage=1000000)             # 2 kulcsszavas argumentum
   parrot('a million', 'bereft of life', 'jump')         # 3 hely szerinti argumentum
   parrot('a thousand', state='pushing up the daisies')  # 1 hely szerinti, 1 kulcsszavas

de a következő hívások mind érvénytelenek::

   parrot()                     # a kötelező argumentum hiányzik
   parrot(voltage=5.0, 'dead')  # nem-kulcsszavas argumentum kulcsszavas után
   parrot(110, voltage=230)     # kétszeres értékadás egy argumentumnak
   parrot(actor='John Cleese')  # ismeretlen kulcsszó

A függvényhívások esetén a kulcsszavas argumentumoknak a hely szerintiek
után kell állniuk. Minden kulcsszavas argumentumnak olyannak kell
lennie, amely egyezik a függvény által elfogadott valamelyik
argumentummal (pl. az ``actor`` nem érvényes argumentum a ``parrot``
függvény számára), és a sorrendjük lényegtelen. Akár lehetnek kötelező
argumentumok is (pl. ``parrot(voltage=1000)`` is érvényes).

Egy hívás során nem kaphat egy argumentum egynél több alkalommal
értéket.  Itt van egy példa, amely nem hajtódik végre emiatt a megkötés
miatt::

   >>> def function(a):
   ...     pass
   ... 
   >>> function(0, a=0)
   Traceback (most recent call last):
     File "<stdin>", line 1, in ?
   TypeError: function() got multiple values for keyword argument 'a'

Ha van egy ``**név`` alakú formális paraméter utolsóként, akkor egy ilyen nevű
szótárban tárolódik az összes kulcsszavas argumentum, amelynek a kulcsszava nem
illeszkedik egyetlen formális paraméterre sem. Ez együtt használható egy
``*név`` alakú formális paraméterrel (ez a következő alszakaszban kerül
tárgyalásra) amely belerakja  egy tuple-ba az összes olyan
nem-kulcsszavas argumentumot, amely nincs benne a formális
paraméterlistában. A ``*név``-nek mindíg a  ``**név`` előtt kell lennie.
Például, ha egy ilyen függvényt definiálunk::

   def sajtuzlet(sajtfajta, *argumentumok, **kulcsszavak):
       print("-- Van Önöknél", sajtfajta, '?')
       print("-- Sajnálom, teljesen kifogytunk a", sajtfajta+'ból')
       for arg in argumentumok:
           print(arg)
       print("-" * 40)
       kulcsok = sorted(kulcsszavak.keys())
       for kw in kulcsok:
           print(kw, ":", kulcsszavak[kw])

Ez meghívható így is::

   sajtuzlet('Pálpusztai', "Ez nagyon büdös, uram.",
              "Ez nagyon, NAGYON büdös, uram.",
              vevo='Sajti János',
              boltos='Pálinkás Mihály',
              helyszin='Sajtbolt')

és természetesen ezt fogja kiírni::

   -- Van Önöknél Pálpusztai ?
   -- Sajnálom, teljesen kifogytunk a Pálpusztaiból
   Ez nagyon büdös, uram.
   Ez nagyon, NAGYON büdös, uram.
   ----------------------------------------
   boltos : Pálinkás Mihály
   helyszin : Sajtbolt
   vevo : Sajti János

Megjegyzendő, hogy a ``kulcsszavak`` nevű szótár tartalmának
kinyomtatása előtt a ``kulcsok`` változóba a ``kulcsszavak`` szótár
kulcsszavainak rendezett listáját raktuk; ha nem ezt tesszük, akkor az a
sorrend, ahogy az argumentumokat kiiratjuk határozatlan lenne.

.. _tut-arbitraryargs:

Tetszőleges hosszúságú argumentumlisták
---------------------------------------

Végül itt a legritkábban használt lehetőség, amikor egy függvénynek tetszőleges
számú argumentuma lehet. Ezeket az argumentumokat egy tuple-ba helyezi el a
Python.  A változó számosságú argumentum előtt akárhány (akár egy sem) egyszerű
argumentum is előfordulhat. ::

   def tobb_adat_irasa(file, separator, *args):
       file.write(separator.join(args))

Normális esetben ezek az úgynevezett ``variadikus`` argumentumoknak kell
a formális paraméterek legvégén állniuk, mivel ezek gyűjtik össze az
összes maradék bemenő argumentumot, amelyet a függvénynek megadtunk.
Minden formális paraméter, amely az ``*args`` paraméter után áll, csak
kulcsszóval hívható meg, nem lehet hely szerinti argumentumként
meghívni. ::

   >>> def osszefuz(*args, sep="/"):
   ...    return sep.join(args)
   ...
   >>> osszefuz("föld", "mars", "venus")
   'föld/mars/venus'
   >>> osszefuz("föld", "mars", "venus", sep=".")
   'föld.mars.venus'

.. _tut-unpacking-arguments:

Argumentumlista kicsomagolása
-----------------------------

Ennek fordítottja történik, ha listába vagy tuple-ba becsomagolt
argumentumokat ki kellene csomagolni olyan függvény meghívásához, amely
elkülönített, helyhezkötött változókat vár. Például a beépített
:func:`range` függvény egymástól elkülönítve várja a  *start* és *stop*
értékeket. Ha ezek nem egymástól elválasztva állnak rendelkezésre, akkor
a :keyword:`break` függvényhívásban a ``*`` műveletjelet tegyük az
összetett-típusú változó neve elé, ez kicsomagolja a listából vagy
tuple-ből az adatokat.  ::

   >>> range(3, 6)      # normális függvényhívás, különálló paraméterekkel
   [3, 4, 5]
   >>> args = [3, 6]
   >>> range(*args)     # listából kicsomagolt paraméterekkel történő függvényhívás
   [3, 4, 5]

.. _tut-lambda:

Lambda-formák
-------------

A :keyword:`lambda` kulcsszóval rövid névtelen függvényeket lehet
létrehozni. Íme egy függvény, amely a két argumentumának összegével tér
vissza: ``lambda a, b: a+b``.  A lambda-formákat mindenhol
használhatjuk, ahol függvényobjektumok szerepelhetnek.  Szintaktikailag
egyetlen kifejezés szerepelhet bennük. Értelmét  tekintve hab a normális
függvények tortáján. A beágyazott függvényekhez hasonlóan látja az őt
meghívó környezet minden változóját.  ::

   >>> def make_incrementor(n):
   ...     return lambda x: x + n
   ...
   >>> f = make_incrementor(42) # make_incrementor magyarul kb.: csinálj növelő-t
   >>> f(0)
   42
   >>> f(1)
   43

A fenti példa arra használja a lambda-kifejezést, hogy egy függvényt
adjon vissza. Ezen kívül lehetőséget biztosít, hogy egy függvényt
adhassunk át argumentumként::

    >>> szamok = [(1, "egy"), (2, "kettő"), (3, "három"), (4, "négy")]
    >>> szamok.sort(key = lambda szam: szam[1])
    >>> szamok
    [(1, 'egy'), (3, 'három'), (2, 'kettő'), (4, 'négy')]

A fenti példa a második (azaz 1-es indexű) tag szerint rendezi sorba a
párokat.


.. _tut-docstrings:

A dokumentációs karakterláncok
-------------------------------

.. index::
   single: docstrings
   single: documentation strings
   single: strings, documentation

A dokumentációs karakterláncok tartalmával és formájával kapcsolatban
egy kialakult és bevált szokásról beszélhetünk.

Az első sor mindig az objektum céljának rövid, tömör összegzése.
Rövidsége miatt nem kell tartalmaznia az objektum nevét vagy típusát,
hiszen ezek az adatok más úton is kinyerhetők (kivéve, ha az objektum
neve a függvény működését leíró ige).  A szöveg nagybetűvel kezdődik és
ponttal végződik.

Ha a dokumentációs karakterlánc (``docstring``) több sorból áll, a
második sor üres lesz -- ezzel vizuálisan elkülönítjük az
összefoglalót a leírás további részétől. Az üres sort egy vagy
több rész követheti, ahol leírjuk az objektum hívásának módját, a
mellékhatásokat stb.

Maga a Python értelmező nem szedi le a helyközöket a többsoros literális
karakterláncból -- ha ezek kiszűrése szükséges, akkor ehhez külön
szövegfeldolgozó programot kellene használni. Ezt a problémát a
következő konvenció használatával kezeljük. Az első sor  *után* a
legelső nem üres sorban megjelenő szöveg behúzási távolsága határozza
meg az egész dokumentációs szöveg behúzását. (A legelső sort azért nem
használjuk erre a célra, mert a szöveg első betűje általában szorosan
követi a karakterláncot nyitó macskakörmöt, ennek eltolása nem  lenne
nyilvánvaló dolog.) A ``docstring`` -- fejrészt követő minden első
sorának elejéről levágunk pont ennyi helyközt. Ha ennél kevesebb
helyközt tartalmaz valamely sor -- bár ilyennek nem kéne lennie -- csak
a helyközök törlődnek, karakter nem vész el. A behúzások egyenlőségét
ajánlott mindig a tabulátorokat kibontva ellenőrizni  (általában 1
tabulátort 8 helyközzel helyettesítünk).

Itt van egy példa a többsoros docstring-re::

   >>> def fuggvenyem():
   ...     """Nem csinál semmit, de ez dokumentálva van.
   ... 
   ...     Valóban nem csinál semmit.
   ...     """
   ...     pass
   ... 
   >>> print(fuggvenyem.__doc__)
   Nem csinál semmit, de ez dokumentálva van.

        Valóban nem csinál semmit.

.. _tut-annotations:

Function Annotations
--------------------

.. sectionauthor:: Zachary Ware <zachary.ware@gmail.com>
.. index::
   pair: function; annotations
   single: -> (return annotation assignment)

(A fejezetet nem találtam fontosnak, ezért nem került egyelőre
fordításra -- a fordító.)

:ref:`Function annotations <function>` are completely optional,
arbitrary metadata information about user-defined functions.  Neither Python
itself nor the standard library use function annotations in any way; this
section just shows the syntax. Third-party projects are free to use function
annotations for documentation, type checking, and other uses.

Annotations are stored in the :attr:`__annotations__` attribute of the function
as a dictionary and have no effect on any other part of the function.  Parameter
annotations are defined by a colon after the parameter name, followed by an
expression evaluating to the value of the annotation.  Return annotations are
defined by a literal ``->``, followed by an expression, between the parameter
list and the colon denoting the end of the :keyword:`def` statement.  The
following example has a positional argument, a keyword argument, and the return
value annotated with nonsense::

   >>> def f(ham: 42, eggs: int = 'spam') -> "Nothing to see here":
   ...     print("Annotations:", f.__annotations__)
   ...     print("Arguments:", ham, eggs)
   ...
   >>> f('wonderful')
   Annotations: {'eggs': <class 'int'>, 'return': 'Nothing to see here', 'ham': 42}
   Arguments: wonderful spam


Intermezzo: kódolási stílus
=============================

.. sectionauthor:: Georg Brandl <georg@python.org>
.. index:: pair: coding; style

Most már egy hosszabb és összetettebb Python kódot szeretnél írni, itt
az idő, hogy beszéljünk a *kódolási stílusról*. A legtöbb nyelven
többféle módon lehet írni (pontosabban *formázni*); némelyik sokkal
olvashatóbb mint másikak. Az, hogy a kódunkat mások számára is
olvashatóvá tegyük mindig jó ötlet, és egy helyes kódolási stílus
elfogadása sokat segít ebben.

A Python számára a :pep:`8` vált a stílus útmutatójává, amelyet a
legtöbb projekt követ; egy nagyon olvasható és szemnek kellemes
kódolási stílust javasol. Minden Python-fejlesztőnek el kellene olvasnia
valamikor. Itt találod a legfontosabb pontjait:

* Használj 4-szóköz behúzást, de tabulátorokat ne!

  A 4 szóköz jó középút a kis behúzás (amely nagyobb egymásbaágyazási
  mélységet enged meg) és a nagy behúzás (egyszerűbb olvasni) között. A
  tabulátorok keveredést okozhatnak, így jobb, ha elkerüljük.

* Törjük úgy a sorokat, hogy ne lépjék túl a 79 karakter hosszúságot!

  Ez segíti azokat a felhasználókat, akiknek kisebb a kijelzőjük, és
  lehetővé teszi, hogy több programfájlt jelenítsünk meg egymás mellett
  nagyobb kijelzőn.

* Használj üres sorokat, hogy elválaszd a függvényeket és az
  osztályokat, valamint a nagyobb blokkokat egy függvényen belül!

* Ha lehet a megjegyzéseket a saját sorába írd!

* Használj dokumentációs karakterláncokat!

* Használj szóközöket a műveleti jelek körül és a vesszők után
  ``a = f(1, 2) + g(3, 4)``!

* Következetesen nevezd el az osztályokat és a függvényeket! Szokás
  szerint ``TeveJelolesModot`` alkalmazunk az osztályoknál és
  ``kisbetuket_alahuzassal`` a függvények esetén. Mindig a ``self``-et
  használd az első metódusargumentumnak (lásd az :ref:`tut-firstclasses`
  fejezetet az osztályokról és a metódusokról).

* Ne használj különleges kódolásokat, ha a kódodat várhatóan nemzetközi
  környezetben is használni fogják! A Python alapértelmezett UTF-8
  kódolása, vagy az ASCII minden esetben jól működik.

* Szintén ne használj nem-ASCII karaktert az azonosítókban, ha csak a
  legkisebb esélye is van, hogy más nyelvet beszélő emberek fogják
  olvasni vagy karbantartani a kódot!


.. rubric:: Lábjegyzet

.. [#] Valójában az *objektumhivatkozással történő hívás* jobb elnevezés
    lenne, mivel, ha egy megváltoztatható objektumot adunk át, a hívó
    látni fogja az összes változást, amit a hívott függvény végez (elem
    beszúrása listába).

