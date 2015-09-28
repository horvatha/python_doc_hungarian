.. _tut-errors:

******************
Hibák és kivételek
******************

Eddig csak említettük a hibajelzéseket, de ha kipróbáltad a példákat feltehetően
láttál néhányat.  Kétfajta megkülönböztetendő hibafajta van:
*szintaktikai hiba (syntax error)* és  *kivétel (exception)*.

.. _tut-syntaxerrors:

Szintaktikai hibák
==================

A szintaktikai hibák, vagy másképp elemzési hibák, talán a leggyakoribb
hibaüzenetek, amíg tanulod a Pythont::

   >>> while 1 print('Hello világ')
     File "<stdin>", line 1
       while 1 print('Hello világ')
                   ^
   SyntaxError: invalid syntax

Az elemző megismétli a hibás sort, és kitesz egy kis ,,nyilat" amely a sorban
előforduló legelsőnek észlelt hibára mutat.  A hibát a nyilat *megelőző* szócska
(token) okozza (vagy legalábbis itt észlelte az értelemező): a példában, a hibát
a :keyword:`print` utasításnál észlelte, mivel a kettőspont (``':'``) hiányzik
előle.  A fájl neve és a sorszám  kiíródik, így tudhatod, hol keressed, ha egy
szkriptet futtattál.

.. _tut-exceptions:

Kivételek
=========

Ha egy állítás vagy kifejezés szintaktikailag helyes, akkor is okozhat hibát, ha
megpróbáljuk végrehajtani. A végrehajtás során észlelt hibákat *kivételeknek
(execptions)* nevezzük és nem feltétlenül végzetesek: nemsokára megtanulod,
hogyan kezeld ezeket a Python programban.  A legtöbb kivételt általában nem
kezelik a programok, ekkor az alábbiakhoz hasonló hibaüzeneteket adnak::

   >>> 10 * (1/0)
   Traceback (most recent call last):
     File "<stdin>", line 1
   ZeroDivisionError: integer division or modulo
   >>> 4 + spam*3
   Traceback (most recent call last):
     File "<stdin>", line 1
   NameError: spam
   >>> '2' + 2
   Traceback (most recent call last):
     File "<stdin>", line 1
   TypeError: illegal argument type for built-in operation

A hibaüzenetek utolsó sora mutatja, mi történt.  Különböző típusú kivételeket
kaphatunk, és a típus az üzenet részeként kerül kiíratásra: a típusok a
példákban:  :exc:`ZeroDivisionError` (nullával való osztás), :exc:`NameError`
(név hiba) és :exc:`TypeError` (típussal kapcsolatos hiba). A kivétel típusaként
kiírt szöveg a fellépő kivétel beépített (built-in) neve. Ez minden belső kivételre
igaz, de nem feltétlenül igaz a felhasználó által definiált kivételekre (habár
ez egy hasznos megállapodás). A szokásos kivételnevek belső azonosítók (nem
fenntartott kulcsszavak).

A sor fennmaradó része egy részletezés, amelynek alakja a kivétel fajtájától
függ.

Az ezt megelőző része a hibaüzenetnek megmutatja a szövegkörnyezetet -- ahol a
kivétel történt -- egy veremvisszakövetés (stack backtrace) formájában.
Általában ez veremvisszakövetést tartalamazza egy listában a forrás sorait.
jóllehet, ez nem fog megjeleníteni a sztenderd bementeről kapott sorokat.

A Python Library Reference 
felsorolja a belső kivételeket és a jelentéseiket.

.. _tut-handling:

Kivételek kezelése
==================

Van rá lehetőség, hogy olyan programokat írjunk, amik kezelik a különböző
kivételeket (exception). Nézzük csak a következő példát, amely addig kérdezi a
felhasználótól az értéket amíg egy egész számot nem ír be, de megengedi a
felhasználónak, hogy megszakítsa a program futását (a :kbd:`Control-C`
használatával, vagy amit az operációs rendszer támogat). Jegyezzük meg, hogy a
felhasználó által létrehozott megszakítás :exc:`KeyboardInterrupt` kivételként
lép fel.  ::

   >>> while 1:
   ...     try:
   ...         x = int(raw_input("Írj be egy számot: "))
   ...         break
   ...     except ValueError:
   ...         print("Ez nem egész szám. Próbáld újra... ")
   ...     

A :keyword:`try` utasítás a következőképpen működik.

* Először a *try mellékág* hajtódik végre, azaz a :keyword:`try` és
  :keyword:`except` közötti utasítások.

* Ha nem lép fel kivétel, az *except ágat* a program átugorja, és a
  :keyword:`try` utasítás befejeződik.

* Ha kivétel lép fel a try ág végrehajtása során, az ág maradék része nem
  hajtódik végre. Ekkor -- ha a típusa megegyezik az :keyword:`except` kulcsszó
  után megnevezett kivétellel, ez az expect ág hajtódik végre, és ezután a
  végrehajtás a try--except blokk után folytatódik.

* Ha olyan kivétel lép fel, amely nem egyezik a az expect ágban megnevezett
  utasítással, akkor a kivételt átadja egy külsőbb :keyword:`try` utasításnak. Ha
  nincs kezelve a kivétel, akkor ez egy *kezeletlen kivétel* a futás megáll egy
  utasítással, ahogy korábban láttuk.

A :keyword:`try` utasításnak lehet egynél több except ága is, hogy a különböző
kivételeket kezelhessük. Egynél több kezelő hajtódhat végre. A kezelők csak a
hozzájuk tartozó try ágban fellépő kivételt kezelik a :keyword:`try` utasítás
másik kezelőjében fellépő kivételt nem.  Egy expect ágat több névvel is
illethetünk egy kerek zárójelbe tett lista segítségével, például::

   ... except (RuntimeError, TypeError, NameError):
   ...     pass

