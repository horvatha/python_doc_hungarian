.. _tut-brieftour:

*************************************************************
A Python alap-könyvtár rövid bemutatása - Standard Library 1.
*************************************************************

.. _tut-os-interface:

Felület az operációs rendszerhez
================================

Az :mod:`os`  modul nagyon sok függvényt
tartalmaz, melyek az operációs rendszerrel kommunikálnak::


   >>> import os
   >>> os.system('time 0:02')  		# FIGYELEM: átállítottuk a rendszer óráját!
   0
   >>> os.system('time')
   A pontos idő: 0:02:08,14
   Írja be az új időt:



   >>> os.getcwd()      			# Az aktuális könyvtár nevét adja vissza.
   'C:\\Python24'
   >>> os.chdir('/server/accesslogs')      # és itt könyvtárar váltottunk.

Fontos, hogy az importálás során az  ``import os`` alakot használd,  és ne a
``from os import *`` alakot.  Ez megóv attól, hogy az :func:`os.open`  függvény
elfedje (és használhatatlanná tegye)  a beépített :func:`open` függvényt, ami
teljesen másképp működik.

.. index:: builtin: help

A beépített :func:`dir` és :func:`help` függvények  sokat segíthetnek ha olyan
nagy modulokkal van dolgod, mint például az :mod:`os`::

   >>> import os
   >>> dir(os)
   <kilistázza az összes függvényt, ami az 'os' modulban található>
   >>> help(os)
   <egy nagyon részletes manual oldalt generál a modulok dokumentációs karakterláncából>

A mindennapi fájl- és könyvtár-műveletekhez az :mod:`shutil`  modul
magasszintű, könnyen használható felületet nyújt::

   >>> import shutil
   >>> shutil.copyfile('data.db', 'archive.db')
   >>> shutil.move('/build/executables', 'installdir')

.. _tut-file-wildcards:

Karakterhelyettesítő jelek -- dzsóker karakterek
================================================

A :mod:`glob`  modulban lévő függvény
segít a fájl listák elkészítésében, ha dzsóker  karaktert használsz::

   >>> import glob
   >>> glob.glob('*.py')
   ['primes.py', 'random.py', 'quote.py']

.. _tut-command-line-arguments:

Parancssori paraméterek
=======================

A programoknak gyakran fel kell dolgozniuk a  parancssori paramétereiket. Ezek a
paraméterek a :mod:`sys` modul *argv* attribútumában tárolódnak,
listaknént. Például ha kiadjuk azt a parancsot, hogy ``python demo.py
egy ketto harom``, az a következő kimenetet eredményezi::

   >>> import sys
   >>> print(sys.argv)
   ['demo.py', 'egy', 'ketto', 'harom'] # a nulladik argumentum mindig a program neve!

A :mod:`getopt`  modul képes feldolgozni a *sys.argv* elemeit a  Unix
:func:`getopt` függvényének szabályai szerint.   Ennél még hatékonyabb
és rugalmasabb program-paraméter feldolgozást tesz lehetővé az
:mod:`optparse` modul.

.. _tut-stderr:

Hiba-kimenet átirányítása, programfutás megszakítása
====================================================

The :mod:`sys`  module also has
attributes for *stdin*, *stdout*, and *stderr*.  The latter is useful for
emitting warnings and error messages to make them visible even when *stdout* has
been redirected:

A :mod:`sys`  modul szintén rendelkezik
*stdin*, *stdout*, és  *stderr* attribútummal.  Ez utóbbi használatos
figyelmeztetések és hibaüzenetek láthatóvá tételére -- például akkor, amikor a
*stdout* át van irányítva, mondjuk egy fájlba::

   >>> sys.stderr.write('Warning, log file not found starting a new one\n')
   Warning, log file not found starting a new one

A legrövidebb út egy program megszakítására  a ``sys.exit()`` utasítás.

.. _tut-string-pattern-matching:

Reguláris kifejezések - karakterláncok
======================================

A :mod:`re`  modul segítségével reguláris kifejezéseket használhatsz
szövegfeldolgozásra. Összetett illeszkedési és módosító szabályokat
határozhatsz meg -- a reguláris kifejezések rövid, tömör megoldást
kínálnak::

   >>> import re               # kovetkezik: minden f-el kezdodo szot kigyujtunk:
   >>> re.findall(r'\bf[a-z]*', 'aki felveszi, az felmelegszik, aki nem, az fazik')
   ['felveszi', 'felmelegszik', 'fazik']

   >>> re.sub(r'(\b[a-z]+) \1', r'\1', 'macska a a kalapban a a hazban')
   'macska a kalapban a hazban'    # a pelda nem tokeletes, 'a a a'-bol 
   				# 'a a'-t csinal, de szemleltetesnek jo

