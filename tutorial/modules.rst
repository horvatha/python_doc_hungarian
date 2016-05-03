.. _tut-modules:

*****************
Modulok
*****************

Ha a Pythont interaktív módban használod, és kilépsz az értelmezőből, majd újra
visszalépsz, minden függvénydefiníció és definiált változó elvész.  Emiatt ha
nagyobb programot akarsz írni, jobban jársz ha valamilyen  szerkesztő programot
használsz az értelmező számára előkészíteni a bemeneti adatokat, és az értelmezőt
úgy futtatod, hogy a bemenet a szövegfájlod legyen.

Ezt a folyamatot *szkriptírásnak* nevezik.  Ahogy a programod egyre hosszabb
lesz, előbb-utóbb részekre (fájlokra) akarod majd bontani, a könnyebb
kezelhetőség végett. Valószínűleg lesznek praktikus függvényeid, amit már megírt
programjaidból szeretnél használni a függvénydefiníciók másolása nélkül.

Ennek a támogatására a Python el tudja helyezni a függvénydefiníciókat  egy
adott fájlba, amik aztán elérhetők lesznek egy szkriptből, vagy  az interaktív
értelmezőből. Ezeket a fájlokat  *moduloknak* hívjuk. A modulban használt
definíciók *importálhatók* más modulokba (például a modulok hierarchiájában
legfelül lévő *main* modulba is). (A következő példákhoz: a felhasznált változók
mind a legfelső névtérben  helyezkednek el, a modulok függvényeit itt futtatjuk,
interaktív módban.)

A modul egy olyan szöveges fájl, ami Python definíciókat és utasításokat
tartalmaz.  A fájl neve egyben a modul neve is (a :file:`.py` kiterjesztést nem
beleértve). A programból az aktuális modul neve a ``__name__`` globális
változóból elérhető  (karakterláncként).

Kérlek egy neked megfelelő szövegszerkesztővel (pl. az IDLE-vel) hozd
létre a :file:`fibo.py` fájlt, a következő tartalommal::

   # Fibonacci-sorozat modul

   def fib(n):    # kiírja a Fibonacci-sorozatot n-ig
       a, b = 0, 1
       while b < n:
           print(b, end="")
           a, b = b, a+b

   def fib2(n): # visszatér a Fibonacci-sorozattal, n-értékig
       eredmeny = []
       a, b = 0, 1
       while b < n:
           eredmeny.append(b)
           a, b = b, a+b
       return eredmeny

Most lépj be a Python értelmezőbe, és importáld a fenti modult  a következő
paranccsal::

   >>> import fibo

Ez a parancs a ``fibo``-ban definiált függvényneveket közvetlenül nem emeli be
az aktuális szimbólumtáblába;  csak magát a modul nevét, ``fibo``-t emeli be.

::

   >>> fibo.fib(1000)
   1 1 2 3 5 8 13 21 34 55 89 144 233 377 610 987
   >>> fibo.fib2(100)
   [1, 1, 2, 3, 5, 8, 13, 21, 34, 55, 89]
   >>> fibo.__name__
   'fibo'

Ha egy függvényt gyakran szeretnél használni az importált modulból,
hozzárendelheted azt egy lokális függvénynévhez::

   >>> fib = fibo.fib
   >>> fib(500)
   1 1 2 3 5 8 13 21 34 55 89 144 233 377

.. _tut-moremodules:

Bővebben a modulokról 
========================