(Magyarul: Futásidejű hiba, TípusHiba, NévHiba)

Az utolsó expect ág esetén elhagyható a kivétel neve.  Rendkívül óvatosan
használjuk, mert elfedhet valódi programozási hibákat!  Arra is használható,
hogy kiírjunk egy hibaüzenetet, majd újra kivételdobást hajtsunk végre,
(megengedve a hívónak, hogy lekezelje a kivételt)::

   import string, sys

   try:
       f = open('file.txt')
       s = f.readline()
       i = int(string.strip(s))
   except IOError, (errno, strerror):
       print("I/O error(%s): %s" % (errno, strerror))
   except ValueError:
       print("Nem képes az adatot egész számmá alakítani.")
   except:
       print("Váratlan hiba:", sys.exc_info()[0])
       raise

A :keyword:`try` ... :keyword:`except` utasításnak lehet egy  *else ága* is,
amelyet -- ha létezik -- az összes except ágnak meg kell előznie. Ez nagyon
hasznos olyan kódokban, amelyeknek mindenképpen végre kell hajtódniuk, ha a try
ágban nem lép fel kivétel. Például::

   for arg in sys.argv[1:]:
       try:
           f = open(arg, 'r')
       except IOError:
           print('nem nyitható meg', arg)
       else:
           print(arg, len(f.readlines()), 'sorból áll.')
           f.close()

Az :keyword:`else` ág használata jobb, mintha a kódot a :keyword:`try` ághoz
adnánk, mivel ez elkerüli egy olyan kivétel elkapását, amely nem a
:keyword:`try` ... :keyword:`except` utasítások által védett ágban vannak.

Ha kivétel lép fel, lehetnek hozzátartozó értékei, amelyeket a kivétel
*argumentumának* is nevezünk. A megléte és a típusa a kivétel fajtájától is
függ. Azokra a kivételtípusokra, amelyek argumentummal rendelkeznek, az except
ág előírhat egy változót a kivétel neve (vagy a lista) után, amely felveszi  az
argumentum értékét, ahogy itt látható::

   >>> try:
   ...     spam()
   ... except NameError, x:
   ...     print('A(z)', x, 'név nincs definiálva.')
   ... 
   A(z) spam név nincs definiálva.

Ha a kivételnek argumentuma van, az mindíg utolsó részeként  kerül a képernyőre.

A kivételkezelők nem csak akkor kezelik a kivételeket, ha azok ténylegesen a try
ágban szerepelnek, hanem akkor is, ha azok valamelyik try ágban meghívott
függvényben szerepelnek (akár közvetve is). Például::

   >>> def ez_rossz():
   ...     x = 1/0
   ... 
   >>> try:
   ...     ez_rossz()
   ... except ZeroDivisionError, detail:
   ...     print('Handling run-time error:', detail)
   ... 
   Handling run-time error: integer division or modulo

.. _tut-raising:

Kivételek létrehozása
=====================

A :keyword:`raise` utasítás lehetővé teszi a programozó számára, hogy egy új,
általa megadott kivételt hozzon létre. Például::

   >>> raise NameError('IttVagyok')
   Traceback (most recent call last):
     File "<stdin>", line 1
   NameError: IttVagyok

A :keyword:`raise` első argumentuma a kivétel neve amit létrehozunk. Az
esetleges második argumentum adja meg a kivétel argumentumát.

.. _tut-userexceptions:

Felhasználó által létrehozott kivételek
=========================================

A programok elnevezhetik a saját kivételeiket, ha karakterláncot rendelnek egy
változóhoz, vagy egy új kivétel-osztályt hoznak létre. Például::

   >>> class MyError:
   ...     def __init__(self, value):
   ...         self.value = value
   ...     def __str__(self):
   ...         return `self.value`
   ... 
   >>> try:
   ...     raise MyError(2*2)
   ... except MyError, e:
   ...     print('A kivételem fellépett, értéke:', e.value)
   ... 
   A kivételem fellépett, értéke: 4
   >>> raise MyError, 1
   Traceback (most recent call last):
     File "<stdin>", line 1
   __main__.MyError: 1

Sok -- a Pythonban megtalálható -- modul ezt használja a függvényekben
előforduló esetleges hibák megjelenítésére.

Bővebb információ az osztályokról a  :ref:`tut-classes` fejezetben található.

.. _tut-cleanup:

Takarító-lezáró műveletek definiálása
=====================================

A :keyword:`try` utasításnak van egy másik opcionális ága, mely
takarító-rendberakó műveletek tárolására szolgál -- ezeket a megelőző ágak
lefutása után kell végrehajtani. Például::

   >>> try:
   ...     raise KeyboardInterrupt
   ... finally:
   ...     print('Viszlát világ!')
   ... 
   Viszlát világ!
   Traceback (most recent call last):
     File "<stdin>", line 2
   KeyboardInterrupt

A *végső záradék* akár kivételdobás történt a :keyword:`try` záradékban, akár
nem -- mindenképpen végrehajtódik. Ha kivételdobás történt, a kivétel a
:keyword:`finally` záradék végrehajtása után újra kiváltódik.  A finally -- a
kivételdobás utószeleként, lefutása végeként -- hajtódik végre, amikor a
:keyword:`try` utasítás elhagyja a  :keyword:`break` vagy a :keyword:`return`
utasítást.  (Fontos: függvényeknél ha nem helyezünk el :keyword:`return`
utasítást, akkor rejtetten egy :keyword:`return None` utasítás fut le -- tehát
ha nem látunk egy utasítást, az esetleg rejtetten akkor is lefuthat!)

A :keyword:`try` utasítás egy vagy több except záradékot tartalmazhat, vagy egy
finally záradékot, de a kettőt együtt nem.

