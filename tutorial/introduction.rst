.. _tut-informal:

****************************
Kötetlen bevezető a Pythonba
****************************

A következő példákban a kimenetet és a bemenetet az elsődleges és másodlagos
promptok (``>>>`` és ``...``)  meglétével, illetve hiányával
különböztetjük meg. A példák kipróbálásához mindent be kell írnod a
promptok után, amikor a prompt megjelenik; azok a sorok, amelyek előtt
nincs prompt, a fordító kimenetei.

Az önmagában álló másodlagos prompt a példa egyik sorában azt jelenti,
hogy csak egy Újsort kell ütni; ez jelzi egy többsoros utasítás végét.

Ennek a kézikönyvnek sok példájában -- még azokban is, amelyeket interaktív
módon írtunk be -- szerepelnek megjegyzések. A Pythonban a megjegyzések
kettőskereszttel (``'#'``) kezdődnek és a sor végéig tartanak. Egy
megjegyzés lehet sor elején, vagy követhet szóközt, tabulátor-karaktert,
de ha egy karakterlánc belsejébe teszed, az nem lesz megjegyzés (lásd a
példában!). A kettőskereszt karakter egy karakterláncon belül csak egy
kettőskeresztet jelent.

Pár példa::

   # ez az első megjegyzés
   SPAM = 1                 # ez a második megjegyzés
                            # ... és ez a harmadik.
   STRING = "# Ez nem megjegyzés, mert idézőjelekben van."


.. _tut-calculator:

A Python használata számológépként
==================================

Próbáljunk ki néhány Python utasítást. Indítsuk el az értelmezőt, és várjuk meg
az elsődleges promptot: ``>>>``. (Nem kell sokáig várni.)


.. _tut-numbers:

Számok
------

A parancsértelmező úgy működik, mint egy sima számológép: be lehet írni
egy kifejezést, és az kiszámolja az értékét. A kifejezések nyelvtana a
szokásos: a ``+``, ``-``, ``*`` és ``/`` műveletek ugyanúgy működnek,
mint a legtöbb nyelvben (például Pascal vagy C); zárójeleket (``()``)
használhatunk a csoportosításra.  Például::


   >>> 2 + 2
   4
   >>> 50 - 5*6
   20
   >>> (50 - 5*6) / 4
   5.0
   >>> 8 / 5  # az osztás egész számok esetén is lebegőpontos eredményt ad
   1.6

Az egész számok (pl. ``2``, ``4``, ``20``)  :class:`int` típusúak,
azok, amiknek törtrésze is van (e.g. ``5.0``, ``1.6``) a
:class:`float` típusba.  Az oktató további részében még többet tanulunk
meg a típusokról.

Az osztás (``/``) mindig lebegőpontos értékeket ad vissza.
Lefelé kerekítő :term:`egészosztás` elvégzéséhez, amellyel egész értéket
kapunk a ``//`` operátor használható; az osztás maradékához a ``%``::

   >>> 17 / 3  # hagyományos osztás lebegőpontos eredménnyel
   5.666666666666667
   >>>
   >>> 17 // 3  # az egészosztással megszabadulunk a törtrésztől
   5
   >>> 17 % 3  # a % operátor az osztás maradékával tér vissza
   2
   >>> 5 * 3 + 2  # eredmény * osztó + maradék
   17

