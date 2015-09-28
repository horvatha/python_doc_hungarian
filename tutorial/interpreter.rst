.. _tut-using:

*****************************
A Python értelmező használata
*****************************


.. _tut-invoking:

Az értelmező elindítása
=======================

A Python értelemező szokásos helye a :file:`/usr/local/bin/python3` vagy
:file:`/usr/bin/python3` azokon a gépeken, ahol elérhető; helyezd el a
:file:`/usr/local/bin` útvonalat a Unix shell keresési útvonalán, akkor
egyszerűen a következőt írhatod a Unix shellbe a Python
indításához [#]_ ::

   python3

(Az oktató a Linuxot is a Unixokhoz sorolja, azokra is érvényesek az itt
leírtak. -- a fordító)

Gyakran több Python-változat is található a gépen, ilyenkor ezek
indíthatóak a következő formákban::

    python3.3
    python3.4

Mivel a telepítés helye telepítési opció, más helyen is lehet a program; ha nem
tudod az értelmezőt elindítani, kérj segítséget egy nagyobb tudású embertől.
(Például a :file:`/usr/local/python` is egy gyakori hely.)

Fájlvége karaktert (:kbd:`Control-D` Unix-on, :kbd:`Control-Z` Windows-on) írva
a Python elsődleges promptjába, az értelmező nulla kilépési státusszal (zero
exit status) lép ki.  Ha ez nem működik, akkor az értelmezőt a következő
utasítással hagyhatod el: ``quit()``.

Windows alatt a Python a telepítés során gyakran a :file:`C:\Python34`
könyvtárba kerül (3.4-es verzió esetén), bár ez módosítható a telepító
futtatásakor. Ha ezt a könyvtárat az útvonaladhoz szeretnéd adni, csak
írd a következő parancsot a a DOS-os parancssorba::

    set path=%path%;C:\python34

(Windowson a Python telepítésével települ az IDLE fejlesztői környezet,
amely a menüsorból indítható. Érdemes lehet a kezdeti lépéseket ezen
gyakorolni. Az IDLE Unix alá is telepíthető. -- a fordító)

Az értelmező sorszerkesztője nem túl kifinumult. Unix rendszereken
gyakran elérhető a GNU readline könyvtár, amely kifinomultabb
szerkesztési lehetőségeket szolgáltat és a már begépelt utasítások megjegyzését.
A szövegsori szerkesztés támogatottságának talán leggyorsabb ellenőrzése a
Control-P megnyomása az első megjelenő promptnál. Ha ez sípol, akkor
van; lásd az Appendix  :ref:`tut-interacting` fejezete bemutatja  az
érvényes billentyűket.  Ha semmi nem jelenik meg, vagy csak egy ``^P``,
akkor a szövegsori szerkesztés nem elérhető; ekkor csak a backspace
karakterrel törölhetsz karaktert az aktuális sorból.

Az értelmező a UNIX shellhez hasonlóan működik: ha a szabváyos
bemenettel van meghívva, akkor interaktívan olvas és hajt végre
utasításokat; ha fájlnév argumentummal  vagy egy fájllal, mint
szabványos bemenettel hívjuk meg, akkor *szkript*-ként lefuttatja a
megadott  fájlt.

Egy másik lehetőség az értelmező indítására a ``python -c parancs [arg] ...``
parancsor, amely végrehajtja az utasítás(oka)t a *parancs*-ban, hasonlóan a
shell :option:`-c` opciójához.  Mivel a Python utasítások gyakran tartalmaznak
szóközöket, vagy más speciális karaktereket, amelyeknek speciális jelentésük van
a shell számára, jobb, ha a *parancs* részt egyszeres idézőjelbe tesszük.

Némelyik Python modul önálló programként is használható, ha így hívod meg:
``python -m modul [arg] ...`` --  ez az utasítás végrehajtja a *modul*
forráskódját.

Érdemes megjegyezni, hogy különbség van a ``python file`` és  a ``python <file``
között. A második esetben a program beviteli kéréseit (amilyenek a :func:`input`
és :func:`raw_input` hívások) a program a *fájlból* veszi.  Mivel a fájlt az
elemző (parser) a fájl végéig beolvassa a program indulása előtt, a program
azonnal fájl-vége karakterrel fog találkozni. Az első esetben (amely gyakran a
kívánt működés)  a program azokat a fájlokat vagy eszközöket használja,
amelyek a Python értelmezőhöz csatlakoznak.

Sokszor hasznos, ha egy szkript fájl  futtatása után rögtön interaktív üzemmódba
kerülünk.  Ezt egyszerű elérni, a szkript neve előtt adjuk meg az :option:`-i`
kapcsolót.

.. _tut-argpassing:

Argumentum átadás
-----------------

Ha az értelmezőt szkript-fájllal indítjuk, akkor a szkript fájl neve  és a nevet
esetleg követő argumentumok a ``sys`` modul ``argv`` változójához
rendelődnek, ez egy karakterláncokból álló lista.  Ennek hossza leglább
1; amennyiben sem szkrip-nevet,  sem semmilyen argumentumot nem adunk
meg, a ``sys.argv[0]``  változó értéke üres string lesz.  Ha a szkript
neveként ``'-'``-t adunk meg  (ez a szabványos bemenetnek felel meg),
``sys.argv[0]`` értéke ``'-'`` lesz.  Argumentumként :option:`-c`
*parancs*-ot megadva ``sys.argv[0]``  értéke ``'-c'`` lesz.  Ha a
:option:`-m` *modul* formát hasznájuk, akkor a ``sys.argv[0]``
változóban a modul teljes neve kerül.  A :option:`-c` *parancs* vagy a
:option:`-m` *modul* után álló kapcsolókat az értelmező nem dolgozza
fel,  hanem a ``sys.argv``-ben eltárolva  a *parancs*-ra vagy *modul*-ra
hagyja annak feldolgozását.

.. _tut-interactive:

Interaktív (párbeszédes) mód
----------------------------

Amikor a parancs a tty-ről (pl. billentyűzet)  érkezik, az értelmező
úgynevezett *interaktív*  (párbeszédes) üzemmódban működik.  Ekkor az
*elsődleges prompt* megjelenítésével kéri a következő  parancs
megadását. Az elsődleges prompt szokásosan három egymás után  következő
nagyobb-jel (``>>>``); a sorok folytatásához  ezek a folytatólagos sorok
-- a *másodlagos promptot* jeleníti meg,  amelyek szokásos értéke három
egymás után írt pont (``...``).  Azt értelmező elindulásakor, mielőtt az
elsődleges prompt megjelenne,  egy üdvözlő-szöveget ír ki, amely
tartalmazza az értelmező verziószámát és a jogi védettséget::

   $ python3.4
   Python 3.4 (default, Sep 24 2012, 09:25:04)
   [GCC 4.6.3] on linux2
   Type "help", "copyright", "credits" or "license" for more information.
   >>>

Folytatólagos sorok szükségesek, ha többsoros szerkezeteket írsz be. Például a
következő :keyword:`if`  utasítás esetén::

   >>> a_vilag_sik = True
   >>> if a_vilag_sik:
   ...     print("Vigyázz, nehogy leess!")
   ...
   Vigyázz, nehogy leess!


.. _tut-interp:

Az értelmező és környezete
==========================


.. _tut-error:

Hibakezelés
-----------

Hiba esetén az értelmező hibaüzenetet küld és kiírja a hiba  nyomvonalát
(stack trace). Párbeszédes üzemmódban az elsődleges promt  jelenik meg;
ha az adatok beolvasása fájlból történt,  kiírja a hibanyomvonalat és
nullától eltérő kilépési értékkel tér vissza.  (A programon belül
fellépő és a program által kezelt kivételek (:keyword:`except` -
:keyword:`try` )  nem jelentenek hibát ebben a környezetben.) Vannak
feltétel nélküli,  úgynevezett végzetes hibák, amelyek azonnal,
nullától eltérő visszatérési értékkel történő programmegszakítást és
kilépést eredményeznek,  ilyenek a belső inkonzisztenciát okozó,
valamint a memóriából  kifutó programok hibái.  Minden hibaüzenet a
:keyword:`szabványos hibakimenetre` (:keyword:`standard error`) kerül, a
futtatott parancsok  rendes kimenete pedig a :keyword:`szabványos
kimenetre` (:keyword:`standard output`) íródik.

Ha az elsődleges, vagy a másodlagos promptnál  a megszakítás-karaktert gépeljük
be (rendszerint Control-C vagy a DEL billentyű),  az törli az eddigi bemenetet
és visszaadja a vezérlést az elsődleges promtnak.   [#]_

Ha a megszakításkérelmet valamely parancs/program végrehajtása alatt adjuk ki,
az :exc:`KeyboardInterrupt` kivételt generál;  ez programból a :keyword:`try`
utasítással 'kapható el'.


.. _tut-scripts:

Végrehajtható Python szkriptek
------------------------------

A BSD-szerű Unix rendszereken (Linuxon is) a Python-szkripteket ugyanúgy, mint a
shell szkripteket -  közvetlenül futtathatóvá lehet tenni, legelső sorként
megadva ezt ::

   #! /usr/bin/env python3

(feltételezve, hogy a Python értelmező elérési útvonala  a felhasználó
:envvar:`PATH` változójában be van állítva és  a szkript fájl végrehajtható
tulajdonsággal bír).

A fenti esetekben a fájlnak a ``#!`` karakterekkel kell kezdődnie.  Ezt a sort
néhány platformon Unix-típusú sorvéggel  (``'\n'``) kell lezárni, nem
pedig  Windows-os (``'\r\n'``) sorvéggel.  Megjegyezendő, hogy  a
``'#'`` karakterrel a Pythonban a megjegyzéseket kezdjük.

A szkriptnek a :program:`chmod` paranccsal adhatunk  végrehajtási engedélyt az
alábbi módon::

   $ chmod +x szkriptem.py

A Windows rendszeren nincs "végrehajtható" mód. A Python telepítő
automatikusan a ``.py`` fájlokat a ``python34.exe`` utasításhoz
társítja, így egy Python-fájlon duplán kattintva az elindul szkriptként.
A kiterjesztés ``.pyw`` is lehet, ilyenkor a konzolablak, amely nem fog
megjelenni. , in that case, the console window that normally appears is
suppressed.

A forráskód karakterkészlete
----------------------------

Alapállapotban a Python forrásfájlokat UTF-8-as kódolásúnak tekintjük.
Ebben a karakterkódolásban a legtöbb nyelv karakterei használhatóak
egyszerre egy sztringben, változónévben és megjegyzésben. A standard
könyvtár csak ASCII karaktereket használ változónévnek: ezt a
megállapodást minden hordozható kódnak követnie kellete.
Ahhoz, hogy az összes karakter helyesen jelenjen meg, az
szövegszerkesztőnek fel kell ismernie, hogy a fájl UTF-8-as, és olyan
betűtípust kell használnia, amely tartalmazza a fájlban szereplő összes
karaktert.

Lehetséges ettől eltérő kódolást is megadni a forrásfájlban. Hogy ezt
megtehessük, még egy speciális megjegyzés-sort helyezzünk a ``#!`` sor
után, hogy definiáljuk a kódolást::

   # -*- coding: kódolás -*-

A kódolás megadásával a forrásfájlban mindent *kódolás* kódolással
kerül értelmezésre UTF-8 helyett. A lehetséges kódolások a Python
Library Reference-ben találhatóak a :mod:`codecs` szakaszban.

Példáuk, ha a kiválasztott szövegszerkesztő nem támogatja az UTF-8
kódolást, és más kódolásra, mondjuk Windows-1252 kódolásra van szükség,
az alábbi írható::

   # -*- coding: cp-1252 -*-

és máris az összes Windows-1252-ben található karakter használható a
forrásfájlban.  A speciális kódolás-megjegyzésnek az *első vagy második*
sorban kell lennie.

.. _tut-startup:

Indítófájl párbeszédes üzemmódban
---------------------------------

Párbeszédes (interaktív) üzemmódban használva a Pythont,  sokszor kívánatos,
hogy az értelmező futtatása előtt bizonyos  parancsok mindig lefussanak.  Ez a
:envvar:`PYTHONSTARTUP` nevű környezeti változó értékének  megadásával
történhet; itt kell megadni a futtatni kívánt parancsfájl nevét.  Ez a megoldás
azonos a Unix :file:`.profile` fájl megoldásával.

Az indítófájl beolvasása csak párbeszédes üzemmódban történik;  nem történik
beolvasás ha a parancsok  fájlból jönnek,  vagy ha bemenetként a
:file:`/dev/tty` explicit módon lett megadva  (amelyet egyébként párbeszédes
üzemmódban az értelmező alaphelyzetben használ).  Az indítófájl a futása során
ugyanazt a névterületet  (name space) használja mint a párbeszédes üzemmód,  így
a benne definiált vagy bele importált objektumok változatlanul,  további
pontosítás nélkül használhatók párbeszédes üzemmódban is.  Ebben a fájlban a
``sys.ps1`` és ``sys.ps2``  értékeinek átírásával változtathatod a promtok
értékeit.

Amennyiben az aktuális könyvtárból egy másik indítófájlt is szeretnél futtatni,
megteheted a globális indítófájl szerkesztésével, ahogy ezt az alábbi példa
mutatja:  ``if os.path.isfile('.pythonrc.py'): execfile('.pythonrc.py')``. ::

   import os
   filename = os.environ.get('PYTHONSTARTUP')
   if filename and os.path.isfile(filename):
       execfile(filename)

.. rubric:: Lábjegyzet

.. [#] On Unix, the Python 3.x interpreter is by default not installed with the
   executable named ``python``, so that it does not conflict with a
   simultaneously installed Python 2.x executable.

.. [#] A problem with the GNU Readline package may prevent this.
