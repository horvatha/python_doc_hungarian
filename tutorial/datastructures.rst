.. _tut-structures:

**************
Adatstruktúrák
**************

Ez a fejezet az eddig tanultakból pár dolgot részletesebben is leír, és pár új
dolgot is megmutat.

.. _tut-morelists:

A listákról bővebben
==========================

A lista adattípusnak a már megismerteken kívül több metódusa (method) is van.
Az összes metódus ismertetése:

.. method:: list.append(x)

   Egy elemet hozzáad a lista végéhez; megegyezik az ``a[len(a):] = [x]``
   utasítással.

.. method:: list.extend(L)

   A lista végéhez hozzáfűzi az L listát (mindegyik elemét egyenként); ugyanaz,
   mint az ``a[len(a):] = L``.

.. method:: list.insert(i, x)

   Beszúr egy elemet az adott helyre. Az első argumentum az elem indexe, amely elé
   beszúrjuk, így  ``a.insert(0, x)`` a lista elejére szúr be, és az
   ``a.insert(len(a), x)`` ugyanazt jelenti mint az ``a.append(x)``.

.. method:: list.remove(x)

   Eltávolítja a legelső olyan elemet a listából, amelynek értéke ``x``. Hiba, ha
   nincs ilyen.


.. method:: list.pop([i])

   Eltávolítja az adott helyen lévő elemet a listából, és visszaadja az értékét. Ha
   nem adtunk meg indexet, akkor az ``a.pop()`` az utolsó elemmel tér vissza.
   (Ekkor is eltávolítja az elemet.) (A függvény-argumentum megadásánál használt
   szögletes zárójel azt jelenti, hogy a paraméter megadása tetszőleges, és nem
   azt, hogy a [] jeleket be kell gépelni az adott helyen. Ezzel a jelöléssel
   gyakran találkozhatsz a  Python Standard Library-ban (Szabványos Python
   könyvtárban) )

.. method:: list.index(x)

   Visszatér az első olyan elem indexével, aminek az értéke ``x``. Hiba, ha nincs
   ilyen.

   .. % Return the index in the list of the first item whose value is \var{x}.
   .. % It is an error if there is no such item.


.. method:: list.count(x)

   Visszaadja ``x`` előfordulásának a számát a listában.

.. method:: list.sort()

   Rendezi a lista elemeit. A rendezett lista az eredeti listába kerül.

.. method:: list.reverse()

   Megfordítja az elemek sorrendjét a listában - szintén az eredeti lista módosul.

Egy példa, amely tartalmazza a legtöbb metódust::

   >>> a = [66.25, 333, 333, 1, 1234.5]
   >>> print(a.count(333), a.count(66.25), a.count('x'))
   2 1 0
   >>> a.insert(2, -1)
   >>> a.append(333)
   >>> a
   [66.25, 333, -1, 333, 1, 1234.5, 333]
   >>> a.index(333)
   1
   >>> a.remove(333)
   >>> a
   [66.25, -1, 333, 1, 1234.5, 333]
   >>> a.reverse()
   >>> a
   [333, 1234.5, 1, 333, -1, 66.25]
   >>> a.sort()
   >>> a
   [-1, 1, 66.25, 333, 333, 1234.5]

Észrevehetted, hogy a metódusok, mint amilyen az ``insert``, ``remove``
vagy ``sort``, amelyek a listát módosítják, nem írtak ki visszatérési
értékeket -- ``None`` értékkel térnek vissza. [1]_ Ez tervezési elv
minden megváltoztatható Pythonbeli adatstruktúra esetén.

.. _tut-lists-as-stacks:

Lista használata veremként
--------------------------

.. sectionauthor:: Ka-Ping Yee <ping@lfw.org>