Ha egyszerűbb szövegmódosítási igényed van, a string metódusokat javasoljuk,
mert olvashatóak és a hibakeresés is könyebb velük::

   >>> 'Teat Peternek'.replace('Peter', 'Elemer')
   'Teat Elemernek'

.. _tut-mathematics:

Matematika
==========

A :mod:`math` modulon keresztül érhetőek el a háttérben működő C
függvények, melyekkel  lebegőpontos műveleteket végezhetsz::

   >>> import math
   >>> math.cos(math.pi / 4.0)
   0.70710678118654757
   >>> math.log(1024, 2)
   10.0

A :mod:`random` modullal véletlenszámokat generálhatsz::

   >>> import random
   >>> random.choice(['alma', 'korte', 'banan'])
   'alma'
   >>> random.sample(range(100), 10)   # ismetles nelkuli mintavetel
   [30, 83, 16, 4, 8, 81, 41, 50, 18, 33]
   >>> random.random()    # random float
   0.17970987693706186
   >>> random.randrange(6)    # véletlen egész szám kiválasztása 0-6ig terjedő tartományban
   4   

.. _tut-internet-access:

Internet elérés
===============

Több modul is van, amely lehetővé teszi az Internet elérését, és
különböző protokollok használatát.  A két legegyszerűbb az
:mod:`urllib2` -- adatfogadás url címekről, és az  :mod:`smtplib` modul,
amellyel levelet küldhetsz::

   >>> import urllib2                
   >>> for line in urllib2.urlopen('http://www.python.org/'):
                                     # for: az oldal soronkénti feldolgozása:
   ...     if 'Python' in line:      # keressük azokat a sorokat, 
   ...         print(line)           # ahol a Python szó megtalálható


   >>> import smtplib
   >>> server = smtplib.SMTP('localhost')
   >>> server.sendmail('soothsayer@example.org', 'jcaesar@example.org',
   """To: jcaesar@example.org
   From: soothsayer@example.org

   Szevasz! Eljutottal a tutorial vegeig!.
   """)
   >>> server.quit()

.. _tut-dates-and-times:

A dátumok és az idő kezelése
============================

A :mod:`datetime`  modul biztosít osztályokat a dátumok és az időpontok
manipulálására --  egyszerűbbeket és összetettebbeket is. A dátum- és az
idő- aritmetikai műveletek támogatottak -- a középpontban  a kimenet
formázása és módosítása áll. A modul támogatja azokat az objektumokat
is, amelyek kezelni tudják az időzónákat.  ::

   # a dátumok könnyen létrehozhatóak és formázhatóak:
   >>> from datetime import date
   >>> most = date.today()
   >>> most
   datetime.date(2003, 12, 2)
   >>> most.strftime("%m-%d-%y. %d %b %Y is a %A on the %d day of %B.")
   '12-02-03. 02 Dec 2003 is a Tuesday on the 02 day of December.'

   # a dátumok támogatják a naptári műveleteket:
   >>> szuletesnap = date(1964, 7, 31)
   >>> kor = most - szuletesnap  # a most-ot az elozo peldaban hataroztuk meg!
   >>> kor.days   # days = napok, itt a napok szamat jelenti
   14368

.. _tut-data-compression:

Tömörítés - zip, gzip, tar...
=============================

Az elterjedtebb archiváló és tömörítő formátumok közvetlenül
támogatottak, a következő modulokban: :mod:`zlib`, :mod:`gzip`,
:mod:`bz2`, :mod:`zipfile`, and :mod:`tarfile`.

::

   >>> import zlib
   >>> s = 'witch which has which witches wrist watch'
   >>> len(s)
   41
   >>> t = zlib.compress(s)
   >>> len(t)
   37
   >>> zlib.decompress(t)
   'witch which has which witches wrist watch'
   >>> zlib.crc32(s)
   226805979

.. % \section{Performance Measurement\label{performance-measurement}}


.. _tut-performance-measurement:

Teljesítménymérés
=================

Néhány Python programozó komoly érdeklődést mutatott a különböző probléma-
megoldások teljesítményének összehasonlítása iránt.  A Pythonban található egy
mérőeszköz, amely azonnali választ ad ezekre a kérdésekre.