A Pythonban a ``**`` operátor használható a hatvány kiszámítására [#]_::

   >>> 5 ** 2  # 5 négyzete
   25
   >>> 2 ** 7  # 2 7-dik hatványa
   128

A C-hez hasonlóan az egyenlőségjellel (``'='``) lehet értéket adni egy
változónak. Az értékadás után az értelmező újabb utasításra vár, látszólag nem
történik semmi::

   >>> szelesseg = 20
   >>> magassag = 5*9
   >>> szelesseg * magassag
   900

Ha egy változó nincs definiálva (nincs érték rendelve hozzá), és
használni próbáljuk, akkor hibaüzenetet kapunk::

   >>> n  # try to access an undefined variable
   Traceback (most recent call last):
     File "<stdin>", line 1, in <module>
   NameError: name 'n' is not defined

A programnyelv teljeskörűen támogatja a lebegőpontos számokat; azok a műveletek
amelyeknél keverednek a típusok, az egészeket lebegőpontossá alakítják::

   >>> 3 * 3.75 / 1.5
   7.5
   >>> 7.0 / 2
   3.5

(A korábbi verziók oktatójában szereplő leírást a komplex számokról bent
hagytuk ebben a fordításban is. -- a fordító)

A Python komplex számokat is tud kezelni -- a képzetes részt a ``j`` vagy ``J``
jellel képezhetjük. A komplex számot ``(valós+képzetesj)`` formában írhatjuk::

   >>> 1j * 1J
   (-1+0j)
   >>> 3+1j*3
   (3+3j)
   >>> (3+1j)*3
   (9+3j)
   >>> (1+2j)/(1+1j)
   (1.5+0.5j)

A komplex számokat gyakran két lebegőpontos számmal ábrázolják:  a
képzetes és a valós résszel. A *z* komplex számnak ezeket a részeit a
``z.real`` és ``z.imag`` utasításokkal olvashatjuk vissza. ::

   >>> a=1.5+0.5j
   >>> a.real
   1.5
   >>> a.imag
   0.5

A lebegőpontos és egész típusú konverziós függvények (:func:`float` és
:func:`int`)  nem működnek komplex  számokra. A komplex-valós
átalakításnak több lehetségesmódja is van: ahhoz, hogy egy komplex
számból valósat csinálj, használd az ``abs(z)`` utasítást, hogy megkapd
a nagyságát (lebegőpontosként) vagy a ``z.real`` utasítást, ha a valós
része kell.  ::

   >>> a=3.0+4.0j
   >>> float(a)
   Traceback (most recent call last):
     File "<stdin>", line 1, in ?
   TypeError: can't convert complex to float; use abs(z)
   >>> a.real
   3.0
   >>> a.imag
   4.0
   >>> abs(a)  # sqrt(a.real**2 + a.imag**2)
   5.0
   >>>
   >>> float(8) # lebegopontos alakra konvertal. kimenete: 8.0

Interaktív módban az utoljára kiírt kifejezés értéke a ``_`` (alsóvonás)
változóban van. Így, ha a Pythont asztali számológépként használod, akkor
egyszerűbb folytatni a számolásokat, például::

   >>> ado = 12.5 / 100
   >>> ar = 100.50
   >>> ar * ado
   12.5625
   >>> ar + _
   113.0625
   >>> round(_, 2)
   113.06
   >>>

Ezt a változót csak olvasható változóként kezelhetjük. Ne adjunk értéket
neki, mert ha adunk, akkor létrehozunk egy független helyi változót
azonos névvel, amely meggátolja a beépített változó elérését, amely
mágikus módon viselkedik.  (Ha egy globális változó nevével létrehozunk
egy helyi változót, akkor az értelmező a helyit használja.)

Az :class:`int` és :class:`float` osztályokon felül a Python támogat
egyéb typusokat is, mint például a :class:`~decimal.Decimal` és
:class:`~fractions.Fraction`.
A Python beépített támogatással rendelkezik a :ref:`komplex számok
<typesnumeric>` tekintetében, és a  ``j`` vagy ``J`` utótagot hasznája a
képzetes rész jelölésére (pl. ``3+5j``, ``1j``).

.. _tut-strings:

Karakterláncok
--------------

A számok mellett a Python karakterláncokkal is tud műveleteket végezni.
A karakterláncokat egyszeres (``'...'``) vagy dupla idézőjelek
(``"..."``) közé lehet zárni. A két jelölés között nincs jelentős
különbség [#]_. A ``\`` használható arra, hogy a karakterláncbeli
idézőjeleket levédjük::

   >>> 'spam eggs'
   'spam eggs'
   >>> 'doesn\'t'
   "doesn't"
   >>> "doesn't"
   "doesn't"
   >>> '"Yes," he said.'
   '"Yes," he said.'
   >>> "\"Yes,\" he said."
   '"Yes," he said.'
   >>> '"Isn\'t," she said.'
   '"Isn\'t," she said.'

Az interaktív parancsértelmezőben a kimeneti karakterlánc idézőjelekben
jelenik meg, és a speciális karakterek vissza-perrel (``\``) levédve.
Bár ez néha különbözőnek látszik a bemenettől (az idézőjel fajtája
megváltozhat), a két karakterlánc egyenértékű. A karakterlánc dupla
idézőjelbe (``"``) van zárva, ha a karakterlánc tartalmaz egyszeres
idézőjelet, és duplát nem különben egyszeres idézőjelben van.  A
:func:`print` függvény egy sokkal olvashatóbb kimenetet eredményez,
elhagyva az idézőjeleket és kiírva a levédett és speciális
karaktereket::

   >>> '"Isn\'t," she said.'
   '"Isn\'t," she said.'
   >>> print('"Isn\'t," she said.')
   "Isn't," she said.
   >>> s = 'First line.\nSecond line.'  # \n újsort jelent
   >>> s  # print() nélkül a \n benne van a kimenetben
   'First line.\nSecond line.'
   >>> print(s)  # print()-tel az \n újsort hoz létre
   First line.
   Second line.

Ha nem akarod, hogy a ``\`` jellek kezdődő karakterek speciális
karakterként értelmeződjenek, akkor *nyers karakterláncokat*
használhatsz egy ``r``-et helyezve a kezdő idézőjel elé::

   >>> print('C:\some\name')  # \n újsort jelent
   C:\some
   ame
   >>> print(r'C:\some\name')  # r van az idézőjel előtt
   C:\some\name

A literális karakterláncok többsorosak is lehetnek. Egy lehetőség erre
a hármas idézőjelek használata: ``"""..."""`` vagy ``'''...'''``.
Ilyenkor az újsorok automatikusan belekerülnek a karakterláncba, de ez a
viselkedés megszüntethető, ha a sor végére egy ``\``-t adsz. A következő
példa ::

   print("""\
   Usage: thingy [OPTIONS] 
        -h                        Display this usage message
        -H hostname               Hostname to connect to
   """)

az alábbi kimenetet adja (vedd észre, hogy az első sorban nincs soremelés):

.. code-block:: text

   Usage: thingy [OPTIONS] 
        -h                        Display this usage message
        -H hostname               Hostname to connect to

Karakterláncokat a ``+`` művelettel ragaszthatunk össze és ``*``-gal
ismételhetünk. ::

   >>> # 3-szor 'un' után 'ium'
   >>> 3 * 'un' + 'ium'
   'unununium'

Két egymást követő literális karakterláncot (azokat, amik idézőjelben
vannak, és nem egy változóban, vagy nem egy függvény hoz létre) az
értelmező magától összefűz::

   >>> 'Py' 'thon'
   'Python'

De ez csak két literálissal működik, változóval vagy kifejezéssel nem::

   >>> prefix = 'Py'
   >>> prefix 'thon'  # nem tud változót és literális karakterláncot összefűzni
     ...
   SyntaxError: invalid syntax
   >>> ('un' * 3) 'ium'
     ...
   SyntaxError: invalid syntax

Ha változókat akarsz összefűzni, vagy változót literálissal, használj
``+`` műveletet::

   >>> prefix + 'thon'
   'Python'

A literálisok összefűzése különösen hasznos hosszú sorok széttörésére::

   >>> text = ('Több karakterláncot írhatunk zárójelbe, '
               'hogy összefűzhessük azokat.')
   >>> text
   'Több karakterláncot írhatunk zárójelbe, hogy összefűzhessük azokat.'


A karakterláncokat *indexelhetjük*, az első karakterhez tartozik a 0
index, a következőhöz az 1-es index és így tovább. Nincs külön karakter
típus; egy karakter egyszerűen egy egy hosszúságú karakterlánc::

   >>> szo = 'Python'
   >>> szo[0]  # karakter a 0 pozícióban
   'P'
   >>> szo[5]  # karakter a 5 pozícióban
   'n'

Az indexek negatívak is lehetnek, ilyenkor jobbról kezdünk el számolni::

   >>> szo[-1]  # utolsó karakter
   'n'
   >>> szo[-2]  # utolsó előtti karakter
   'o'
   >>> szo[-6]
   'P'

Az indexelésen felül a *szeletelés* is támogatott. Míg az indexelés
egyetlen karaktert jelöl ki, a *szeletelés* egy rész-karakterláncot::

   >>> szo[0:2]  # karakterek a 0 pozíciótól (benne van) a 2-esig (az már nem)
   'Py'
   >>> szo[2:5]  # karakterek a 2 pozíciótól (benne van) a 5-esig (az már nem)
   'tho'

A kezdő index mindig beleértendő az eredménybe, a végső index pedig nem.
Ez teszi lehetővé, hogy ``s[:i] + s[i:]`` mindig egyenlő ``s``-el::

   >>> szo[:2] + szo[2:]
   'Python'
   >>> szo[:4] + szo[4:]
   'Python'

A szeletek indexeinek hasznos alapértékei vannak; a kihagyott első index
alapértéke 0, az elhagyott második index alapértéke a szeletelendő
karakterlánc hossza::

   >>> szo[:2]  # karakterek az elejétől a 2-esig (már nincs benne)
   'Py'
   >>> szo[4:]  # karekterek a 4-es pozíciótól (benne van) a végéig
   'on'
   >>> szo[-2:] # karakterek az utolsó előttitől (beleértve) a végéig
   'on'

Jegyezd meg, hogy a -0 valóban azonos a 0-val, így ez nem jobbról
számol! ::

   >>> szo[-0]     # mivel -0 és 0 egyenlőek
   'P'


..
        Úgy a legkönnyebb megjegyezni hogy működnek a szeletek, ha azt képzeljük, hogy
        az indexek  a karakterek *közé* mutatnak, az első karakter bal élét számozzuk
        nullának. Ekkor az *n* karakterből álló karakterlánc utolsó karakterének jobb
        éle az *n*, például::

             +---+---+---+---+---+---+ 
             | S | e | g | í | t | s |
             +---+---+---+---+---+---+ 
             0   1   2   3   4   5   6
            -6  -5  -4  -3  -2  -1

        Az első sorban álló számok adják a 0...5 indexeket a karakterláncban; a második
        sor mutatja a megfelelő negatív indexeket. Az *i*-től *j*-ig terjedő szelet
        mindazokat a karaktereket tartalmazza, amelyek az *i* és *j* jelű élek között
        vannak.

A nem negatív indexek esetén a szelet hossza az indexek különbségével egyenlő,
ha mindkettő a valódi szóhatárokon belül van. Például a ``szo[1:3]`` hossza 2.

Ha olyan indexet használunk, amely túl nagy, az eredmény hibát ad::

   >>> szo[42]  # a szo csak 7 karakteres
   Traceback (most recent call last):
     File "<stdin>", line 1, in <module>
   IndexError: string index out of range

Ellenben a tartományon kívüli indexek a szeletekben rugalmasan kezeli a
Python nyelv::

   >>> word[4:42]
   'on'
   >>> word[42:]
   ''

A Python karakterláncait nem lehet megváltoztatni azok
:term:`megváltoztathatatlanok`.  Ezért, ha egy adott indexű helyhez
értéket rendelünk, hibát kapunk::

   >>> szo[0] = 'J'
     ...
   TypeError: 'str' object does not support item assignment
   >>> szo[2:] = 'py'
     ...
   TypeError: 'str' object does not support item assignment

If you need a different string, you should create a new one::
Ha másik karakterláncra van szükség, alkothatunk egy újat::

   >>> 'J' + szo[1:]
   'Jython'
   >>> szo[:2] + 'py'
   'Pypy'

A beépített :func:`len` függvény a karakterlánc hosszával tér vissza::

   >>> s = 'legeslegelkáposztásíthatatlanságoskodásaitokért'
   >>> len(s)
   47


.. seealso::

   :ref:`textseq`
      Strings are examples of *sequence types*, and support the common
      operations supported by such types.

   :ref:`string-methods`
      Strings support a large number of methods for
      basic transformations and searching.

   :ref:`string-formatting`
      Information about string formatting with :meth:`str.format` is described
      here.

   :ref:`old-string-formatting`
      The old formatting operations invoked when strings and Unicode strings are
      the left operand of the ``%`` operator are described in more detail here.


.. _tut-lists:

Listák
------

A Python többfajta *összetett* adattípust ismer, amellyel több különböző értéket
csoportosíthatunk. A legsokoldalúbb a *lista*, amelyet vesszőkkel elválasztott
értékekként írhatunk be szögletes zárójelbe zárva. A lista elemeinek nem kell
azonos típusúaknak lenniük, bár gyakran minden elem azonos típusú. ::

   >>> a = ['spam', 'tojások', 100, 1234]
   >>> a
   ['spam', 'tojások', 100, 1234]


Ahogy a karakterláncokat (és minden más beépített
:term:`sorozattípust`), a listákat is indexelhetjük és szeletelhetjük::

   >>> a[0]
   'spam'
   >>> a[3]
   1234
   >>> a[-2]
   100
   >>> a[1:-1]   # a szeletelés új listát ad
   ['tojások', 100]

A listák az összefűzést is támogatják::

   >>> a[:2] + ['sonka', 2*2]
   ['spam', 'tojások', 'sonka', 4]
   >>> 3*a[:3] + ['Boe!']
   ['spam', 'tojások', 100, 'spam', 'tojások', 100, 'spam', 'tojások', 100, 'Boe!']

A karakterláncokkal ellentétben -- amelyek *megváltoztathatatlanok* -- a listák
egyes elemeit módosíthatjuk::

   >>> a
   ['spam', 'tojások', 100, 1234]
   >>> a[2] = a[2] + 23
   >>> a
   ['spam', 'tojások', 123, 1234]

A szeleteknek értékeket is adhatunk és ez akár a lista elemszámát is
megváltoztathatja::

   >>> # Pár elem átírása:
   ... a[0:2] = [1, 12]
   >>> a
   [1, 12, 123, 1234]
   >>> # Pár elem törlése:
   ... a[0:2] = []
   >>> a
   [123, 1234]
   >>> # Pár elem beszúrása:
   ... a[1:1] = ['bletch', 'xyzzy']
   >>> a
   [123, 'bletch', 'xyzzy', 1234]
   >>> a[:0] = a     # Beszúrja magát (pontosabban egy másolatát) a saját elejére.
   >>> a
   [123, 'bletch', 'xyzzy', 1234, 123, 'bletch', 'xyzzy', 1234]

A beépített :func:`len` függvény listákra is alkalmazható::

   >>> len(a)
   8

A listák egymásba ágyazhatóak, azaz listába elhelyezhetünk listát
elemként::

   >>> a = ['a', 'b', 'c']
   >>> n = [1, 2, 3]
   >>> x = [a, n]
   >>> x
   [['a', 'b', 'c'], [1, 2, 3]]
   >>> x[0]
   ['a', 'b', 'c']
   >>> x[0][1]
   'b'


.. _tut-firststeps:

Első lépések a programozás felé
===============================

Természetesen a Pythont sokkal összetettebb feladatokra is használhatjuk annál,
minthogy kiszámoljuk 2+2 értékét.  Például írhatunk egy rövid ciklust a
Fibonacci-sorozat kiiratására::

   >>> # Fibonacci-sorozat:
   ... # az előző két elem összege adja a következőt
   ... a, b = 0, 1
   >>> while b < 10:
   ...       print(b)
   ...       a, b = b, a+b
   ... 
   1
   1
   2
   3
   5
   8

Ebben a példában a Python több új tulajdonságát megtaláljuk:

* Az első sor egy *többszörös értékadást* tartalmaz: ``a`` és ``b`` egyszerre
  veszi fel a 0 és 1 értékeket. Az utolsó sorban újból ezt használjuk, hogy
  megmutassuk, hogy előbb a jobboldal értékelődik ki, és csak azután megy végbe az
  értékadás. A jobboldali kifejezések jobbról balra értékelődnek ki.

* A :keyword:`while` ciklus addig hajtódik végre, amíg a feltétel (itt: ``b <
  10``) igaz marad.  A Pythonban -- ahogy a C-ben is -- minden nullától eltérő
  egész érték igazat, a nulla hamisat jelent.  A feltétel lehet egy karakterlánc
  vagy egy lista (gyakorlatilag bármilyen sorozat): minden aminek nem nulla a
  hossza -- igaz, az üres sorozatok hamisak.  A példában használt feltétel egy
  egyszerű összehasonlítás.  A legalapvetőbb összehasonlító relációkat a C-vel
  azonosan jelöljük: ``<`` (kisebb mint), ``>`` (nagyobb mint), ``==``
  (egyenlőek), ``<=`` (kisebb vagy egyenlő), ``>=`` (nagyobb vagy egyenlő) és
  ``!=`` (nem egyenlő).

* A ciklus *magját beljebb húzzuk*: a behúzás a Python jelölése az
  utasítások csoportosítására.  A Python alapértelmezett interaktív
  parancsértelmezőjében neked kell (néhány) szóközt beírnod minden
  behúzott sor elé.  Gyakorlatban az összetettebb programkódokat úgyis
  szövegszerkesztővel fogod elkészíteni, a legtöbb szövegszerkesztőnek
  van eszköze az automatikus behúzásra.  Ha egy összetett utasítást
  írunk be párbeszédes (interaktív) módban, azt egy üres sornak kell
  követnie (mivel az értelmező nem tudja kitalálni, lesz-e még újabb
  sor).  Jegyezd meg, hogy minden sort ugyanannyival kell beljebb húzni.
  (Az IDLE integrált fejlesztői környezetben található parancsértelmező
  és az ipython nevű parancsértelmező automatikusan behúzza a
  ciklusmagot.)

* A :func:`print` függvény kiírja annak a kifejezésnek az értékét, amelyet
  megadtunk.  Ez abban különbözik attól, mintha a kifejezést csak önmagában írnánk
  be (mint ahogy számológépként használtuk), ahogy a többszörös
  argumentumokat és a karakterláncokat kezeli.  A karakterláncokat
  idézőjelek nélkül írja ki, és szóközöket illeszt az egyes tagok közé,
  így széppé teheted a kimenetet::

     >>> i = 256*256
     >>> print('Az i értéke:', i)
     Az i értéke: 65536

  Az *end* kulcsszavas argumentum használható arra, hogy megakadályozzuk
  a print után az újsor kiírását, vagy mást írjunk ki a print végén::

     >>> a, b = 0, 1
     >>> while b < 1000:
     ...     print(b, end=",")
     ...     a, b = b, a+b
     ...
     1,1,2,3,5,8,13,21,34,55,89,144,233,377,610,987,


.. rubric:: Lábjegyzetek

.. [#] Mivel a ``**`` nagyobb precedenciájú, mint a ``-``, a ``-3**2`` mint
   ``-(3**2)`` fog értelmezésre kerülni, így az eredmény ``-9``.  Hogy
   ezt elkerüljük, és ``9``-et kapjunk, ``(-3)**2`` formában kell írni.

.. [#] Más nyelvekkel szemben a speciális karakterek, mint a  ``\n``
   azonos jelentésűek mind egyszeres (``'...'``) mind dupla (``"..."``)
   idézőjelben.
   Az egyetlen különbség a kettő között, hogy az egyszeres idézőjelben
   nem kell levédeni a ``"`` jelet (de a ``\'`` jelet igen) és viszont.
