.. _tut-brieftourtwo:

***********************************
Az alap-könyvtár bemutatása 2. rész
***********************************

Ebben a részben néhány olyan modult vizsgálunk meg, amire a professzionális
programozás során szükség lesz. Ezen modulok kisebb szkriptekben ritkán
fordulnak elő.

.. _tut-output-formatting:

A kimenet formázása
===================

A :mod:`repr` modulban található :func:`repr` függvény lehetővé teszi a
nagyméretű, mélyen egymásba  ágyazott adatszerkezetek rövid,
áttekinthető kijelzését::

   >>> import repr   
   >>> repr.repr(set('elkelkaposztastalanitottatok'))
   "set(['a', 'e', 'i', 'k', 'l', 'n', ...])"

A :mod:`pprint` modullal finoman szabályozhatod beépített és a
felhasználó által definiált objektumok megjelenítését -- úgy, hogy az az
értelmező számára továbbra is feldolgozható marad. Amikor az eredmény
nem fér el egy sorban, egy "csinos nyomtató" sortöréseket és behúzásokat
ad a kimenethez, hogy az jól olvasható legyen::

   >>> import pprint
   >>> t = [[[['black', 'cyan'], 'white', ['green', 'red']], [['magenta',
   ...     'yellow'], 'blue']]]
   ...
   >>> pprint.pprint(t, width=30)
   [[[['black', 'cyan'],
      'white',
      ['green', 'red']],
     [['magenta', 'yellow'],
      'blue']]]

A :mod:`textwrap` modullal szövegblokkokak jeleníthetünk meg adott
szélességű blokkban::

   >>> import textwrap
   >>> doc = """The wrap() method is just like fill() except that it returns
   ... a list of strings instead of one big string with newlines to separate
   ... the wrapped lines."""
   ...
   >>> print(textwrap.fill(doc, width=40))
   The wrap() method is just like fill()
   except that it returns a list of strings
   instead of one big string with newlines
   to separate the wrapped lines.

A :mod:`locale`  modul a különböző kultúrákhoz kötődő egyedi
adatformázásokhoz fér hozzá -- a locale format funkciójának  grouping
(csoportosítás, a következő példában helyiérték) tulajdonsága
közvetlenül biztosítja az adott kultúrának megfelelő szám-kijelzést::

   >>> import locale
   >>> locale.setlocale(locale.LC_ALL, 'English_United States.1252')
   'English_United States.1252'
   >>> conv = locale.localeconv()          # get a mapping of conventions
   >>> x = 1234567.8
   >>> locale.format("%d", x, grouping=True)
   '1,234,567'
   >>> locale.format("%s%.*f", (conv['currency_symbol'],
   ...	      conv['int_frac_digits'], x), grouping=True)
   '$1,234,567.80'


.. _tut-templating:

Szöveg-sablonok
===============

A :mod:`string` modulban található egy nagyon hasznos osztály, a
:class:`Template`. Ez lehetőséget ad a végfelhasználóknak
sablon-szövegek  szerkesztésére.

A szövegbe adatmezőket a ``$`` jellel helyezhetünk el, melyek mellé  kapcsos
zárójelbe Python változóneveket kell írni (ez számot, betűt és alsóvonás
karaktert tartalmazhat). A kapcsos zárójelpárra akkor van szükség, ha nem önálló
szó a beillesztett adat, például lent a 'Peternek' szó lesz ilyen - egyébként a
zárójelpár elhagyható. Egyszerű ``$`` jelet így írhatsz: ``$$`` ::

   >>> from string import Template
   >>> t = Template('${nev}nek kuldunk  10$$-t, hogy $cselekedet.')
   >>> t.substitute(nev='Peter', cselekedet='csizmat vegyen')
   'Peternek kuldunk 10$-t, hogy csizmat vegyen'

A :meth:`substitute` medódus :exc:`KeyError` kivételt dob, ha az adatmezőkben
megadott változónevet a paraméterként átadott szótárban, vagy  kulcsszavas
paraméterekben nem találja.  Elképzelhető olyan helyzet, hogy valamelyik
változónév (vagy kulcs, ha szótárról van szó) hiányzik - ilyenkor a
:meth:`safe_substitute` metódust érdemes használni,  ami a hiányzó adatoknál az
adatmezőt változatlanul hagyja::

   # ez a pelda nem fut le, hianyzik $owner
   >>> t = Template('Return the $item to $owner.')
   >>> d = dict(item='unladen swallow')
   >>> t.substitute(d)
   Traceback (most recent call last):
     . . .
   KeyError: 'owner'
   >>> t.safe_substitute(d)    # ez a pelda lefut, pedig hianyzik $owner:browse confirm saveas

   'Return the unladen swallow to $owner.'

