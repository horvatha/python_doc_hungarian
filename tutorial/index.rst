.. _tutorial-index:

######################
  Python-oktató
######################

A Python egy könnyen tanulható, sokoldalú programozási nyelv. Jól
használható, magas szintű adatstruktúrákkal rendelkezik, valamint  az
objektum-orientált programozás egyszerű, mégis hatékony
megvalósításával. A Python elegáns nyelvtana, a nyelv dinamikusan
típusossága, valamint parancsértelmező jellege ideális szkriptek
készítésére és gyors alkalmazásfejlesztésre több platformon, szerteágazó
területeken.

A Python értelmező és a sokrétű, alaposan kidolgozott alap-könyvtár (Standard
Library) szabadon elérhető és felhasználható akár forráskódként, akár bináris
formában minden jelentősebb platformra a Python weboldaláról:
`<http://www.python.org/>`_. Ugyanitt hivatkozások is vannak külső fejlesztésű
modulokra, programokra és eszközökre, és kiegészítő dokumentációkra.

A Python értelmező egyszerűen kiegészíthető új függvényekkel és
adattípusokkal C vagy C++ nyelven (vagy más, C-ből hívható nyelven). A
Python maga is alkalmas kiegészítő nyelv már elkészült alkalmazások
bővítéséhez.

Ez az oktató megismerteti az olvasóval a Python alapvető gondolkodásmódját,
nyelvi lehetőségeit és rendszerét. Segít a gyakorlott felhasználóvá válásban. A
példák a dokumentumba ágyazottak, internet elérés nem szükséges az olvasásukhoz.

..
        A nyelvbe épített szabványos objektumok és modulok leírásához nézd meg a
        `Python Library Reference (Python szabványos könyvtár)
        <http://pythonlib.pergamen.hu>`_ készülő magyar fordítását (és ha van kedved,
        segíts a fordításban!).

A Python Reference Manual  a nyelv részletes definícióját tartalmazza. C
vagy C++ nyelven írt kiegészítések létrehozásához hasznos olvasnivaló az
Extending and Embedding the Python Interpreter és a Python/C API
Reference.  Ezenkívül van néhány könyv, amely a Pythonnal komolyan
foglalkozik.

Ez nem egy minden apró részletre kiterjedő oktatóanyag. Bemutatja  a
Python legfontosabb lehetőségeit, és megismerteti veled a nyelv
gondolkodásmódját és stílusát. Ha végigcsinálod, képes leszel Python
modulok és programok írására, és ami a legfontosabb: a további
tanulásra.  Az oktató után érdemes a  :ref:`library-index` fejezetben
leírt modulokkal megismerkedni.

A :ref:`glossary` fejezetet is érdemes átolvasni.

A magyar fordítással kapcsolatban a :ref:`tut-hungarian` fejezetben
olvashatsz.

.. toctree::
   :numbered:

   appetite.rst
   interpreter.rst
   introduction.rst
   controlflow.rst
   datastructures.rst
   modules.rst
   inputoutput.rst
   errors.rst
   classes.rst
   stdlib.rst
   stdlib2.rst
   whatnow.rst
   interactive.rst
   floatingpoint.rst

.. _tut-hungarian:

Pár szó a magyar fordításról
============================

A fordítást Horváth Árpád <horvath.arpad.szfvar kukac gmail.com> és
Nyírő Balázs <diogenesz at pergamen.hu> készítette.  A fordítás a 2.1-es
verziójú tutoriallal kezdődött. Ezt konvertáltam át LaTeX forrásból
reStructuredText-be.  Még lehetnek olyan részek, amelyek csak a
Python2-es verziójával megy.  Kérem ezeket, és más tévedéseket,
elírásokat, fordítási hibákat jelezzék Horváth Árpádnak.

Angol szavak/kifejezések magyar megfelelői
------------------------------------------

A tuple szóra nem találtunk igazán jó magyar megfelelőt. A Python3
könyvben szereplő rendezett sorozat kifejezés kissé hosszadalmasnak
tűnik számunkra, ezért maradtunk a tuple kifejezésnél, amelyet az angol
kiejtésnek megfelelően ragoztunk.


Szótár
------

+-----------------------------+-------------------------------------+
| angol                       | magyar                              |
+=============================+=====================================+
| argument                    | argumentum                          |
+-----------------------------+-------------------------------------+
| built-in (function)         | beépített (függvény)                |
+-----------------------------+-------------------------------------+
| control flow statements     | vezérlő utasítások                  |
+-----------------------------+-------------------------------------+
| string-literal              | literális karakterlánc              |
+-----------------------------+-------------------------------------+
| backslash                   | vissza-per jel                      |
+-----------------------------+-------------------------------------+
| positional argument         | hely szerinti argumentum            |
+-----------------------------+-------------------------------------+
| keyword argument            | kulcsszavas argumentum              |
+-----------------------------+-------------------------------------+
| prompt                      | prompt                              |
+-----------------------------+-------------------------------------+
| tuple                       | tuple  tuple-t, tuple-nek...        |
+-----------------------------+-------------------------------------+
| sequence                    | sorozat                             |
+-----------------------------+-------------------------------------+
| object                      | objektum                            |
+-----------------------------+-------------------------------------+
| attribute (osztály)         | jellemző                            |
+-----------------------------+-------------------------------------+
| data attribute              | adatjellemző                        |
+-----------------------------+-------------------------------------+
| method                      | metódus                             |
+-----------------------------+-------------------------------------+
| exception                   | kivétel                             |
+-----------------------------+-------------------------------------+
| raise an exception          | kivételt vált ki                    |
+-----------------------------+-------------------------------------+
| clause                      | ág                                  |
+-----------------------------+-------------------------------------+
| handler (az exeption-öknél) | kezelő                              |
+-----------------------------+-------------------------------------+
| token                       | token                               |
+-----------------------------+-------------------------------------+
| default                     | alapértelmezett                     |
+-----------------------------+-------------------------------------+
| scope                       | hatáskör, hatókör                   |
+-----------------------------+-------------------------------------+
| keyword argument            | kulcsszavas argumentum              |
+-----------------------------+-------------------------------------+
| right-hand side expression  | jobboldali kifejezés                |
+-----------------------------+-------------------------------------+
| sequence unpacking          | a sorozat szétpakolása              |
+-----------------------------+-------------------------------------+
| shortcut operators          | ???                                 |
+-----------------------------+-------------------------------------+
| slice notation              | szelet jelölési mód                 |
+-----------------------------+-------------------------------------+
| List Comprehensions         | listaértelmezés                     |
+-----------------------------+-------------------------------------+
| Library reference           | Referencia könyvtár                 |
+-----------------------------+-------------------------------------+


