.. _tut-io:

****************************
Bemenet és kimenet
****************************

Egy program kimenete többféleképpen jelenhet meg:  például ember által olvasható
nyomtatott formában, vagy későbbi feldolgozás céljából fájlba írva. Ez a fejezet
a lehetőségek közül többet is bemutat.

.. _tut-formatting:

Esztétikus kimenet kialakítása
==============================

Az értékek kiírására két lehetőségünk van:  *kifejezés állítás* és a
:func:`print` függvény. (Egy harmadik lehetőség a fájlobjektumok
:meth:`write` metódusának használata; a szabványos kimeneti fájlt  a
``sys.stdout``-ként érhetjük el. A referencia könyvtárt érdemes megnézni
(Library Reference), ott több információt találunk a szabványos kimenet
használatáról.

Gyakori igény, hogy a programozó az egyszerű - szóközökkel tagolt kimeneti
formázásnál több lehetőséget szeretne. A kétféleképpen formázhatod a kimenetet:

.. index:: module: string

* A kimeneti karakterláncok teljes formázását te kezeled -- a karakterláncok
  szétvágásával és összeillesztésével bármilyen elképzelt kimenetet előállíthatsz.
  A szabványos modulok között található :mod:`string` számos hasznos
  karakterlánc-kezelő rutint tartalmaz  (pl. karakterlánc kitöltése
  adott szélesség eléréséhez) -- ezek rövid bemutatása lejjebb
  található.

* A második lehetőség a ``%`` operátor használata, ahol a formázandó
  karakterlánc baloldali argumentum (Példa pár sorral lentebb!). A ``%`` operator
  feldolgozza a baloldali karakterláncot, mintha az :cfunc:`sprintf` függvényt
  használtuk volna --  és a formázott karakterláncot adja visszatérési értékként.

Egy kérdés maradt hátra: hogy hogyan konvertálhatsz egy számjegyekből álló
karakterláncot értékkel bíró számmá? Szerencsére a Python erre több lehetőséget
is biztosít: add át a karakterláncot a :func:`repr`  vagy az :func:`str`
függvényeknek. A fordított egyszeres idézőjelek (``````) használata egyenértékű
a :func:`repr` függvény hívásával, de használatuk helyett a függvényhívást
javasoljuk.

Az :func:`str` függvény az értékek ember által olvasható formájával tér vissza,
míg a :func:`repr` függvény visszatérési értéke az interpreter számára
értelmezhető adat. (vagy :exc:`SyntaxError` kivételt vált ki nem megfelelő
szintaxis esetén.)

Azon objektumok esetében, amelyeknek nincs emberi olvasásra szánt ábrázolása, az
:func:`str` függvény ugyanazzal az értékkel tér vissza, mintha a  :func:`repr`
függvényt hívtuk volna meg.  Néhány érték, például a számok vagy a struktúrák
esetében -- mint például  a listák vagy a szótárak, bármely két fent említett
funkciót használva  ugyanazt az eredményt kapjuk. Karakterláncok és lebegőpontos
számoknak két eltérő ábrázolási módjuk van:

Néhány példa::

   >>> s = 'Helló, világ.'
   >>> str(s)
   'Helló, világ.'
   >>> repr(s)
   "'Helló, világ.'"
   >>> str(0.1)
   '0.1'
   >>> repr(0.1)
   '0.10000000000000001'
   >>> x = 10 * 3.25
   >>> y = 200 * 200
   >>> s = 'x értéke: ' + repr(x) + ', és y értéke: ' + repr(y) + '...'
   >>> print(s)
   x értéke: 32.5, és y értéke: 40000...

   % >>> # The repr() of a string adds string quotes and backslashes:
   >>> # A repr() függvény idézőjelek közé rakja a stringet,
   >>> # és kijelzi a különleges (escape) karaktereket is:
   ... hello = 'helló világ\n'
   >>> hellos = repr(hello)
   >>> print(hellos)
   'helló világ\n'

   >>> # A repr() függvénynek akár objektumokat is átadhatunk:
   ... repr((x, y, ('hús', 'tojás')))
   "(32.5, 40000, ('hús', 'tojás'))"

   % >>> # reverse quotes are convenient in interactive sessions:
   >>> # A visszahajló idézőjeleket (`) interaktív módban használhatjuk,
   >>> # ugyanazt érjük el, mint ha a repr() függvényt hívtuk volna meg:
   ... `x, y, ('spam', 'eggs')`
   "(32.5, 40000, ('spam', 'eggs'))"

Ha akarunk készíteni egy táblázatot, amiben a számok második és harmadik
hatványai szerepelnek, két lehetőségünk is van::

   >>> for x in range(1, 11):
   ...     print(repr(x).rjust(2), repr(x*x).rjust(3),)
   %...     # Note trailing comma on previous line
   ...     # Figyelem! az előző sor végén szereplő vessző miatt a
   ...     # következő print az előző sor végén folytatja a kiírást!
   ...     print(repr(x*x*x).rjust(4))
   ...
    1   1    1
    2   4    8
    3   9   27
    4  16   64
    5  25  125
    6  36  216
    7  49  343
    8  64  512
    9  81  729
   10 100 1000
   >>> for x in range(1,11):
   ...     print('%2d %3d %4d' % (x, x*x, x*x*x))
   ... 
    1   1    1
    2   4    8
    3   9   27
    4  16   64
    5  25  125
    6  36  216
    7  49  343
    8  64  512
    9  81  729
   10 100 1000

(Megjegyzés: az oszlopok között 1 szóköznyi helyet a :keyword:`print` utasítás
hagyott -- az utasítás a paraméterei között mindig 1 szóközt hagy.)

Ez a példa bemutatja a szöveges (karakterlánc)  objektumok :meth:`rjust`
metódusát, ami  a megadott karakterláncot jobbra igazítja, majd a megadott
szélességig  feltölti üres karakterekkel a baloldalt.  Ehhez hasonlóak a
:meth:`ljust` és a :meth:`center` függvények. Ezek írási műveletet nem végeznek,
egyszerűen visszatérnek az új karakterlánccal. Ha a bemenetként megadott szöveg
túl hosszú, azt nem csonkolják -- változatlanul  adják vissza az eredeti
karakterláncot. Ez elrontja ugyan a kimenet rendezettségét, de rendszerint jobb,
mintha a függvény valótlan (csonkított) értékkel térne vissza. (Ha valóban
szeletelni akarod a karakterláncot, ezt így tudod megtenni: ``x.ljust(
n)[:n]``.)

Létezik egy másik metódus, a :meth:`zfill`, amely az adott  numerikus
karakterláncot balról nulla karakterekkel tölti fel.  Ez könnyen megérthető a
plusz és minusz jelek esetében::

   >>> '12'.zfill(5)
   '00012'
   >>> '-3.14'.zfill(7)
   '-003.14'
   >>> '3.14159265359'.zfill(5)
   '3.14159265359'

A  ``%`` operátort használata így néz ki::

   >>> import math
   >>> print('PI értéke megközelítőleg %5.3f.' % math.pi)
   PI értéke megközelítőleg 3.142.

Ha a karakterláncban egynél több formázást szeretnél használni, akkor az alábbi
példát követve egy tuple változót kell paraméterként használnod::

   >>> table = {'Sjoerd': 4127, 'Jack': 4098, 'Dcab': 7678}
   >>> for name, phone in table.items():
   ...     print('%-10s ==> %10d' % (name, phone))
   ... 
   Jack       ==>       4098
   Dcab       ==>       7678
   Sjoerd     ==>       4127

A legtöbb formázás pontosan ugyanúgy működik, mint C-ben, és a megfelelő  típusú
változó átadását igényli. Ha kivétel váltódik ki, és azt a kódodban nem kapod
el, ???not a core dump???.

A ``%s`` formázás ennél rugalmasabb: ha a csatolt paraméter nem  egy
karakterlánc, akkor a :func:`str` beépített függvény használatával
automatikusan azzá konvertálja.  A ``*`` használata a szélesség, vagy a
pontosság meghatározására egy külön paraméterként (integer) lehetséges. A
következő C formázások nem támogatottak: ``%n``, ``%p``.

Ha egy nagyon hosszú formázott karakterláncot szeretnél használni, amit nem
akarsz darabokra felosztani, szép lenne,  ha hivatkozni tudnál a formázandó
változókra azok neveivel, pozíciójuk helyett. Ezt a ``%(name)format`` forma
használatával teheted meg, például így::

   >>> table = {'Bea': 1975, 'Balazs': 1978, 'Fanni': 2003}
   >>> print('Fanni: %(Fanni)d; Bea: %(Bea)d; Balazs: %(Balazs)d' % table)
   Fanni: 2003; Bea: 1975; Balazs: 1978


Ez különösen hasznos az új :func:`vars` függvénnyel együtt,  amely egy szótár
típusú változóval tér vissza,  amely az összes helyi változót tartalmazza.

.. _tut-files:

Fájlok írása és olvasása
========================

.. index::
   builtin: open
   object: file

Az :func:`open` függvény egy  object objektummal  tér vissza, és rendszerint két
paraméterrel használjuk: ``open(filename, mode)``. ::

   >>> f=open('/tmp/munkafile', 'w')
   >>> print(f)
   <open file '/tmp/munkafile', mode 'w' at 80a0960>

Az első paraméter egy fájlnevet tartalmazó karakterlánc. A második paraméter
néhány karakterből áll csupán, és a fájl használatának  a módját (írás, olvasás)
állíthatjuk be vele.  A megnyitás módja (*mode*) lehet ``'r'`` mikor csak
olvassuk a fájlt -- ``'w'``, ha kizárólag írni szerenténk (a  már esetleg
ugyanezen néven létező fájl tartalma törlődik!)  -- és ``'a'``, amikor
hozzáfűzés céljából nyitjuk meg a fájlt;  ilyenkor bármilyen adat, amit a fájlba
írunk, automatikusan  hozzáfűződik annak a végéhez.  A ``'r+'`` megnyitási mód
egyszerre nyitja meg a fájlt  írásra és olvasásra. A *mode* paraméter beállítása
nem kötelező; elhagyása esetén  ``'r'`` alapértelmezett értéket vesz fel.

Windows és Macintosh rendszereken a megnyitási módhoz hozzáfűzött  ``'b'``
karakterrel bináris módban nyithatjuk meg a fájlt, például így: ``'rb'``,
``'wb'`` vagy ``'r+b'``.   A Windows megkülönbözteti a szöveges és bináris
fájlokat;  a szöveges fájlban a sorvéget jelző karakterek (end-of-line) kis
mértékban automatikusan megváltoznak adat írása vagy olvasása esetén.

Ez a háttérben zajló módosítás ugyan megfelelő az ASCII szöveges fájlok számára,
de a bináris fájlokat használhatatlanná teszi (pl. :file:`JPG` vagy
:file:`.EXE` fájlokat).   Ezért nagyon figyelj oda, hogy a bináris módot
használd olvasáskor és íráskor. (Megjegyezzük, hogy Macintosh-on a használt C
könyvtártól függ a megnyitási módok pontos működése.

.. _tut-filemethods:

A fájl objektumok metódusai
---------------------------

A fejezet további példái feltételezik, hogy már létezik  az ``f`` fájl objektum.

A fájl tartalmának olvasásához hívd meg az  ``f.read(size)``  metódust, ami a
megadott adatmennyiségnek megfelelő hossszúságú  karakterlánccal tér vissza. A
*size* egy opcionális paraméter --  elhagyása, vagy negatív értéke esetén a
teljes tartalmat visszaadja a  metódus -- ha esetleg a fájl kétszer akkora, mint
a gépedben lévő memória, az esetleg problémát jelenthet neked.

Ha használod a *size* paramétert, akkor a visszatérési értékként kapott
karakterlánc hosszát maximalizálni tudod. Ha eléred a fájl végét,  az
``f.read()`` egy üres karakterlánccal tér vissza (``""``). ::

   >>> f.read()
   'Ez a fájl teljes tartalma.\n'
   >>> f.read()
   ''

A ``f.readline()``  egy sort olvas ki a fájlból. A sor végét az újsor karakter
(``\n``) jelenti, amely a beolvasott karakterlánc végén található. Ez a karakter
egyetlen esetben maradhat ki a visszaadott karakterlácból:  ha a fájl utolsó
sorát olvassuk be, és az nem újsor karakterre végződik.

Ez a visszatérési értéket egyértelművé teszi: ha a  ``f.readline()`` metódus
üres karakterlánccal tér vissza, az olvasás elérte a fájl végét. Ekkor az üres
sort a ``'\n'`` karakter  jelképezi -- a karakterlánc egyetlen egy újsor
karaktert tartalmaz. ::

   >>> f.readline()
   'Ez a fájl első sora.\n'
   >>> f.readline()
   'A fájl második sora.\n'
   >>> f.readline()
   ''

A ``f.readlines()`` metódus egy listával tér vissza,  amely a fájl minden sorát
tartalmazza. Ha megadjuk a *sizehint* paramétert, a metódus a megadott számú
byte-ot kiolvassa a fájlból, és még annyit, amennyi a következő újsor
karakterig tart. (A fordító megjegyzése: *sizehint* paramétert hiába adtam meg,
2.1-es pythont használva a teljes fájltartalmat kiolvasta.) ::

   >>> f.readlines()
   ['Ez a fájl első sora.\n', 'Ez pedig a második\n']

A ``f.write(string)`` metódus a *string* tartalmát a fájlba írja, és ``None``
értékkel tér vissza.  ::

   >>> f.write('Tesztszöveg, az írás bemutatására\n')

Ha egy karakterlánctól eltérő típusú változót  szeretnénk kiírni, akkor azt
előbb karakterlánccá kell konvertálni::

   >>> value = ('a valasz', 42)
   >>> s = str(value)
   >>> f.write(s)

Ford.: Ha megnézzük a keletkezett fájl tartalmát, az ``('a valasz', 42)`` lesz.

Az ``f.tell()`` metódussal a fájl objektum aktuális pozícióját kérdezhetjük le
-- bájt-ban, a fájl kezdetétől számolva.  (pl.: hányadik bájt-ot olvassuk most
éppen)

A fájl objektum pozíciójának (a kurzornak) a megváltoztatásának módja:
``f.seek(léptetés, innen_kezdve)``.   Az  *innen_kezdve* ponttól *léptetés*
mennyiséggel mozgatjuk a kurzort.  (Példa: ha van egy 100 bájtos fájl, amiben
éppen a 39. karakternél állunk, akkor az aktuális pozícióhoz képest ugorhatunk
tovább, megadott lépésekben)

A *from_what* value of 0 measures from the beginning of the file, 1 uses the
current file position, and 2 uses the end of the file as the reference point.
*from_what* can be omitted and defaults to 0, using the beginning of the file as
the reference point.

Az *innen_kezdve* paraméter a következő értékeket veheti fel:

* 0: a fájl eleje lesz a hivatkozási pont

* 1: az aktuális pozíció lesz a hivatkozási pont

* 2: a fájl vége az aktuális hivatkozási pont

::

   >>> f = open('/tmp/munkafajl', 'r+')
   >>> f.write('0123456789abcdef')
   >>> f.seek(5)     # Ugorj a 6. bajthoz a fajlban 
   >>> f.read(1)        
   '5'
   >>> f.seek(-3, 2) # Ugorj a fajl vegehez kepest harom karakterrel vissza
   >>> f.read(1)
   'd'

Ha már minden módosítást elvégeztél a fájl-al, hívd meg a   ``f.close()``
metódust a fájl bezárásához (a rendszererőforrások felszabadításához). A
``f.close()`` hívása után a fájl objektum használatára tett kísérletek
automatikusan meghiúsulnak.  ::

   >>> f.close()
   >>> f.read()
   Traceback (most recent call last):
     File "<stdin>", line 1, in ?
   ValueError: I/O operation on closed file

A fájl objektumoknak van néhány kiegészítő metódusuk, például az :meth:`isatty`
és a :meth:`truncate` -- ezek ritkábban használtak. A Referencia könyvtárban
teljes útmutatót találsz a fájl objektumok használatához.

.. _tut-pickle:

A :mod:`pickle` modul
---------------------

.. index:: module: pickle

A karakterláncokat könnyű fájlba írni és onnan kiolvasni.  A számokkal kicsit
nehezebb a helyzet, mert a :meth:`read`  metódus mindent karakterként ad vissza,
amit aztán át a számmá történő átalakítás miatt például az :func:`int`
függvénynek kell átadnunk,  ami a ``'123'`` karakterláncból a 123-as értéket
adja vissza.

Hogyha összetettebb adattípusokat akarsz menteni, például listákat, szótárakat
vagy osztály-példányokat, egyre bonyolultabbá válik a dolgod.

Ahelyett, hogy a felhasználóknak állandóan adatmentő algoritmusokat kelljen
írnia és javítania, a Python rendelkezik a  :mod:`pickle` modullal.   Ez
egy elképesztő modul,  ami képes a legtöbb Python objektumot
karakterláncként ábrázolni (még a Python kódok néhány formáját is!).
Ezt a folyamatot :dfn:`pickling`-nek hívják.

Az objektum karakterláncból történő létrehozását pedig :dfn:`unpickling`-nek
nevezik.

A karakterlánccá történő konvertálás és a visszakonvertálás között a
karakterlánc jelképezi az objektumot. Ezt akár fájlban, akár más formában
tárolhatjuk. (hálózaton elküldve másik gépen, vagy adatbázisban)

Ha van egy ``x`` objektumod, és létezik az írásra megnyitott  ``f`` fájl
objektum, a legegyszerűbb út az objektumod tárolására a  következő egysoros kód::

   pickle.dump(x, f)

Ha újból elő kívánod állítani az objektumot,   és az ``f`` egy olvasásra
megnyitott fájl objektum::

   x = pickle.load(f)

(Vannak más variációi is ennek, amik több objektum tárolásánál / újbóli
előállításánál használatosak, illetve akkor, ha nem akarod a tárolási formába
alakított objektumodat ténylegesen fájlba írni; a téma teljes
dokumentációja: :mod:`pickle` a Python Library Referenceben.)

A :mod:`pickle`   a szabályos módja a
Python objektumok készítésének,  amiket tárolhatsz, vagy újra felhasználhatsz
más programokkal,  vagy akár a jövőbeni felhasználás céljából a jelenlegi
programoddal.  Ennek a technikai megnevezése: :dfn:`persistent` objektumok
(állandó, tartós).   Azért, mert a :mod:`pickle`  széles körben
elterjedt,  a legtöbb Python kiegészítést készítő programozó
figyelmeztet, hogy bizonyosodj meg arról, hogy az új adattípusok
tárolhatók és újra előállíthatók  (mintha egy matricát leragasztanál,
majd újra felvennéd).