A Template alosztály egyedi határolójelet is tud használni. Például  egy tömeges
fájl-átnevező funkció esetében (pl. fénykép-karbantartó programnál) az adatmezők
jelzésére használhatod a százalék jelet is, az aktuális dátum,  a kép sorszáma,
vagy a fájl formátumának jelzése esetén::

   >>> import time, os.path
   >>> photofiles = ['img_1074.jpg', 'img_1076.jpg', 'img_1077.jpg']
   >>> class BatchRename(Template):
   ...     delimiter = '%'
   >>> fmt = raw_input('Add meg az atnevezes modjat: (%d-datum %n-fajl_sorszama %f-fileformatum):  ')
   Add meg az atnevezes modjat: (%d-datum %n-fajl_sorszama %f-fileformatum):  Ashley_%n%f

   >>> t = BatchRename(fmt)
   >>> date = time.strftime('%d%b%y')
   >>> for i, filename in enumerate(photofiles):
   ...     base, ext = os.path.splitext(filename)
   ...     newname = t.substitute(d=date, n=i, f=ext)
   ...     print('%s --> %s' % (filename, newname))

   img_1074.jpg --> Ashley_0.jpg
   img_1076.jpg --> Ashley_1.jpg
   img_1077.jpg --> Ashley_2.jpg

Another application for templating is separating program logic from the details
of multiple output formats.  The makes it possible to substitute custom
templates for XML files, plain text reports, and HMTL web reports.

A szövegsablonok másik felhasználási lehetősége a többféle kimeneti formátumot
támogató programokban van. Itt a vezérlési logika a kimenettől el van választva
-- A kimeneti fájl felépítése más XML fájloknál, szövegfájloknál, vagy html
kimenet esetében, viszont az adattartalom értelemszerűen megegyezik.

.. _tut-binary-formats:

Bináris adatblokkok használata
==============================

A :mod:`struct` modulban található a :func:`pack` és az :func:`unpack`
függvények, melyekkel  változó hosszúságú bináris adatblokkokat
kezelhetsz.  A következő példa bemutatja a ZIP fájlok fejléc
információinak feldolgozását (a ``"H"`` és az ``"L"`` kódcsomag
jelképezi a két és négybájtos  előjel nélküli számokat)::

   import struct

   data = open('fajlom.zip', 'rb').read()
   start = 0
   for i in range(3):                      # megnezzuk az elso harom fajl fejlecet
       start += 14
       fields = struct.unpack('LLLHH', data[start:start+16])
       crc32, comp_size, uncomp_size, filenamesize, extra_size =  fields

       start += 16
       filename = data[start:start+filenamesize]
       start += filenamesize
       extra = data[start:start+extra_size]
       print(filename, hex(crc32), comp_size, uncomp_size)

       start += extra_size + comp_size     # tovabblepes a kovetkezo fejlechez

.. _tut-multi-threading:

Többszálúság
============

A szálkezeléssel lehet az egymástól sorrendileg nem függő folyamatokat
párhuzamossá tenni. A szálakkal egyidőben fogadhatjuk a program felhasználójának
utasításait, miközben  a háttérben a program egy feladaton dolgozik - a két
folyamat (kommunikáció, és háttérben munka)  egymással párhuzamosan fut.

A következő kód bemutatja a  :mod:`threading` modul magasszintű
használatát, ami egy külön háttérfolyamatot indít, miközben a program
fut tovább::

   import threading, zipfile

   class AsyncZip(threading.Thread):
           def __init__(self, infile, outfile):
               threading.Thread.__init__(self)        
               self.infile = infile
               self.outfile = outfile
           def run(self):
               f = zipfile.ZipFile(self.outfile, 'w', zipfile.ZIP_DEFLATED)
               f.write(self.infile)
               f.close()
               print('Kesz a zip tomoritese ennek a fajlnak: ', self.infile)

   background = AsyncZip('valami.txt', 'myarchive.zip')
   background.start()
   print('A foprogram tovabb fut az eloterben.')

   background.join()    # Varakozas a hattermuvelet befejezesere
   print('A foprogram megvarja, hogy a hattermuvelet befejezodjon.')


A többszálú programok egyik legfontosabb feladata azon szálak működésének
koordinálása, melyek megosztott adatokon, vagy közös erőforrásokon dolgoznak.
(pl. mi történik ha két szál egyidőben akar egy fájlt írni?) A Threading modul
több szinkronizációs elemet tartalmaz -- ilyen a zárolás, az eseménykezelés, a
feltételes változók és a szemaforok.

Igaz ugyan, hogy ezek hatékony eszközök -- ám előfordulhatnak kisebb tervezési
hibák is, melyek nehezen megismételhetők, és nehezen kinyomozhatók. Mivel a
szálak egymástól függetlenül futnak, még végrehajtásuk sorrendje sem biztos -
emiatt előfordulhat, hogy a program nem ugyanúgy viselkedik, ha egymás után
többször lefuttatjuk -- így a hibakeresés is nehezebbé válik.