Például használhatunk tuple becsomagolást és kicsomagolást  a megszokott
paraméter-átadás helyett. A :mod:`timeit` modul gyorsan demonstrál egy
egyszerű teljesítmény mérést::

   >>> from timeit import Timer
   >>> Timer('t=a; a=b; b=t', 'a=1; b=2').timeit()
   0.57535828626024577
   >>> Timer('a,b = b,a', 'a=1; b=2').timeit()
   0.54962537085770791

A :mod:`timeit` modul apró kódrészletek végrehajtási idejének mérésére szolgál.
Ezzel ellentétben a :mod:`profile` és a :mod:`pstats` modulok nagyobb
kódrészletek futási-idő kritikus részeinek meghatározására szolgál.


.. _tut-quality-control:

Minőségellenőrzés
=================

A jóminőségű programok fejlesztésnek egyik elmélete az, hogy minden függvényhez
próbaadatokat, teszteket írunk -- majd a fejlesztési folyamat során ezeket
gyakran  lefuttatjuk - így azonnal kiderül, ha a várttól eltérően viselkedik a
program.

A The :mod:`doctest`  modul tartalmaz olyan eszközöket, amelyekkel
modulokat vizsgálhatunk, és  a program dokumentációs karakterláncába
ágyazott teszteket futtathatunk le. A teszt létrehozása olyan egyszerű,
mint kivágni és beilleszteni  egy tipikus függvényhívás során
bejövő-keletkező adatokat.

Ez a lehetőség elősegíti a jobb dokumentáltságot, hiszen a felhasználónak
rögtön függvényhívási példát mutathatunk -- továbbá ellenőrizhetővé teszi a
doctest modulnak, hogy a kód a dokumentációval összhangban van-e. ::

   def atlag(ertekek):
       """Listában átadott számok számtani közepét határozza meg a függvény.

       >>> print(atlag([20, 30, 70]))
       40.0
       """
       return sum(ertekek) / len(ertekek)

   import doctest
   doctest.testmod()   # a beágyazott tesztet automatikusan kipróbálja. 

A :mod:`unittest` modul kicsit bonyolultabb, mint  a :mod:`doctest`
modul -- viszont több átfogó tesztkészlet kezeléséről gondoskodik, egy
különálló fájlban::

   import unittest

   class StatisztikaiFuggvenyekTesztelese(unittest.TestCase):

       def atlag_tesztelese(self):
           self.assertEqual(atlag([20, 30, 70]), 40.0)
           self.assertEqual(round(atlag([1, 5, 7]), 1), 4.3)
           with self.assertRaises(ZeroDivisionError):
               atlag([])
           with self.assertRaises(TypeError):
               atlag(20, 30, 70)

   unittest.main() # A parancssorból történő hívás lefuttatja a teszteket.


.. _tut-batteries-included:

Elemekkel együtt...
===================

A Python filozófiája: "elemekkel együtt". A legjobban ez úgy látszik,  ha
észrevesszük nagyszámú moduljainak - csomagjainak kifinomultságát,
összetettségét.

Például:


* Az :mod:`xmlrpclib` és a :mod:`SimpleXMLRPCServer` modulok a távoli
  eljáráshívásokat egyszerű műveletté teszik számunkra. A neveik
  ellenére nincs közvetlen XML tudásra szükség.

* Az :mod:`email` csomag egy  könyvtár az elektronikus levelek
  kezelésére -- beleértve a MIME és más RFC 2822-alapú üzeneteket is.
  Eltérően az :mod:`smtplib` és :mod:`poplib` moduloktól, melyek
  azonnali levélküldést és fogadást valósítanak meg,  az  email csomag
  teljes eszközkészlettel rendelkezik  összetett üzenet-struktúrák
  felépítéséhez és dekódolásához -- a csatolt állományokat  is
  beleértve. Továbbá tartalmazza az Interneten használt kódoló és fejléc
  protokollokat.

* Az :mod:`xml.dom` és az :mod:`xml.sax`  csomagok nagyon jól
  használhatók az elterjedt adat-cserélő formátumok kezelésére,
  értelmezésére és feldolgozására Ugyanúgy a :mod:`csv`  modul támogatja
  a csv formátum közvetlen írását és olvasását. Mindent egybevéve ezek a
  modulok és csomagok remekül leegyszerűsíti a Python programok és más
  alkalmazások közötti adatcserét.

* A kultúrális tulajdonságok beállíthatók és támogatottak számos
  modulban, például:  :mod:`gettext`, :mod:`locale`, és a  :mod:`codecs`
  csomagban is.