A modulok végrehajtható utasításokat ugyanúgy tartalmazhatnak, mint
függvénydefiníciókat.  Ezeket az utasításokat rendszerint a modul
inicializálásához használjuk.  Kizárólag a modul *első* importálásakor
futnak le.  [#]_

Minden modulnak megvan a saját szimbólumtáblája, ami a modulban definiált
függvények számára globális.  Emiatt a modul létrehozója nyugodtan használhatja
a modulban lévő globális változókat, és nem kell aggódnia egy esetleges
névütközés miatt.  Másrészről, ha pontosan tudod hogy mit csinálsz,  el
tudod érni a modul globális változóit a változónév teljes elérési
útjának a használatával: ``modulnév.elemnév``.

A modulok importálhatnak más modulokat. Szokás -- de nem kötelező --  az
:keyword:`import` utasításokat a modul elején elhelyezni  (vagy a szkript
elején, lásd később). Az importált modulneveket az értelmező az importáló modul
globális  szimbólumtáblájába helyezi el.

Az  :keyword:`import` utasítás egyik változata, hogy az importált
függvényeket közvetlenül az importáló modul szimbólumtáblájába helyezzük
el.  Például::

   >>> from fibo import fib, fib2
   >>> fib(500)
   1 1 2 3 5 8 13 21 34 55 89 144 233 377

Az előbbi példa a helyi szimbólumtáblába nem helyezi el annak a modulnak
nevét, amiből az importálás történt. (``fibo`` objektum nem jön létre a
névtérben)

Ha a modulban lévő összes nevet közvetlenül a helyi szimbólumtáblába szeretnéd
importálni, így tudod megtenni::

   >>> from fibo import *
   >>> fib(500)
   1 1 2 3 5 8 13 21 34 55 89 144 233 377

Ha így használod az ``import`` utasítást, a forrásmodul minden  nevét a helyi
szimbólumtáblába emeled, kivéve az aláhúzással (``_``) kezdődőeket.

A legtöbb esetben a Python-programozók nem használják ezt a lehetőséget,
mivel egy halom ismeretlen nevet helyez az értelmezőbe, amelyek esetleg
felülírnak már definiált dolgokat.

Érdemes megjegyezni, hogy általában a ``*`` importálása egy modulból
vagy csomagból kerülendő, mivel ez gyakran nehezen olvasható kódot
eredményez. Azonban egy interaktív parancsértelmezőben teljesen érthető
a használata a gépelés lerövidítésére.

.. note::

   Hatékonysági okok miatt minden modul csak egyszer kerül importálásra
   egy értelmezési szakaszban. Ezért, ha változtatsz a modulon, újra
   kell indítanod az értelmezőt -- vagy, ha ez csak egyetlen modul, amit
   interaktívan szeretnél tesztelni, használd az :func:`imp.reload`
   függvényt, például: ``import imp; imp.reload(modulnév)``.

.. _tut-modulesasscripts:

Modulok végrehajtása szkriptként
-------------------------------------

Amikor végrehajtasz egy Python modult így ::

   python fibo.py <arguments>

a modulbeli kód végrehajtásra kerül, ugyanúgy, mintha importáltad volna,
csak a ``__name__`` változót ``"__main__"`` értékre állítva. Ez azt
jelenti, hogyha a következő kódot a modul végéhez adod::

   if __name__ == "__main__":
       import sys
       fib(int(sys.argv[1]))

akkor a fájlt szkriptként is használhatóvá teszed és modulként is, mivel
a kód, amely a parancssort feldolgozza csak akkor kerül végrehajtásra,
amikor az "main" fájlként kerül végrehajtásra::

   $ python fibo.py 50
   1 1 2 3 5 8 13 21 34

Amikor a modult importáljuk, a kód nem fog lefutni::

   >>> import fibo
   >>>

Ezt a lehetőséget gyakran használják egy kényelmes felhasználói
interfész létrehozására a modulhoz, vagy tesztelési okokból (a modul
futtatása szkriptként végrehajtja a teszteket).


.. _tut-searchpath:

A modulok keresési útvonala
---------------------------

.. index:: triple: module; search; path

Amikor a :mod:`spam` modult importáljuk, az értelmező először egy
beépített modult keres ilyen néven.  az aktuális könyvtárban keresi a
:file:`spam.py` fájlt. Ha itt nem találja, tovább keres a
:data:`sys.path` változóban felsorolt könyvtárakban a a :file:`spam.py`
fájl után. A :data:`sys.path` változó a kezdeti értékét a következő
helyekről kapja:

* a könyvtárból, amely a bemeneti szkriptet tartalmazza (vagy az aktuális
  könyvtárból)
* :envvar:`PYTHONPATH` környezeti változóból (ez könyvtárnevek listája,
  hasonló szintaktikával, mint a :envvar:`PATH` környezeti változóból).
* egy telepítéstől függő alapértelmezésből.

A :data:`sys.path` kezdeti értékeit a Python-program módosítani tudja. 
A könyvtár, amely a futtatott szkriptet tartalmazza, a keresési útvonal
elejére kerül, a standard könyvtár útvonala elé. Ez azt jelenti, hogy az
ebben a könyvtárban található szkriptek fognak betöltődni azok helyett a
könyvtárbeli modulok helyett, amelyeknek azonos a nevük. Ez hiba, hacsak
nem volt szándékos a helyettesítés. A :ref:`tut-standardmodules` szakasz
több információt nyújt erről.
:ref:`tut-standardmodules` for more information.

.. %
    Do we need stuff on zip files etc. ? DUBOIS

,,Lefordított'' Python-fájlok
----------------------------------

Hogy a modulok betöltődését lerövidítsük, a Python eltárolja az összes
modul lefordított változatát a ``__pycache__`` könyvtárban
:file:`modul.{verzió}.pyc`` név alatt, ahol a verzió mutatja a
lefordított fájl formátumát; általában a Python verziószámát
tartalmazza. Például a spam.py fájlnak a CPython 3.4-es kiadásával
fordított változata a ``__pycache__/spam.cpython-34.pyc`` néven kerül
eltárolásra. Ez az elnevezési szokás lehetővé teszi, hogy különböző
Python-verziókkal fordított modulváltozatok létezzenek egyszerre. 

A Python összeveti a forráskód módosításának dátumát a fordított
változatéval, hogy eldöntse, újra kell-e fordítani. Ez teljesen
automatikus folyamat. A lefordított modulok teljesen platformfüggetlenek,
így a könyvtárakat meg lehet osztani különböző architektúrájú rendszerek
között.

A Python két esetben nem ellenőrzi a fordított változatot. Első eset:
teljesen újra fordítja, és nem tárolja az eredményt, ha a modul direkt a
parancssorból lett meghívva. A másik eset, amikor nem ellenőrzi a
fordított változatot, ha nincs forrásmodul. A forrás nélküli (csak
fordított) terjesztések esetén a lefordított modulnak a
forráskönyvtárban kell elhelyezkedniük, és nem lehet jelen forrásmodul.

Néhány tipp haladóknak:

* Amikor a Python értelmezőt a :option:`-O` vagy :option:`-OO`
  paraméterrel hívjuk meg, az kisebb méretre fordítja le a modult.   Az
  :option:`-O` paraméter eltávolítja az assert utasításokat, a
  :option:`-OO` paraméter az az assert utasításokon felül a
  dokumentációs karakterláncokat is.  Mivel több modul épít ezekre az
  összetevőkre, csak akkor használd ezt, ha tudod, mit teszel. Az
  optimalizált modulok ``.pyo`` kiterjesztést használnak ``.pyc``
  helyett, és általában kisebbek.  A későbbi kiadásokban az
  optimalizálás hatása változhat.

* A program semmivel sem fut gyorsabban, ha :file:`.pyc` vagy :file:`.pyo`
  kiterjesztésű bájtkódot futtatunk -- a sebességnövekedés a betöltési időben
  jelentkezik. Ha a :file:`.py` fálj bájtkódra fordított verziója rendelkezésre
  áll,  nincs szükség a fordításra, rögtön lehet futtatni a bájtkódot tartalmazó
  verziót.

*
  .. index:: module: compileall

  A  :mod:`compileall`    modul :file:`.pyc`  (vagy :option:`-O`
  kapcsoló esetén :file:`.pyo`) fájlokat tud készíteni az aktuális
  könyvtár összes moduljából.  

* Több részlet található erről a folyamatról a PEP 3147-ben. Itt
  megtalálható a döntés folyamatábrája is.


.. _tut-standardmodules:

Standard modulok
================

A Python funkciójuk szerint csoportokba sorolt szabványos modulokkal rendelkezik
--  egy könyvtárral -- részletesen: `Python Library Reference -- Python
referenciakönyvtár a későbbiekben <https://docs.python.org/3/library/>`_ `A
modulok tételes felsorolása -- Module index
<https://docs.python.org/3/py-modindex.html>`_  Néhány modult az értelmezőbe
építettünk be, ezeken keresztül olyan funkciók megvalósítása lehetséges, amelyek
ugyan nem tartoznak szorosan a nyelvhez,  de például az operációs rendszerrel
való kapcsolathoz szükségesek -- ilyenek például a rendszerhívások.

.. index:: module: sys

Ezen modulok függenek a használt operációs rendszertől, hiszen annak
működtetéséhez kellenek. Például az :mod:`amoeba` modul csak azokon a
rendszereken elérhető, amik támogatják az Amoeba primitívek használatát.
A másik figyelemre méltó -- és különleges modul a  `sys
<http://docs.python.org/lib/module-sys.html>`_ , ami minden Python
értelmezőbe be van építve. Például a  ``sys.ps1`` és ``sys.ps2``
változók tartalmazzák az értelmezőben megjelenő elsődleges és másodlagos
prompt karakterláncát::

   >>> import sys
   >>> sys.ps1
   '>>> '
   >>> sys.ps2
   '... '
   >>> sys.ps1 = 'C> '
   C> print('Yuck!')
   Yuck!
   C>


Ez a két változó csak akkor létezik, ha az értelmező interaktív módban fut.

A  ``sys.path`` változó karakterláncok listáját tartalmazza,  melyek
meghatározzák az értelmező keresési útvonalát,  amit az a modulok importálásakor
bejár. Kezdeti értékét a :envvar:`PYTHONPATH` környezeti változóból veszi,  vagy
ha ez nem létezik, akkor az értelmezőbe beépített alapértelmezett  útvonalakból.
A változó értékét ugyanúgy módosíthatod, mint egy listáét::

   >>> import sys
   >>> sys.path.append('/ufs/guido/lib/python')

.. _tut-dir:

A :func:`dir` függvény
======================

A beépített :func:`dir` függvénnyel listázhatjuk ki a modulban  definiált
neveket. A :func:`dir` meghívása után a nevek rendezett listájával tér vissza.

::

   >>> import fibo, sys
   >>> dir(fibo)
   ['__name__', 'fib', 'fib2']
   >>> dir(sys)  # doctest: +NORMALIZE_WHITESPACE
   ['__displayhook__', '__doc__', '__excepthook__', '__loader__', '__name__',
    '__package__', '__stderr__', '__stdin__', '__stdout__',
    '_clear_type_cache', '_current_frames', '_debugmallocstats', '_getframe',
    '_home', '_mercurial', '_xoptions', 'abiflags', 'api_version', 'argv',
    'base_exec_prefix', 'base_prefix', 'builtin_module_names', 'byteorder',
    'call_tracing', 'callstats', 'copyright', 'displayhook',
    'dont_write_bytecode', 'exc_info', 'excepthook', 'exec_prefix',
    'executable', 'exit', 'flags', 'float_info', 'float_repr_style',
    'getcheckinterval', 'getdefaultencoding', 'getdlopenflags',
    'getfilesystemencoding', 'getobjects', 'getprofile', 'getrecursionlimit',
    'getrefcount', 'getsizeof', 'getswitchinterval', 'gettotalrefcount',
    'gettrace', 'hash_info', 'hexversion', 'implementation', 'int_info',
    'intern', 'maxsize', 'maxunicode', 'meta_path', 'modules', 'path',
    'path_hooks', 'path_importer_cache', 'platform', 'prefix', 'ps1',
    'setcheckinterval', 'setdlopenflags', 'setprofile', 'setrecursionlimit',
    'setswitchinterval', 'settrace', 'stderr', 'stdin', 'stdout',
    'thread_info', 'version', 'version_info', 'warnoptions']

Ha paraméterek nélkül hívjuk meg a :func:`dir` függvényt,  az aktuális névtérben
definiált nevekkel tér vissza::

   >>> a = [1, 2, 3, 4, 5]
   >>> import fibo, sys
   >>> fib = fibo.fib
   >>> dir()
   ['__name__', 'a', 'fib', 'fibo', 'sys']

Fontos, hogy az így kapott lista az összes névfajtát tartalmazza. Változókat,
modulokat, függvényeket, stb.

.. index:: module: builtins

A :func:`dir` nem listázza ki a nyelv beépített függvényeit és
változóit. Ezek a :mod:`builtins` modulban vannak definiálva::

   >>> import builtins
   >>> dir(builtins)  # doctest: +NORMALIZE_WHITESPACE
   ['ArithmeticError', 'AssertionError', 'AttributeError', 'BaseException',
    'BlockingIOError', 'BrokenPipeError', 'BufferError', 'BytesWarning',
    'ChildProcessError', 'ConnectionAbortedError', 'ConnectionError',
    'ConnectionRefusedError', 'ConnectionResetError', 'DeprecationWarning',
    'EOFError', 'Ellipsis', 'EnvironmentError', 'Exception', 'False',
    'FileExistsError', 'FileNotFoundError', 'FloatingPointError',
    'FutureWarning', 'GeneratorExit', 'IOError', 'ImportError',
    'ImportWarning', 'IndentationError', 'IndexError', 'InterruptedError',
    'IsADirectoryError', 'KeyError', 'KeyboardInterrupt', 'LookupError',
    'MemoryError', 'NameError', 'None', 'NotADirectoryError', 'NotImplemented',
    'NotImplementedError', 'OSError', 'OverflowError',
    'PendingDeprecationWarning', 'PermissionError', 'ProcessLookupError',
    'ReferenceError', 'ResourceWarning', 'RuntimeError', 'RuntimeWarning',
    'StopIteration', 'SyntaxError', 'SyntaxWarning', 'SystemError',
    'SystemExit', 'TabError', 'TimeoutError', 'True', 'TypeError',
    'UnboundLocalError', 'UnicodeDecodeError', 'UnicodeEncodeError',
    'UnicodeError', 'UnicodeTranslateError', 'UnicodeWarning', 'UserWarning',
    'ValueError', 'Warning', 'ZeroDivisionError', '_', '__build_class__',
    '__debug__', '__doc__', '__import__', '__name__', '__package__', 'abs',
    'all', 'any', 'ascii', 'bin', 'bool', 'bytearray', 'bytes', 'callable',
    'chr', 'classmethod', 'compile', 'complex', 'copyright', 'credits',
    'delattr', 'dict', 'dir', 'divmod', 'enumerate', 'eval', 'exec', 'exit',
    'filter', 'float', 'format', 'frozenset', 'getattr', 'globals', 'hasattr',
    'hash', 'help', 'hex', 'id', 'input', 'int', 'isinstance', 'issubclass',
    'iter', 'len', 'license', 'list', 'locals', 'map', 'max', 'memoryview',
    'min', 'next', 'object', 'oct', 'open', 'ord', 'pow', 'print', 'property',
    'quit', 'range', 'repr', 'reversed', 'round', 'set', 'setattr', 'slice',
    'sorted', 'staticmethod', 'str', 'sum', 'super', 'tuple', 'type', 'vars',
    'zip']

.. _tut-packages:

A csomagok
==========

.. xxx Átnézni a fejezetet

A csomagok adnak lehetőséget a Python modulok névtereinek strukturálására, a
pontozott modulnevek használatával. Például a :mod:`A.B` modulnév  hivatkozik a
``B`` modulra, ami az ``A`` modulban található (ott importáltuk). Ha a
programozók a fenti példa szerint használják a modulokat, nem kell amiatt
aggódniuk, hogy egymás globális neveivel ütközés lép fel. Például a több
modulból álló csomagok (NumPy, Python Imaging Library...) írói is a pontozott
modulnevek használatával kerülik el a változónevek ütközését.

Tegyük fel, hogy egy modulokból álló csomagot akarsz tervezni, hogy egységesen
tudd kezelni a hangfájlokat és a bennük lévő adattartalmat. Több különböző
hangfájlformátum létezik (rendszerint a kiterjesztésük alapján lehet őket
beazonosítani, pl.:  :file:`.wav`, :file:`.aiff`, :file:`.au`) -- valószínűleg
egy bővülő modulcsoportot kell készítened és karbantartanod a fájlformátumok
közötti konvertálásra.

Ráadásul még többfajta műveletet is el kell tudnod végezni a hanganyagon,
például keverést, visszhang készítését, hangszínszabályzást, művészeti sztereo
effekteket -- szóval a fentiek tetejébe még  írni fogsz egy végeláthatatlan
modulfolyamot, ami ezeket a műveleteket elvégzi. A csomagok egy lehetséges
struktúrája -- a hierarchikus fájlrendszereknél használatos jelöléssel::

   sound/                          Legfelső szintű csomag
         __init__.py               a sound csomag inicializálása
         formats/                  A fájlformátum konverziók alcsomagja
                 __init__.py
                 wavread.py
                 wavwrite.py
                 aiffread.py
                 aiffwrite.py
                 auread.py
                 auwrite.py
                 ...
         effects/                  A hangeffektusok alcsomagja
                 __init__.py
                 echo.py
                 surround.py
                 reverse.py
                 ...
         filters/                  A szűrők alcsomagja
                 __init__.py
                 equalizer.py
                 vocoder.py
                 karaoke.py
                 ...

A csomag: olyan hierarchikus könyvtárszerkezet, amely egymással  összefüggő
modulokat tartalmaz.

Egy csomag importálása során a Python bejárja a ``sys.path``  változóban
szereplő könyvtárakat, a csomag alkönyvtárak után kutatva. A ``sys.path`` az
előre meghatározott keresési útvonalakat tartalmazza.

A Python az :file:`__init__.py` fájlok jelenlétéből tudja, hogy egy könyvtárat
csomagként kell kezelnie -- és ez a fájl segít abban is, hogy az alkönyvtárakban
lévő csomagokat is érzékelje a Python.

A legegyszerűbb esetben az :file:`__init__.py` egy üres fájl, de tartalmazhat és
végrehajthat a csomaghoz tartozó inicializáló kódot, vagy beállíthatja az
``__all__`` változót (lásd lejjebb).

A csomag felhasználói egyenként is importálhatnak modulokat a  csomagból::

   import sound.effects.echo

Ez betölti a :mod:`sound.effects.echo` almodult.  A hivatkozást teljes
útvonallal kell megadni.  ::

   sound.effects.echo.echofilter(input, output, delay=0.7, atten=4)

Egy másik alternatíva almodulok importálására::

   from sound.effects import echo

Ez szintén betölti az :mod:`echo` almodult, és elérhetővé teszi  a csomagnevek
nélkül is (nem kell a sound.effects előtag).  ::

   echo.echofilter(input, output, delay=0.7, atten=4)

Van még egy lehetőség a kiválasztott függvény importálására::

   from sound.effects.echo import echofilter

Ez szintén betölti az :mod:`echo` almodult, de a :func:`echofilter` függvény
közvetlenül elérhetővé válik::

   echofilter(input, output, delay=0.7, atten=4)

Fontos, hogy a ``from csomag import elem`` használatakor az elem az importált
csomag almodulja (submodule) vagy alcsomagja (subpackage) is lehet, vagy
valamilyen más, a csomagban definiált 'elem' nevű objektum,  például függvény,
osztály vagy változó.

Az ``import`` utasítás először ellenőrzi, hogy az importálandó elem definiálva
van-e a csomagban. Ha nem, akkor az elemről feltételezi, hogy az egy modul,  és
megkísérli azt betölteni. Ha a modul keresése sikertelen,  :exc:`ImportError`
kivétel váltódik ki.

Ezzel ellentétben az ``import elem.alelem.alelem_aleleme`` utasításforma
használatakor az utolsó alelem_aleleme kivételével mindegyik elemnek csomagnak
kell lennie. Az utolsó elem lehet modul vagy csomag is, de a fentiekkel
ellentétben nem lehet osztály, függvény vagy definiált változó.

.. _tut-pkg-import-star:

Egy csomagból \* importálása
--------------------------------------

.. index:: single: __all__

Mi történik, amikor a programozó kiadja a  ``from sound.effects import *``
utasítást? Ideális esetben az értelmező a fájlrendszerben megtalálja a csomagban
lévő almodulokat, és mindet importálja. Az összes almodul importálása
hosszú ideig eltarthat, és olyan mellékhatásai lehetnek, amiknek akkor
kellene megtörténniük, amikor az almodult közvetlenül importáljuk.

A csomagkészítők számára az egyedüli megoldás az, ha a csomagot egyértelmű
index-el látják el. Az import utasítás a következő szabályokat használja: ha a
csomag :file:`__init__.py` állományának kódjában szerepel az  ``__all__`` nevű
lista, az abban felsorolt nevek lesznek az  importálandó modulnevek a ``from
package import *`` utasítás végrehajtásakor.
A csomag készítőjének feladata, hogy ezt a listát naprakészen tartsa, mikor a
csomag újabb verzióját készíti.  A csomagkészítők dönthetnek úgy, hogy ezt a
funkciót nem támogatják, ha nem tartják hasznosnak, hogy valaki az \*-ot
importálja.  Például a :file:`sounds/effects/__init__.py` fájl a
következő kódot tartalmazhatja::

   __all__ = ["echo", "surround", "reverse"]

A fentiek értelmében a ``from sound.effects import *`` importálni fogja a három
felsorolt almodult a :mod:`sound` csomagból.

Ha az ``__all__`` lista nem meghatározott, a  ``from sound.effects
import *`` utasítás *nem* importálja a :mod:`sound.effects` csomag
összes almodulját az aktuális névtérbe -- csupán azt biztosítja, hogy a
:mod:`sound.effects` csomag importálva legyen (lehetőleg végrehajtva az
összes utasítást a :file:`__init__.py` inicializáló kódból) és aztán a
csomagban található összes nevet importálja.  Ebbe beleértendő minden
név, amit definiáltak (és az almodulok, amiket importáltak) az
:file:`__init__.py`-ben.  Szintén importálásra kerül a csomag minden
almodulja, amit korábbi ``import``  utasításokkal közvetlenül
importáltunk. Tekinsük a következő kódot::

   import sound.effects.echo
   import sound.effects.surround
   from sound.effects import *

Ebben a példában az :mod:`echo` és a :mod:`surround` modulokat az
értelmező az aktuális névtérbe  importálja, mert a :mod:`sound.effects`
csomag részei voltak a ``from...import`` utasítás végrehajtásakor. (Ez
akkor is így lesz, ha az ``__all__`` változó definiálva van.)

Jó tudni, hogy az ``import *`` importálási mód használata kerülendő,
mert a kódot nehezen olvashatóvá teszi. Ámbár ennek az importálási
módnak a használatával egyszerűbben dolgozhatunk az értelmező interaktív
módjában, egyes modulokat úgy terveznek, hogy csak az ``__all__``
változóban megadott  neveket exportálják.

Emlékezzünk rá, hogy semmi probléma nincs a  ``from Package import
specific_submodule`` szerkezet használatával! Valójában ez az ajánlott
importálási mód, hacsak az importáló modulnak nincs szüksége különböző
csomagokból származó  azonos nevű almodulok használatára.

Csomagon belüli hivatkozások
----------------------------

Ha a csomagokban alcsomagok szerepelnek (mint a :mod:`sound` csomag a
példában), abszolút importálást használhatsz arra, hogy egy
testvércsomag almoduljára hivatkozhassál.

Például a :mod:`sound.filters.vocoder` modulnak szüksége van a
:mod:`sound.effects` csomag :mod:`echo` moduljára a nyugtázás miatt,
akkor használhatja a ``from sound.effects import echo`` sort.

Relatív importálást is használhatsz az import utasítás 
``from modul import név`` alakjával. Ezek az importálások kezdeti
pontokat használnak arra, hogy az aktuális és szülőkönyvtárban szereplő
csomagokra hivatkozhassunk a relatív importálás során.
A :mod:`surround` modulból például használhatod a következőket::

   from . import echo
   from .. import formats
   from ..filters import equalizer

Vegyük észre, hogy a relatív importálás az aktuális modul neve alapján
működik. Mivel a főmodul neve mindig ``"__main__"``, ezért azok a
modulok, amelyeket egy alkalmazás főmoduljának szánunk, mindig abszolút
importálást kell használnia.

Modulok, amelyek több, különálló könyvtár moduljaiból épülnek fel
-----------------------------------------------------------------

A csomagok rendelkeznek egy egyedi jellemzővel, melyet :attr:`__path__`
-nak hívunk. Ez egy lista, amelyben    az :file:`__init__.py` fájlt
tartalmazó könyvtár nevét találjuk --  mielőtt az aktuális fájlban lévő
kód végrehajtódna.  Ez egy módosítható változó, amely befolyásolja a
csomagban található modulok és alcsomagok keresését.

Bár erre a lehetőségre ritkán van szükség, a csomagot újabb modulokkal
egészíthetjük ki vele.

.. rubric:: Lábjegyzet

.. [#] Valójában a függvénydefinicíók is 'utasítások' amelyek 'végrehajtódnak';
   a modulszintű függvények végrehajtása behelyezi a függvény nevét a
   modul globális szimbólumtáblájába.