A szálak koordinálására azt javasoljuk, hogy az erőforrások elérését egy szál
biztosítsa, és használd a :mod:`Queue` modult a többi szálból érkező
kérések kezelésére. Ha egy programban a szálak közötti kommunikációt a
:class:`Queue` objektumokkal biztosítod, a program koordinálásának
megtervezése egyszerűbb, olvashatóbb és megbízhatóbb lesz.

.. _tut-logging:

Naplózás
========

A :mod:`logging` modul egy összetett, finoman beállítható naplózó
rendszert tartalmaz.  A legegyszerűbb esetben a naplózandó üzenetek
fájlba, vagy a ``sys.stderr`` (szabványos hibakimenetre) - küldhetők::

   import logging
   logging.debug('Debugging information')
   logging.info('Informational message')
   logging.warning('Warning:config file %s not found', 'server.conf')
   logging.error('Error occurred')
   logging.critical('Critical error -- shutting down')

A fenti példa kimenete::

   WARNING:root:Warning:config file server.conf not found
   ERROR:root:Error occurred
   CRITICAL:root:Critical error -- shutting down

Alapértelmezés szerint az információs és debug üzenetek elfojtottak,  és a
kimenet a szabványos hiba csatornára kerül. A kimenet célja más is lehet,
például email, datagram, socket vagy akár egy HTTP szerver.  Az új szűrők az
üzenet prioritásától függően más-más kimenetre terelhetik  a naplózandó
üzenetet. A prioritások:  :const:`DEBUG`, :const:`INFO`, :const:`WARNING`,
:const:`ERROR`, és :const:`CRITICAL`.

A naplózó rendszer közvetlenül a Pythonból is beállítható, vagy használhatsz
konfigurációs fájlt, s így a programból való kilépés nélkül megváltoztathatod a
naplózás beállításait.

.. _tut-weak-references:

Gyenge hivatkozások
===================

A memóriakezelést a Python automatikusan végzi (hivatkozásszámlálás a legtöbb
objektum esetében, és szemétgyűjtés). Az utolsó objektum-hivatkozás
megsemmisülése után az elfoglalt memória felszabadul.

Ez az automatizmus a legtöbb esetben jó és hasznos, de néha szükség van  az
objektumok követésére mindaddig, amíg használatban vannak. Érdekes, hogy  éppen
ez a követés az, ami a hivatkozásokat állandóvá teszi (nem szűnnek meg).

A :mod:`weakref` modulban olyan eszközök vannak, amelyekkel úgy lehet
nyomon követni az objektumokat, hogy a nyomkövetéssel nem hozol létre
újabb hivatkozást az objektumra. Amikor az objektumot már senki nem
használja, automatikusan törlődik a weakref (gyenge referencia)
táblából, és a weakref objektum erről értesítést kap. A tipikus
programok tároló objektumokat tartalmaznak, melyek létrehozása
erőforrásigényes::

   >>> import weakref, gc
   >>> class A:
   ...     def __init__(self, ertek):
   ...             self.ertek = ertek
   ...     def __repr__(self):
   ...             return str(self.ertek)
   ...
   >>> a = A(10)                   # hivatkozas letrehozasa
   >>> d = weakref.WeakValueDictionary()
   >>> d['primary'] = a            # itt nem keletkezik hivatkozas
   >>> d['primary']                # ha az objektum meg letezik, visszaadja az erteket 
   10
   >>> del a                       # toroljuk az egyetlen hivatkozast
   >>> gc.collect()                # a szemetgyujtest azonnal lefuttatjuk
   0
   >>> d['primary']                # ez a bejegyzes automatikusan megszunt, nem kell kulon torolni
   Traceback (most recent call last):
     File "<pyshell#108>", line 1, in -toplevel-
       d['primary']                # ez a bejegyzes automatikusan megszunt, nem kell kulon torolni
     File "C:/PY24/lib/weakref.py", line 46, in __getitem__
       o = self.data[key]()
   KeyError: 'primary'

.. _tut-list-tools:

Listakezelő eszközök
====================

A legtöbb adatstruktúrának szüksége van a beépített lista típusra. Néha
előfordul, hogy a listák egy másfajta megvalósítására van szükség,  az eredeti
listáktól eltérő viselkedéssel.

Az :mod:`array` modulban található :class:`array()` objektum hasonlít
azokhoz a listákhoz, melyek csak hasonló adat-típusokat tárolnak, de ezt
a tárolást sokkal tömörebben végzi. A következő példában egy számokból
álló tömböt láthatunk, ahol a számok mint két bájtos előjel-nélküli
bináris számként tárolódnak  (típuskód: ``"H"``) -- ellentétben a
hagyományos Python int (egész szám) objektumokból álló listákkal, ahol
minden bejegyzés 16 bájtot használ. ::

   >>> from array import array
   >>> a = array('H', [4000, 10, 700, 22222])
   >>> sum(a)
   26932
   >>> a[1:3]
   array('H', [10, 700])