A lista metódusai megkönnyítik a lista veremként történő használatát
ahol az utolsó lerakott elemet vesszük ki először (,,utoljára be,
először ki", angol rövidítése LIFO). Ahhoz, hogy a verem tetejére egy
elemet adjunk, használjuk az :meth:`append` utasítást.

A lista legelső/legfelső elemének kivételéhez használjuk a :meth:`pop` utasítást
mindenféle index nélkül. Például::

   >>> stack = [3, 4, 5]
   >>> stack.append(6)
   >>> stack.append(7)
   >>> stack
   [3, 4, 5, 6, 7]
   >>> stack.pop()
   7
   >>> stack
   [3, 4, 5, 6]
   >>> stack.pop()
   6
   >>> stack.pop()
   5
   >>> stack
   [3, 4]

.. _tut-lists-as-queues:

A listák használata sorként (queue)
-----------------------------------

.. sectionauthor:: Ka-Ping Yee <ping@lfw.org>


A listát használhatjuk úgy is mint egy sort, ahol az elsőnek hozzáadott
elemet vesszük ki először ("first-in, first-out", FIFO), a listák mégsem
hatékonyak erre a célra. Míg a lista végéhez fűzés (append) és onnan
levétel (pop) az gyors, addig a lista elejére beszúrás (insert) és onnan
kiszedés lassú (mert az egész listát eggyel el kell tolni).

Egy sor implementálásához használjuk a :class:`collections.deque`
osztályt, amely úgy lett kialakítva, hogy mindkét oldalához gyorsan
hozzá lehessen fűzni és elvenni elemet. Például::

   >>> from collections import deque
   >>> sor = deque(["Marcsi", "Jani", "Misi"])
   >>> sor.append("Teri")           # Teri megérkezett
   >>> sor.append("Gergely")        # Gergely megérkezett
   >>> sor.popleft()                # Az első érkező távozik
   'Marcsi'
   >>> sor.popleft()                # A második érkező távozik
   'Jani'
   >>> sor                      # A maradék sor az érkezés sorrendjében
   deque(['Misi', 'Teri', 'Gergely'])


.. _tut-listcomps:

Listaértelmezés
-------------------

A listaértelmezés egy tömör lehetőség listák létrehozásához.
Gyakori alkalmazásuk, hogy új listát készítsünk, amelyben minden elemet
úgy hozunk létre, hogy egy másik sorozat vagy iterálható objektum minden
egyes tagján valamilyen műveletet végzünk, vagy az elemek egy
részhalmazát vesszük, amelyek bizonyos feltételeknek megfelelnek.

Például, ha a négyzetszámok listáját akarjuk létrehozni, mint alább::

   >>> squares = []
   >>> for x in range(10):
   ...     squares.append(x**2)
   ...
   >>> squares
   [0, 1, 4, 9, 16, 25, 36, 49, 64, 81]

Létrehozhatjuk így is::

   squares = [x**2 for x in range(10)]

Ez így is létrehozható: ``squares = list(map(lambda x: x**2, range(10)))``,
de az előbbi sokkal tömörebb és olvashatóbb.

A listaértelmezés egy zárójelből áll, amelyben egy kifejezést egy
:keyword:`for` ág követ, majd nulla vagy több :keyword:`for` vagy
:keyword:`if` ág. Az eredmény egy új lista lesz, melyet a kifejezés
kiértékelésével kapunk az azt követő :keyword:`for` és :keyword:`if`
ágak figyelembevételével.  Például az alábbi listaértelmezés kombinálja
két lista elemeit, amennyiben azok nem egyenlőek::

   >>> [(x, y) for x in [1,2,3] for y in [3,1,4] if x != y]
   [(1, 3), (1, 4), (2, 3), (2, 1), (2, 4), (3, 1), (3, 4)]

és ez egyenértékű az alábbival::

   >>> combs = []
   >>> for x in [1,2,3]:
   ...     for y in [3,1,4]:
   ...         if x != y:
   ...             combs.append((x, y))
   ...
   >>> combs
   [(1, 3), (1, 4), (2, 3), (2, 1), (2, 4), (3, 1), (3, 4)]

Vedd észre, hogy a :keyword:`for` és :keyword:`if` állítások sorrendje
azonos mindkét példában.

Ha a kifejezés egy tuple (pl. az ``(x, y)`` az előző példában), akkor
zárójelbe kell rakni. ::

   >>> vec = [-4, -2, 0, 2, 4]
   >>> # létrehoz egy listát az értékek megduplázásával
   >>> [x*2 for x in vec]
   [-8, -4, 0, 4, 8]
   >>> # szűri a listát a negatív értékek elhagyásával
   >>> [x for x in vec if x >= 0]
   [0, 2, 4]
   >>> # alkalmaz egy függvényt az összes elemre
   >>> [abs(x) for x in vec]
   [4, 2, 0, 2, 4]
   >>> # meghív egy metódust az összes elemre
   >>> freshfruit = ['  banana', '  loganberry ', 'passion fruit  ']
   >>> [weapon.strip() for weapon in freshfruit]
   ['banana', 'loganberry', 'passion fruit']
   >>> # 2 elemű tuple-ok listáját hozza létre, mint (szám, négyzete)
   >>> [(x, x**2) for x in range(6)]
   [(0, 0), (1, 1), (2, 4), (3, 9), (4, 16), (5, 25)]
   >>> # a tuple-nak zárójelben kell lennie, különben hiba lép fel
   >>> [x, x**2 for x in range(6)]
     File "<stdin>", line 1, in ?
       [x, x**2 for x in range(6)]
                  ^
   SyntaxError: invalid syntax
   >>> # egy lista kisimítható két for-t tartalmazó listaértelmezéssel
   >>> vec = [[1,2,3], [4,5,6], [7,8,9]]
   >>> [num for elem in vec for num in elem]
   [1, 2, 3, 4, 5, 6, 7, 8, 9]

Egy listaértelmezés összetett kifejezéseket és egymásba ágyazott
függvényeket is tartalmazhat::

   >>> from math import pi
   >>> [str(round(pi, i)) for i in range(1, 6)]
   ['3.1', '3.14', '3.142', '3.1416', '3.14159']

Egymásba ágyazott listaértelmezések
---------------------------------------

Egy listaértelmezés eredeti kifejezése tetszőleges kifejezés lehet, akár
egy másik listaértelmezés is.

Tekintsük a következő példát, amelyben egy 3x4-es mátrixot egy 4-elemű
listákból álló 3-elemű lista tárol::

   >>> matrix = [
   ...     [1, 2, 3, 4],
   ...     [5, 6, 7, 8],
   ...     [9, 10, 11, 12],
   ... ]

A következő listaértelmezés felcseréli a sorokat és az oszlopokat
(transzponálja a mátrixot)::

   >>> [[row[i] for row in matrix] for i in range(4)]
   [[1, 5, 9], [2, 6, 10], [3, 7, 11], [4, 8, 12]]

Ahogy ez előző fejezetben láttuk, az egymásba ágyazott listaértelmezés
az utána álló :keyword:`for` figyelembevételével értékelődik ki, tehát
ez a példa egyenértékű az alábbival::

   >>> transposed = []
   >>> for i in range(4):
   ...     transposed.append([row[i] for row in matrix])
   ...
   >>> transposed
   [[1, 5, 9], [2, 6, 10], [3, 7, 11], [4, 8, 12]]

amely pedig azonos az alábbival::

   >>> transposed = []
   >>> for i in range(4):
   ...     # az alábbi 3 sor felel meg a belső listaértelmezésnek
   ...     transposed_row = []
   ...     for row in matrix:
   ...         transposed_row.append(row[i])
   ...     transposed.append(transposed_row)
   ...
   >>> transposed
   [[1, 5, 9], [2, 6, 10], [3, 7, 11], [4, 8, 12]]

Az való életben persze ehelyett beépített függvényeket érdemes használni
összetett folyamatok helyett. A :func:`zip` függvény remek eredményt ad
ebben az esetben::

   >>> list(zip(*matrix))
   [(1, 5, 9), (2, 6, 10), (3, 7, 11), (4, 8, 12)]

Lásd :ref:`tut-unpacking-arguments` fejezetet a fenti sorban szereplő csillaggal kapcsolatban.

.. _tut-del:

A :keyword:`del` utasítás
=========================

Egy listaelem eltávolításának egyik módja, hogy az elem értéke helyett az
indexét adjuk meg: ez a :keyword:`del` utasítás.  Ez abban különbözik a
:meth:`pop` metódustól, hogy az egy értékkel tér vissza. A
:keyword:`del` utasítás arra is használható, hogy szeleteket töröljünk a
listából vagy kiürítsük az egész listát (amit már megtettünk korábban
úgy, hogy a szeletnek az üres lista értékét adtuk). Például::

   >>> a = [-1, 1, 66.25, 333, 333, 1234.5]
   >>> del a[0]
   >>> a
   [1, 66.25, 333, 333, 1234.5]
   >>> del a[2:4]
   >>> a
   [1, 66.25, 1234.5]

A :keyword:`del` utasítást arra is használhatjuk,  hogy az egész változót
töröljük::

   >>> del a

A továbbiakban hibát generál, ha az ``a`` névre hivatkozunk  (kivéve, ha új
értéket adunk neki).  Később a :keyword:`del` utasításnak más
alkalmazásával is találkozunk.

.. _tut-tuples:

Tuple-ok és sorozatok
=======================

Láttuk, hogy a listáknak és a karakterláncoknak rengeteg közös
tulajdonsága van, például az indexelés és a szeletek használata.  Ez két
példa a *sorozat*-adattípusra. Mivel a Python egy folytonos
fejlődésben lévő nyelv, másfajta sorozat adattípusok is hozzáadhatóak.
Létezik egy másik sorozat-adattípus a *tuple* (ejtsd tjupl).

A tuple-t angol formájában fogjuk használni, mivel nincs igazán jó
magyar megfelelője. Az irodalomban használják még a rendezett sorozat
elnevezést, de az abban szereplő rendezett jelző félrevezető.

A tuple objektumokat tartalmaz vesszőkkel elválasztva, például::

   >>> t = 12345, 54321, 'hello!'
   >>> t[0]
   12345
   >>> t
   (12345, 54321, 'hello!')
   >>> # A tuple-okat egymásba ágyazhatjuk:
   ... u = t, (1, 2, 3, 4, 5)
   >>> u
   ((12345, 54321, 'hello!'), (1, 2, 3, 4, 5))
   >>> # A tuple-ok megváltoztathatatlanok:
   ... t[0] = 88888
   Traceback (most recent call last):
     File "<stdin>", line 1, in <module>
   TypeError: 'tuple' object does not support item assignment
   >>> # de tartalmazhatnak megváltoztatható elemeket:
   ... v = ([1, 2, 3], [3, 2, 1])
   >>> v
   ([1, 2, 3], [3, 2, 1])

Ahogy látható, a kimeneten a tuple-ok mindig zárójelezve vannak,  így azok
egymásba ágyazva is helyesen értelmezhetők;  megadhatjuk zárójelekkel és anélkül
is, néha azonban feltétlenül szükségesek a zárójelek (amikor az egy nagyobb
kifejezés része). Nem lehetséges a tuple elemeinek új értéket adni,
de létrehozható olyan tuple, amelynek vannak megváltoztatható elemei,
például listák.

Habár a tuple-ok hasonlóaknak látszanak a listákhoz, gyakran eltérő
helyzetekben és különböző célokra használjuk azokat.
A tuple-ok :term:`megváltoztathatatlan` adattípusok, és gyakran azonos
típusú elemek sorozatát tárolja, amelyeket az úgynevezett szétpakolás
révén érjük el (lásd később ebben a fejezetben) vagy indexelés révén
(vagy akár jellemzőjük révén a :func:`namedtuples
<collections.namedtuple>` esetén). 
A listák :term:`megváltoztatható` adattípusok, és az elemeik gyakran
eltérő típusúak, és elmeiket általában úgy érjük el, hogy végigmegyünk
az elemeiken.

Egy különös probléma nulla vagy egy elemet tartalmazó tuple létrehozása:
a nyelv szintaxisa lehetővé teszi ezt. Az üres zárojellel hozható létre
a nulla elemű; az egy elemű pedig az érték után tett vesszővel  (nem
elég, ha az értéket zárójelbe tesszük). Csúnya, de hatékony. Például::

   >>> ures = ()
   >>> egyszeres = 'hello',    # <-- figyeljünk a vesszőre a végén
   >>> len(ures)
   0
   >>> len(egyszeres)
   1
   >>> egyszeres
   ('hello',)

A ``t = 12345, 54321, 'hello!'`` értékadás egy példa  a *tuple-ba
csomagolásra*: a ``12345``, ``54321`` és ``'hello!'`` értékek egy
tuple-ba kerülnek becsomagolásra. A fordított művelet is lehetséges,
például::

   >>> x, y, z = t

Ezt hívják, elég helyesen, *sorozat-szétpakolásnak*, és minden
jobboldalon álló sorozattípusra működik. A sorozat szétpakolásához az
szükséges,  hogy a bal oldalon annyi elem szerepeljen, ahány elem van a
tuple-ban.  Jegyezzük meg, hogy a többszörös értékadás valójában csak a
tuple-ba csomagolás és a sorozat-szétpakolás kombinációja!

.. _tut-sets:

A halmazok (set)
================

A Python a halmaz adattípust is tartalmazza. A halmaz elemek
rendezetlen halmaza, amelyben minden elem csak egyszer  fordulhat elő.
Alapvető használata: megadott elem meglétének ellenőrzése, elemek
kettőzésének kiszűrése.  A set objektumok támogatják az olyan
matematikai műveleteket, mint az egyesítés, metszet,  különbség, és a
szimmetrikus eltérés.

A halmazok létrehozására a kapcsos zárójel, vagy a :func:`set()`
függvény használható. Jegyezzük meg, hogy az üres halmaz létrehozásához
csak a  :func:`set()` használható, a `{}` nem, mert az utóbbi üres
szótárat hoz létre: egy olyan adattszerkezetet, amelyet a következő
fejezetben tárgyalunk.

Íme egy rövid bemutató::

   >>> kosar = {'alma', 'narancs', 'alma', 'körte', 'narancs', 'banán'}
   >>> print(kosar)        # a többször szereplő elemek csak egyszer szereplenek
   {'narancs', 'körte', 'alma', 'banán'}
   >>> 'narancs' in kosar            # gyors ellenőrzés: benne van-e
   True
   >>> 'kakukkfu' in kosar
   False

   >>> # Példa: set műveletek két szó egyedi betűin
   ...
   >>> a = set('abracadabra')
   >>> b = set('alacazam')
   >>> a                                  # 'a' egyedi elemei
   {'a', 'r', 'b', 'c', 'd'}
   >>> a - b                              # 'a'-ban megvan, b-ből hiányzik
   {'r', 'd', 'b'}
   >>> a | b                              # 'a'-ban, vagy 'b'-ben megvan
   {'a', 'c', 'r', 'd', 'b', 'm', 'z', 'l'}
   >>> a & b                              # 'a'-ban és 'b'-ben is megvan
   set(['a', 'c'])
   >>> a ^ b                              # vagy 'a'-ban, vagy 'b'-ben megvan, de 
   				          # egyszerre mindkettőben nem 
   {'r', 'd', 'b', 'm', 'z', 'l'}

A listaértelmezéshez hasonlóan létezik halmazértelmezés is::

   >>> a = {x for x in 'abracadabra' if x not in 'abc'}
   >>> a
   {'r', 'd'}

.. _tut-dictionaries:

Szótárak
========

Egy másik hasznos adattípus a Pythonban a *szótár*. A szótárakat más
nyelvekben ,,asszociatív tömböknek" nevezik.  Szemben a sorozatokkal --
amelyek számokkal vannak indexelve -- a tömböket *kulcsokkal*
indexeljük, amely mindenféle megváltoztathatatlan típus lehet;
karakterláncok és számok mindig lehetnek kulcsok.  Tuple-ok is
használhatók kulcsnak, ha csak számokat, karakterláncokat vagy tuple-okat
tartalmaznak; ha egy tuple  megváltoztatható objektumot tartalmaz --
közvetlenül vagy közvetve, akkor nem lehet kulcs.

Listát nem lehet kulcsként használni, mert annak értékei az  :meth:`append`,
:meth:`extend` metódusokkal, valamint a  szeletelő vagy indexelt értékadásokkal
(helyben) módosíthatók.

Gondoljunk úgy a szótárra, mint *kulcs: érték* párok rendezetlen halmazára,
azzal a megkötéssel, hogy a szótárban a kulcsoknak egyedieknek kell lenniük. Egy
kapcsos zárójelpárral egy üres szótárat hozhatunk létre: ``{}``.  Ha a zárójelbe
vesszőkkel elválasztott kulcs:érték párokból álló listát helyezünk, akkor ez
belekerül a szótárba;  egy szótár tartalma is ilyen módon jelenik meg a
kimeneten.

A legfontosabb műveletek egy szótáron: eltárolni egy értéket egy kulccsal
együtt, visszakapni egy értéket megadva a kulcsát.   Lehet törölni is egy
kulcs:érték párt a ``del``-lel. Ha olyan kulccsal tárolsz egy új értéket,
amilyen kulcsot  már használtál, a kulcshoz az új érték fog tartozni, a
régi érték elveszik.  Hiba, ha egy nemlétező kulcsra hivatkozol.

Egy szótáron a ``list(d.keys())`` végrehajtása a kulcsok listáját adja vissza
véletlenszerű sorrendben (ha rendezni akarod, csak használd a
``sorted(d.keys())`` formát). [2]_  Ha ellenőrizni szeretnéd, vajon egy kulcs
benne van-e a szótárban, használd az :keyword:`in` kulcsszót.

Íme egy kis példa a szótár használatára::

   >>> tel = {'János': 4098, 'Simon': 4139}
   >>> tel['Géza'] = 4127
   >>> tel
   {'Simon': 4139, 'Géza': 4127, 'János': 4098}
   >>> tel['János']
   4098
   >>> del tel['Simon']
   >>> tel['Pisti'] = 4127
   >>> tel
   {'István': 4127, 'Géza': 4127, 'János': 4098}
   >>> list(tel.keys())
   ['István', 'Géza', 'János']
   >>> sorted(tel.keys())
   ['Géza', 'István', 'János']
   >>> 'Géza' in tel
   True
   >>> 'István' not in tel
   False

A :func:`dict` konstruktor közvetlenül tuple-okban tárolt kulcs-érték párok
listájából is létre tudja hozni a szótárat. ::

   >>> dict([('sape', 4139), ('guido', 4127), ('jack', 4098)])
   {'sape': 4139, 'jack': 4098, 'guido': 4127}

Ezen felül a szótárértelmezés használható arra, hogy szótárat hozzunk
létre tetszőleges kulcs- és értékkifejezésekből::

   >>> {x: x**2 for x in (2, 4, 6)}
   {2: 4, 4: 16, 6: 36}

Ha a kulcsok egyszerű karakterláncok, néha egyszerűbb a párokat
kulcsszavas argumentumokkal létrehozni::

   >>> dict(sape=4139, guido=4127, jack=4098)
   {'sape': 4139, 'jack': 4098, 'guido': 4127}


.. _tut-loopidioms:

Ciklustechnikák
===============

Ha egy szótár kulcsain szeretnénk végigmenni, a for után egyszerűen a
szótárat írjuk be::


    >>> gyumolcsok = {1:'alma', 2:'körte', 3:'banán'}
    >>> for kulcs in gyumolcsok:
    ...     print(kulcs, gyumolcsok[kulcs])
    ... 
    1 alma
    2 körte
    3 banán

Ha végig szeretnénk menni egy szótár elemein, akkor az  :meth:`items`
metódussal lépésenként egyidőben megkapjuk a kulcsot, és a hozzá tartozó
értéket.  ::

   >>> lovagok = {'Gallahad': 'a tiszta', 'Robin': 'a bátor'}
   >>> for k, v in lovagok.items():
   ...     print(k, v)
   ...
   Gallahad a tiszta
   Robin a bátor

Ha sorozaton megyünk végig, akkor a helyet jelző index értékét és a hozzá
tartozó értéket egyszerre kaphatjuk meg az :func:`enumerate` függvénnyel. ::

   >>> for i, v in enumerate(['tic', 'tac', 'toe']):
   ...     print(i, v)
   ...
   0 tic
   1 tac
   2 toe

Két vagy több sorozat egyszerre történő feldolgozásához  a sorozatokat a
:func:`zip` függvénnyel kell párba állítani.

::

   >>> kerdesek = ['neved', 'csoda, amit keresel', 'kedvenc színed']
   >>> valaszok = ['Lancelot', 'A szent Grál', 'Kék']
   >>> for q, a in zip(kerdesek, valaszok):
   ...     print('Mi a %s?  %s.' % (q, a))
   ...	
   Mi a neved?  It is lancelot. Lancelot.
   Mi a csoda, amit keresel?  A szent Grál.
   Mi a kedvenc színed?  Kék.

Egy sorozaton visszafelé haladáshoz először add meg a sorozatot, majd utána hívd
meg a :func:`reversed` függvényt.

::

   >>> for i in reversed(range(1,10,2)):
   ...     print(i)
   ...
   9
   7
   5
   3
   1

Rendezett sorrendben való listázáshoz használd a :func:`sorted` függvényt, amely
új, rendezett listát ad vissza, változatlanul hagyva a régi listát.

::

   >>> kosar = ['alma', 'narancs', 'alma', 'körte', 'narancs', 'banán']
   >>> for gyum in sorted(set(kosar)):
   ...     print(gyum)
   ... 	
   alma
   banán
   körte
   narancs

.. _tut-conditions:

A feltételekről bővebben
==============================

A  ``while`` és az ``if`` utasításokban eddig használt feltételek egyéb
műveleteket is tartalmazhatnak az összehasonlítás mellett.

Az ``in`` és ``not in`` relációk ellenőrzik, hogy az érték előfordul-e egy
sorozatban. Az ``is`` és ``is not`` relációk összehasonlítják, hogy két dolog
valóban azonos-e; ez csak olyan változékony dolgoknál fontos, amilyenek például
a listák. Minden összehasonlító relációnak azonos precedenciája van, mely
magasabb mint a számokkal végzett műveleteké.

Relációkat láncolhatunk is, például: az  ``a < b == c`` megvizsgálja, hogy az
``a`` kisebb-e mint ``b``, és ezen felül  ``b`` egyenlő-e ``c``-vel.

A relációkat összefűzhetjük ``and`` és ``or`` logikai műveletekkel is, és a
reláció erdményét (vagy bármely logikai műveletét) ellentettjére változtathatjuk
a ``not`` művelettel. Ezeknek mindnek kisebb precedenciájuk van, mint a
relációknak, és közülük a ``not`` rendelkezik a legmagasabbal és az ``or`` a
legkisebbel. Tehát az ``A and not B or C`` ugyanazt jelenti, mint az ``(A and
(not B)) or C``. Természetesen a zárójeleket használhatjuk a kívánt feltétel
eléréséhez.

Az ``and`` és ``or`` logikai műveletek úgynevezett shortcat (lusta/rövid
kiértékelésű) műveletek: Az argumentumaik balról jobbra fejtődnek ki, és a
kifejtés rögtön megáll, mihelyt a végeredmény egyértelmű. Például: ha ``A`` és
``C`` mindkettő igaz de  ``B`` hamis, akkor a  ``A és B és C`` kifejezés során a
C értékét a Python már nem vizsgálja. Általában a shortcut műveletek
visszatérési értéke -- ha általános értékeket és nem logikai értéket használunk
-- az utolsónak kifejtett argumantummal egyezik.

Lehetséges, hogy egy reláció vagy más logikai kifejezés értékét egy változóba
rakjuk. Például::

   >>> string1, string2, string3 = '', 'Trondheim', 'Hammer Dance'
   >>> non_null = string1 or string2 or string3
   >>> non_null
   'Trondheim'

Jegyezzük meg, hogy a Pythonban -- szemben a C-vel -- nem lehet értékadás egy
kifejezés belsejében. Lehet, hogy a C programozók morognak emiatt, de így
kikerülhető egy gyakori probléma, amelyet a C programokban gyakran elkövetnek:
``=`` jelet írnak ott, ahol ``==`` kellene.

.. _tut-comparing:

Sorozatok és más típusok összehasonlítása
=========================================

Sorozat objektumokat összehasonlíthatunk másik azonos típusú objektummal. Az
összehasonlítás a *lexikografikai* rendezést használja: először az első elemeket
hasonlítja össze, ha ezek különböznek, ez meghatározza az összehasonlítás
eredményét; ha egyenlőek, akkor a második elemeket hasonlítja össze, és így
tovább, amíg az egyik sorozatnak vége nem lesz. Ha a két összehasonlítandó elem
azonos típusú sorozat, akkor az összehasonítás rekurzívan történik. Ha a két
sorozat minden eleme azonos, akkor tekinthetőek a sorozatok egyenlőeknek.  Ha sz
egyik sorozat a másiknak kezdő rész-sorozata, akkor a rövidebb sorozat a kisebb.
A karakterláncok lexikografikai elemzésére az egyes karakterek ASCII rendezését
alkalmazzuk.  Néhány példa azonos típusú sorozatok összehasonlítására::

   (1, 2, 3)              < (1, 2, 4)
   [1, 2, 3]              < [1, 2, 4]
   'ABC' < 'C' < 'Pascal' < 'Python'
   (1, 2, 3, 4)           < (1, 2, 4)
   (1, 2)                 < (1, 2, -1)
   (1, 2, 3)             == (1.0, 2.0, 3.0)
   (1, 2, ('aa', 'ab'))   < (1, 2, ('abc', 'a'), 4)

Jegyezzük meg, hogy szabályos a különböző típusú objektumok
összehasonlítása is  ``<`` és ``>`` relációkkal, feltéve, hogy a
megfelelő objektumoknak vannak megfelelő összehasonlító metódusai.
Például a vegyes számtípusok összehasonlíthatóak a számértékük alapján,
így 0 egyenlő 0.0-val, stb. Máskülönben, egy önkényes rendezés helyett,
az értelmező :exc:`TypeError` kivételt dob. 

.. rubric:: Lábjegyzet

.. [1] Más nyelvek visszaadhatják a megváltoztatott objektumok, amely
       lehetővé teszi a metódusok olyan láncolását, mint a következő
       kódban ``d->insert("a")->remove("b")->sort();``.

.. [2] A ``d.keys()`` metódus meghívása egy :dfn:`szótárnézet`
       objektummal tér vissza. Ez támogatja az olyan műveleteket, mint a
       tagság tesztelése vagy az iteráció, de a tartalma nem független
       az eredeti szótártól -- ez csak egy nézet.