A :mod:`collections` modulban lévő :class:`deque()` objektum egy -- a
listához hasonló típus -- viszont gyorsabban tud új elemet felvenni a
lista végére és elemet kiemelni a lista elejéről. Hátránya viszont, hogy
a keresésben lassabb - nehézkesebb, mint a hagyományos lista. Ez az
objektumtípus hasznos várakozási sorok, listák megvalósítására és
mélységi keresés esetén::

   >>> from collections import deque
   >>> d = deque(["task1", "task2", "task3"])
   >>> d.append("task4")
   >>> print("Handling", d.popleft())
   Handling task1

   unsearched = deque([starting_node])
   def breadth_first_search(unsearched):
       node = unsearched.popleft()
       for m in gen_moves(node):
           if is_goal(m):
               return m
           unsearched.append(m)

Ráadásul az Alapkönyvtár más eszközöket is tartalmaz, például a
:mod:`bisect` modult, ami rendezett listák módosítására szolgál::

   >>> import bisect
   >>> scores = [(100, 'perl'), (200, 'tcl'), (400, 'lua'), (500, 'python')]
   >>> bisect.insort(scores, (300, 'ruby'))
   >>> scores
   [(100, 'perl'), (200, 'tcl'), (300, 'ruby'), (400, 'lua'), (500, 'python')]

A :mod:`heapq`  modulban található függvényekkel megvalósíthatók a
hagyományos listákon alapuló  adathalmazok kezelése. A legalacsonyabb
értékű bejegyzés mindig a nulla pozícióba kerül.  Ez hasznos, ha a
programodnak gyakran kell elérnie a lista legkisebb elemét, de nem
akarod a listát teljes mértékben rendezni::

   >>> from heapq import heapify, heappop, heappush
   >>> data = [1, 3, 5, 7, 9, 2, 4, 6, 8, 0]
   >>> heapify(data)                      # átrendezi a listát
   >>> heappush(data, -5)                 # új elemet ad a listába
   >>> [heappop(data) for i in range(3)]  # kilistázza a három legkisebb elemet.
   [-5, 0, 1]

.. _tut-decimal-fp:

Lebegőpontos Aritmetika
=======================

A :mod:`decimal` modulban található a  :class:`Decimal` adattípus,
lebegőpontos számításokhoz. A beépített :class:`float` típushoz képest,
amely a bináris lebegőpontos számításokhoz készült, az új osztály nagyon
sokat segít pénzügyi programoknál (és ott, ahol véges decimális
ábrázolást használnak, a pontosság ellenőrzésével, a törvényeknek vagy a
szabályoknak megfelelő kerekítés használatával, a fontos számjegyek
nyomkövetésével -- vagy olyan programoknál, ahol a  felhasználó kézzel
végzett számításokhoz akarja hasonlítani a végeredményt.

Például számítsuk ki az 5%-os adóját egy 70 centes telefonköltségnek,
ami különböző eredményt ad decimális és bináris lebegőpontos számítás
használata esetén. A különbség fontos lesz, ha a kerekítés a
legközelebbi centhez történik::

   >>> from decimal import *       
   >>> Decimal('0.70') * Decimal('1.05')
   Decimal("0.7350")
   >>> .70 * 1.05
   0.73499999999999999       

A :class:`Decimal` osztály eredménye egy lezáró nullát tartalmaz,
automatikusan négy számjegyűen kerül ábrázolásra, a 2\*2 számjegyű
(tizedesjegyek) szorzás eredményeképp.  A :class:`Decimal` ugyanolyan
matematikát használ, mint amit a papíron végzett számolás,  és elkerüli
azokat a kérdéseket, amikor a bináris lebegőpontos számítás nem tud
abszolút pontosan ábrázolni decimális mennyiségeket.

A :class:`Decimal` osztály teljesen pontosan ábrázolja a maradékos
osztást,  és az egyenlőségtesztelést, ami a bináris lebegőpontos
ábrázolás esetén helytelen eredményre vezet::

   >>> Decimal('1.00') % Decimal('.10')
   Decimal("0.00")
   >>> 1.00 % 0.10
   0.09999999999999995

   >>> sum([Decimal('0.1')]*10) == Decimal('1.0')
   True
   >>> sum([0.1]*10) == 1.0
   False      

A :mod:`decimal` modulban a számítások pontosságát szükség szerint
beállíthatod::

   >>> getcontext().prec = 36
   >>> Decimal(1) / Decimal(7)
   Decimal("0.142857142857142857142857142857142857")

