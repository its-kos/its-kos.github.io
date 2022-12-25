.. _manual:

Οδηγίες χρήσης της Beautiful Soup
============================

.. image:: 6.1.jpg
   :align: right
   :alt: "Το Ψάρι-υπηρέτης ξεκίνησε βγάζοντας κάτω απο το χέρι του ένα μεγάλο γράμμα, σχεδόν όσο μεγάλο όσο το ίδιο."

Η `Beautiful Soup <http://www.crummy.com/software/BeautifulSoup/>`_ είναι μια βιβλιοθήκη
της Python για εξαγωγή δεδομένων απο αρχεία HTML και XML. Δουλεύει με τον αγαπημένο σας 
αναλυτή (parser) για να παράξει ιδιωματικούς τρόπους πλοήγησης, αναζήτησης και τροποποίησης
του δέντρου ανάλυσης (parse tree). Συνήθως γλυτώνει σε προγραμματιστές ώρες ή και μέρες δουλειάς.

Αυτές οι οδηγίες επεξηγούν όλες τις κύριες λειτουγίες της Beautiful Soup 4, 
με παραδείγματα. Σας δείχνουν τι κάνει αυτή η βιβλιοθήκη καλά, πώς δουλεύει, 
πως να τη χρησιμοποιήσετε, πως να την κάνετε να κάνει αυτό που θέλετε, και τι να κάνετε όταν
δεν δουλέυει όπως περιμένετε.

Αυτές οι οδηγίες χρήσης καλύπτουν την Beautiful Soup έκδοση 4.11.0. Τα παραδείγματα σε
αυτές τις οδηγίες είναι γραμμένα για Python 3.8.

Υπάρχει περίπτωση να αναζητείτε οδηγιες για την `Beautiful Soup 3
<http://www.crummy.com/software/BeautifulSoup/bs3/documentation.html>`_.
Αν ναι, θα πρέπει να γνωρίζετε οτι η έκδοση 3 δεν βρίσκεται πλέον 
υπο ανάπτυξη και η υποστήριξη γι' αυτή σταμάτησε στις 31 Δεκεμβρίου, 2020.
Αν θέλετε να μάθετε τις διαφορές μεταξύ των εκδόσεων 3 και 4, δείτε `Porting code to BS4`_.

Αυτές οι οδηγίες έχουν μεταφραστεί σε άλλες γλώσσες απο
χρήστες της Beautiful Soup:

* `这篇文档当然还有中文版. <https://www.crummy.com/software/BeautifulSoup/bs4/doc.zh/>`_
* このページは日本語で利用できます(`外部リンク <http://kondou.com/BS4/>`_)
* `이 문서는 한국어 번역도 가능합니다. <https://www.crummy.com/software/BeautifulSoup/bs4/doc.ko/>`_
* `Este documento também está disponível em Português do Brasil. <https://www.crummy.com/software/BeautifulSoup/bs4/doc.ptbr>`_
* `Эта документация доступна на русском языке. <https://www.crummy.com/software/BeautifulSoup/bs4/doc.ru/>`_

Αναζητώντας βοήθεια
------------

Άν έχετε ερωτήσεις για την Beautiful Soup, ή συναντήσετε προβλήματα, 
`στείλτε email στήν ομάδα συζητήσεων
<https://groups.google.com/forum/?fromgroups#!forum/beautifulsoup>`_. Αν
το πρόβλημα σας περιλαμβάνει ανάλυση (parsing) εγγράφου HTML, σιγουρευτείτε να αναφέρετε
:ref:`τι λέει η μέθοδος diagnose() <diagnose>` γι' αυτό
το έγγραφο.

Γρήγορη Εκκίνηση
===========

Παρακάτω είναι ένα HTML έγγραφο το οποίο θα χρησιμοποιηθεί σαν παράδειγμα σε καθ' όλη την διάρκεια
των οδηγιών. Είναι μέρος μιας ιστορίας απο την `Αλίκη στη χώρα των θαυμάτων`::

 html_doc = """<html><head><title>The Dormouse's story</title></head>
 <body>
 <p class="title"><b>The Dormouse's story</b></p>

 <p class="story">Once upon a time there were three little sisters; and their names were
 <a href="http://example.com/elsie" class="sister" id="link1">Elsie</a>,
 <a href="http://example.com/lacie" class="sister" id="link2">Lacie</a> and
 <a href="http://example.com/tillie" class="sister" id="link3">Tillie</a>;
 and they lived at the bottom of a well.</p>

 <p class="story">...</p>
 """

Διατρέχοντας το έγγραφο "three sisters" με την Beautiful Soup μας δίνεται ένα
αντικέιμενο ``BeautifulSoup``, το οποίο αναπαριστά το έγγραφο ώς μια εμφωλευμένη
δομή δεδομένων::

 from bs4 import BeautifulSoup
 soup = BeautifulSoup(html_doc, 'html.parser')

 print(soup.prettify())
 # <html>
 #  <head>
 #   <title>
 #    The Dormouse's story
 #   </title>
 #  </head>
 #  <body>
 #   <p class="title">
 #    <b>
 #     The Dormouse's story
 #    </b>
 #   </p>
 #   <p class="story">
 #    Once upon a time there were three little sisters; and their names were
 #    <a class="sister" href="http://example.com/elsie" id="link1">
 #     Elsie
 #    </a>
 #    ,
 #    <a class="sister" href="http://example.com/lacie" id="link2">
 #     Lacie
 #    </a>
 #    and
 #    <a class="sister" href="http://example.com/tillie" id="link3">
 #     Tillie
 #    </a>
 #    ; and they lived at the bottom of a well.
 #   </p>
 #   <p class="story">
 #    ...
 #   </p>
 #  </body>
 # </html>

Παρακάτω είναι μερικοί τρόποι πλοήγησης αυτής της δομής δεδομένων::

 soup.title
 # <title>The Dormouse's story</title>

 soup.title.name
 # u'title'

 soup.title.string
 # u'The Dormouse's story'

 soup.title.parent.name
 # u'head'

 soup.p
 # <p class="title"><b>The Dormouse's story</b></p>

 soup.p['class']
 # u'title'

 soup.a
 # <a class="sister" href="http://example.com/elsie" id="link1">Elsie</a>

 soup.find_all('a')
 # [<a class="sister" href="http://example.com/elsie" id="link1">Elsie</a>,
 #  <a class="sister" href="http://example.com/lacie" id="link2">Lacie</a>,
 #  <a class="sister" href="http://example.com/tillie" id="link3">Tillie</a>]

 soup.find(id="link3")
 # <a class="sister" href="http://example.com/tillie" id="link3">Tillie</a>

Μια συχνή εργασία είναι η εξαγωγή όλων των URLs που βρίσκονται μέσα στις ετικέτες <a> μιας σελίδας::

 for link in soup.find_all('a'):
     print(link.get('href'))
 # http://example.com/elsie
 # http://example.com/lacie
 # http://example.com/tillie

Μια άλλη συχνή εργασία είναι η εξαγωγή όλου του κειμένου απο μια σελίδα::

 print(soup.get_text())
 # The Dormouse's story
 #
 # The Dormouse's story
 #
 # Once upon a time there were three little sisters; and their names were
 # Elsie,
 # Lacie and
 # Tillie;
 # and they lived at the bottom of a well.
 #
 # ...

Σας μοιάζει αυτό με κάτι που θέλετε να κάνετε? Αν ναι, συνεχίστε την ανάγνωση.

Εγκατάσταση της Beautiful Soup
=========================

Αν χρησιμοποιείτε μια πρόσφατη έκδοση των Debian ή Ubuntu Linux, μπορείτε να εγκαταστήσετε την Beautiful Soup
με το σύστημα διαχείρησης πακέτων τους: 

:kbd:`$ apt-get install python3-bs4`

Η Beautiful Soup 4 εκδίδεται μέσω του PyPi, οπότε αν δεν μπορείτε να την εγκαταστήσετε με το σύστημα διαχείρησης πακέτων, μπορείτε να την εγκαταστήσετε
με το ``easy_install`` η το ``pip``. Το όνομα του πακέτου είναι ``beautifulsoup4``. Βεβαιωθείτε οτι χρησιμοποιείτε την σωστή έκδοση
του ``pip`` ή ``easy_install`` για την έκδοση Python σας
(τα παραπάνω μπορεί να ονομάζονται ``pip3`` και ``easy_install3`` αντίστοιχα).

:kbd:`$ easy_install beautifulsoup4`

:kbd:`$ pip install beautifulsoup4`

(Το πακέτο ``BeautifulSoup`` `δεν` είναι αυτό που θέλετε. Αυτό είναι η προηγούμενη μείζων έκδοσγ, `Beautiful Soup 3`_. 
Αρκετό λογισμικό χρησιμοποιεί την BS3, και γι' αυτό είναι διαθέσιμη, αλλά αν γράφετε νέο κώδικα θα πρέπει να εγκαταστήσετε την ``beautifulsoup4``.)

Αν δεν έχετε το ``easy_install`` ή ``pip`` εγκατεστημένα, μπορείτε να
`κατεβάσετε το Beautiful Soup 4 source tarball
<http://www.crummy.com/software/BeautifulSoup/download/4.x/>`_ και
να το εγκαταστήσετε με το ``setup.py``.

:kbd:`$ python setup.py install`

Αν όλα τα άλλα αποτύχουν, η άδεια της Beautiful Soup σας επιτρέπει να
πακετάρετε ολόκληρη την βιβλιοθήκη μαζί με την εφαρμογή σας. Μπορείτε να κατεβάσετε το
tarball, να αντιγράψετε τον κατάλογο ``bs4`` στο σύνολο αρχείων κώδικά σας,
και να την χρησιμοποιήσετε χωρίς να την εγκαταστήσετε.

Χρησιμοποιώ την Python 3.8 για την ανάπτυξη της Beautiful Soup, αλλά θα πρέπει να λειτουργεί και με
διαφορετικές, πρόσφατε εκδόσεις.

.. _parser-installation:


Εγκατάσταση ενός αναλυτή
-------------------

Η Beautiful Soup υποστηρίζει τον αναλυτή (parser) HTML ο οποίος εμπεριέχεται στην βασική βιβλιοθήκη της Python,
αλλά επιπλέον υποστηρίζεται και ένας αριθμός απο εξωτερικούς αναλυτές (parsers) Python.
Ένας απο αυτούς είναι ο `lxml αναλυτής <http://lxml.de/>`_. Αναλόγως την εγκατάσταση σας,
μπορείτε να εγκαταστήσετε τον lxml με μια απο αυτές τις εντολές:

:kbd:`$ apt-get install python-lxml`

:kbd:`$ easy_install lxml`

:kbd:`$ pip install lxml`

Μια διαφορετική εναλλακτική είναι ο `html5lib αναλυτής
<http://code.google.com/p/html5lib/>`_, ο οποίος αναλύει HTML με τον ίδιο τρόπο
που το κάνει και ένα πρόγραμμα περιήγησης. Αναλόγως την εγκατάσταση σας,
μπορείτε να εγκαταστήσετε τον html5lib με μια απο αυτές τις εντολές:

:kbd:`$ apt-get install python-html5lib`

:kbd:`$ easy_install html5lib`

:kbd:`$ pip install html5lib`

Ο παρακάτω πίνακας συνοψίζει τα πλεονεκτήματα και τα μειονεκτήματα του κάθε αναλυτή:

+----------------------+--------------------------------------------+--------------------------------+--------------------------+
| Parser               | Typical usage                              | Πλεονεκτήματα                  | Μειονεκτήματα            |
+----------------------+--------------------------------------------+--------------------------------+--------------------------+
| html.parser          | ``BeautifulSoup(markup, "html.parser")``   | * Συμπεριλαμβάνονται μπαταρίες | * Όχι τόσο γρήγορος όσο  |
| της Python           |                                            | * Αξιοπρεπή ταχύτητα           |   ο lxml, λιγότερο       |
|                      |                                            | * Επιεικής (Απο την Python 3.2)|επιεικής απο τον html5lib.|
+----------------------+--------------------------------------------+--------------------------------+--------------------------+
| lxml's HTML αναλυτής | ``BeautifulSoup(markup, "lxml")``          | * Πολύ γρήγορος                | * Εξωτερική εξάρτηση     |
|                      |                                            | * Επιεικής                     |   της C                  |
+----------------------+--------------------------------------------+--------------------------------+--------------------------+
| lxml's XML αναλυτής  | ``BeautifulSoup(markup, "lxml-xml")``      | * Πολύ γρήγορος                | * Εξωτερική εξάρτηση     |
|                      | ``BeautifulSoup(markup, "xml")``           | * Πρός το παρόν, ο μόνος       |   της C                  |
|                      |                                            |   υποστηριζόμενος αναλυτής XML |                          |
+----------------------+--------------------------------------------+--------------------------------+--------------------------+
| html5lib             | ``BeautifulSoup(markup, "html5lib")``      | * Εξαιρετικά επιεικής          | * Πολύ αργός             |
|                      |                                            | * Αναλύει σελίδες με ίδιο τρόπο| * Εξωτερική εξάρτηση     |
|                      |                                            |   με ένα πρόγραμμα περιήγησης  |   της Python             |
|                      |                                            | * Δημιουργεί έγκυρο HTML5      |                          |
+----------------------+--------------------------------------------+--------------------------------+--------------------------+

Εφόσων είναι εφικτό, συνίσταται η εγκατάσταση και η χρήση του lxml λόγο ταχύτητας. Αν
χρησιμοποιείται πολύ παλία έκδοση της Python -- νωρίτερα απο την 3.2.2 -- είναι
`απαραίτητη` η εγκατάσταση του lxml ή html5lib. Ό εσωτερικός HTML αναλυτής της Python
δεν είναι αρκετά καλός σε αυτές τις παλιές εκδόσεις.

Σημειώστε πως άν ένα έγγραφο είναι λανθασμένο, διαφορετικοί αναλυτές θα δημιουργήσουν
διαφορετικά δέντρα Beautiful Soup γι' αυτό. Δείτε `Διαφορές
μεταξύ αναλυτών`_ για λεπτομέριες.

Δημιουργώντας τη "Σούπα"
===============

Για να αναλύσετε ένα έγγραφο, δώστε το στον κατασκευαστή ``BeautifulSoup``.
Μπορείτε να του δώσετε μια συμβολοσειρά (String) ή ένα ανοιχτό filehandle::

 from bs4 import BeautifulSoup

 with open("index.html") as fp:
     soup = BeautifulSoup(fp, 'html.parser')

 soup = BeautifulSoup("<html>a web page</html>", 'html.parser')

Αρχικά, το έγγραφο μετατρέπεται σε Unicode, και οι HTML οντότητες μετατρέπονται
σε Unicode χαρακτήρες::

 print(BeautifulSoup("<html><head></head><body>Sacr&eacute; bleu!</body></html>", "html.parser"))
 # <html><head></head><body>Sacré bleu!</body></html>

Έπειτα, η Beautiful Soup αναλύει το έγγραφι χρησιμοποιώντας τον καλύτερο διαθέσιμο
αναλυτή. Θα χρησιμοποιήσει HTML αναλυτή, εκτός αν του πείτε συγκεκριμένα να χρησιμοποιήσει XML αναλυτή. 
(Δείτε `Ανάλυση XML`_.)

Τύποι αντικειμένων
================

Η Beautiful Soup μετατρέπει ένα περίπλοκο έγγραφο HTML σε ένα περίπλοκο HTML δέντρο
απο αντικείμενα της Python. Εσείς θα χρειαστεί να ασχοληθείτε μόνο με 4
`είδη` αντικειμένων: ``Tag``, ``NavigableString``, ``BeautifulSoup``,
και ``Comment``.

.. _Tag:

``Tag``
-------

Ένα αντικείμενο ``Tag`` αντιστοιχεί σε ένα XML ή HTML tag στο αρχικό έγγραφο::

 soup = BeautifulSoup('<b class="boldest">Extremely bold</b>', 'html.parser')
 tag = soup.b
 type(tag)
 # <class 'bs4.element.Tag'>

Τα Tags έχουν πολλές ιδιότητες και μεθόδους, τα πιο πολλά απο αυτά θα καλυφθούν
στο `Πλοήγηση στο δέντρο`_ and `Αναζητώντας στο δέντρο`_. Για τώρα, τα πιο
σημαντικά χαρακτηριστικά ενός tag είναι το όνομα του και οι ιδιότητες του.

Όνομα
^^^^

Κάθε tag έχει ένα όνομα, προσβάσιμο σαν ``.name``::

 tag.name
 # 'b'

Αν αλλάξετε το όνομα κάποιου tag, η αλλαγή θα φανεί σε οποιαδήποτε HTML
σήμανση δημιούργησε η Beautiful Soup::

 tag.name = "blockquote"
 tag
 # <blockquote class="boldest">Extremely bold</blockquote>

Ιδιότητες
^^^^^^^^^^

Ένα tag μπορεί να έχει έναν οποιοδήποτε αριθμό ιδιοτήτων. Το tag ``<b
id="boldest">`` έχει μια ιδιότητα "id" η τιμή της οποίας είναι
"boldest". Μπορείτε να έχετε πρόσβαση στις ιδιότητες ενός tag αντιμετωπίζοντας το tag σαν
ένα λεξικό (Python dictionary)::

 tag = BeautifulSoup('<b id="boldest">bold</b>', 'html.parser').b
 tag['id']
 # 'boldest'

Έχετε πρόσβαση σε αυτό το λεξικό απευθείας σαν ``.attrs``::

 tag.attrs
 # {'id': 'boldest'}

Μπορείτε να προσθέσετε, να αφαιρέσετε, και να τροποποιήσετε ιδιότητες ενός tag. Και πάλι, αυτό
γίνεται αντιμετωπίζοντας το tag ως λεξικό::

 tag['id'] = 'verybold'
 tag['another-attribute'] = 1
 tag
 # <b another-attribute="1" id="verybold"></b>

 del tag['id']
 del tag['another-attribute']
 tag
 # <b>bold</b>

 tag['id']
 # KeyError: 'id'
 tag.get('id')
 # None

.. _multivalue:

Ιδιότητες με πολλές τιμές
&&&&&&&&&&&&&&&&&&&&&&&

Η HTML 4 ορίζει μερικές ιδιότητες οι οποίες μπορεί να έχουν πολλαπλές τιμές. Η HTML 5
αφαιρεί δυο απο αυτές, αλλά ορίζει μερικές ακόμα. Η πιο κοινή ιδιότητα
με πολλαπλές τιμές είναι η ``class`` (αυτό γιατί ένα tag μπορεί να έχει παραπάνω απο
μια CSS κλάση). Μερικές ακόμα είναι οι ``rel``, ``rev``, ``accept-charset``,
``headers``, και ``accesskey``. Η Beautiful Soup παρουσιάζει την τιμή(ες)
μιας ιδιότητας με πολλές τιμές ως λίστα::

 css_soup = BeautifulSoup('<p class="body"></p>', 'html.parser')
 css_soup.p['class']
 # ['body']
  
 css_soup = BeautifulSoup('<p class="body strikeout"></p>', 'html.parser')
 css_soup.p['class']
 # ['body', 'strikeout']

Άν μια ιδιότητα `φαίνεται` να έχει πολλαπλές τιμές, αλλά δεν είναι ιδιότητα
πολλαπλών τιμών όπως ορίζεται απο οποιαδήποτε άλλη έκδοση του HTML
προτύπου, η Beautiful Soup θα αφήσει αυτή την ιδιότητα ώς έχει::

 id_soup = BeautifulSoup('<p id="my id"></p>', 'html.parser')
 id_soup.p['id']
 # 'my id'

Όταν μετατρέπετε ένα tag πίσω σε μια συμβολοσειρά (String), πολλαπλές ιδιότητες είναι
ενοποιημένα::

 rel_soup = BeautifulSoup('<p>Back to the <a rel="index">homepage</a></p>', 'html.parser')
 rel_soup.a['rel']
 # ['index']
 rel_soup.a['rel'] = ['index', 'contents']
 print(rel_soup.p)
 # <p>Back to the <a rel="index contents">homepage</a></p>

Μπορείτε να απενεργοποιήσετε αυτή την λειτουργία περνώντας το ``multi_valued_attributes=None`` σαν
όρισμα στον κατασκευαστή (constructor) της ``BeautifulSoup``::

 no_list_soup = BeautifulSoup('<p class="body strikeout"></p>', 'html.parser', multi_valued_attributes=None)
 no_list_soup.p['class']
 # 'body strikeout'

Μπορείτε να χρησιμοποιήσετε την ``get_attribute_list`` για να πάρετε μια τιμή η οποία θα είναι πάντα
λίστα, ανεξαρτήτως του αν η ιδιότητα έιναι ιδιότητα πολλαπλών τιμών::

 id_soup.p.get_attribute_list('id')
 # ["my id"]
 
Αν αναλύσετε ένα έγγραφο ως XML, δεν υπάρχουν ιδιότητες πολλαπλών τιμών::

 xml_soup = BeautifulSoup('<p class="body strikeout"></p>', 'xml')
 xml_soup.p['class']
 # 'body strikeout'

Ξανά, μπορείτε να το ρυθμίσετε χρησιμοποιώντας το όρισμα ``multi_valued_attributes``::

 class_is_multi= { '*' : 'class'}
 xml_soup = BeautifulSoup('<p class="body strikeout"></p>', 'xml', multi_valued_attributes=class_is_multi)
 xml_soup.p['class']
 # ['body', 'strikeout']

Πιθανώς δε θα χρειαστεί να το άνετε, αλλά αν χρειαστεί, χρησιμοποιήστε τις προεπιλογές (defaults) σαν
οδηγό. Εφαρμόζουν τους κανόνες οι οποίοι περιγράφονται στις προδιαγραφές της HTML::

 from bs4.builder import builder_registry
 builder_registry.lookup('html').DEFAULT_CDATA_LIST_ATTRIBUTES

  
``NavigableString``
-------------------

Μια συμβολοσειρά (String) αντιστοιχεί σε ένα κομμάτι κειμένου μέσα σε ένα tag. Η Beautiful Soup
χρησιμοποιεί την κλάση ``NavigableString`` για να συμπεριλάβει αυτά τα κομμάτια κειμένου::

 soup = BeautifulSoup('<b class="boldest">Extremely bold</b>', 'html.parser')
 tag = soup.b
 tag.string
 # 'Extremely bold'
 type(tag.string)
 # <class 'bs4.element.NavigableString'>

Ένα ``NavigableString`` είναι ακριβώς σαμ μια Unicode συμβολοσειρά της Python, με την διαφορά
οτι υποστηρίζει επιπλέον μερικά απο τα χαρακτηριστικά που περιγράφονται στο `Πλοήγηση στο
δέντρο`_ και `Αναζτήτηση στο δέντρο`_. Μπορείτε να μετατρέψετε ένα
``NavigableString`` σε μία Unicode συμβολοσειρά μέσω της ``str``::

 unicode_string = str(tag.string)
 unicode_string
 # 'Extremely bold'
 type(unicode_string)
 # <type 'str'>

Δεν μπορείτε να τροποποποιήσετε μια ακολουθία χαρακήρων επί τόπου, αλλά μπορείτε να αντικαταστήσετε μια ακολουθία χαρακήρων
με μια άλλη, χρησιμοποιώντας την :ref:`replace_with()`::

 tag.string.replace_with("No longer bold")
 tag
 # <b class="boldest">No longer bold</b>

Η ``NavigableString`` υποστηρίζει τα περισσότερα χαρακτηριστικά τα οποία περιγράφονται στο
`Πλοήγη στο δέντρο`_ και `Αναζήτηση στο δέντρο`_, αλλά όχι όλα.
Συγκεκριμένα, επειδή μια ακολουθία χαρακήρων δεν μπορεί να περιέχει τίποτα (όπως για παράδειγμα 
ένα tag μπορεί να περιέχει μια ακολουθία χαρακήρων ή ένα διαφορετικό tag), Οι ακολουθίες χαρακήρων δεν υποστηρίζουν τις
``.contents`` ή ``.string`` ιδιότητες, ή την μέθοδο ``find()``.

Αν θέλετε να χρησιμοποιήσετε ένα αντικέιμενο ``NavigableString`` εκτός της Beautiful Soup,
μπορείτε να καλέσετε την ``unicode()`` σε αυτό για να το μετατρέψετε σε μια νορμάλ Unicode συμβολοσειρά της Python.
Αν δε το κάνετε, η συμβολοσειρά σας θα "κουβαλάει" μια αναφορά σε
ολόκληρο το δέντρο ανάλυσης της Beautiful Soup, ακόμα και όταν σταματήσετε να χρησιμοποιείτε την
Beautiful Soup. Το οποίο είναι μεγάλη σπατάλη μνήμης.

``BeautifulSoup``
-----------------

Το αντικείμενο ``BeautifulSoup`` αντιπροσωπεύει το έγγραφο που αναλύθηκε (parsed) ολοκληρωτικά.
Για τις περισσότερες χρήσεις, μπορείτε να το χρησιμοποιήσετε ως ένα αντικέιμενο :ref:`Tag`.
Αυτό σημαίνει πως υποστηρίζει τις περισσότερες μεθόδους που περιγράφονται στα
`Πλοήγηση στο δέντρο`_ και `Αναζήτηση στο δέντρο`_.

Επιπλέον, μπορείτε να περάσετε ένα αντικείμενο ``BeautifulSoup`` σε μία μέθοδο
που ορίζετε στο `Τροποποιώντας το δέντρο`_, ακριβώς όπως θα περνάγατε ένα αντικείμενο :ref:`Tag`. Αυτό
σας επιτρέπει να κάνετε πράγματα όπως να συνδιάσετε δυο έγγραφα που έχουν αναλυθεί::

 doc = BeautifulSoup("<document><content/>INSERT FOOTER HERE</document", "xml")
 footer = BeautifulSoup("<footer>Here's the footer</footer>", "xml")
 doc.find(text="INSERT FOOTER HERE").replace_with(footer)
 # 'INSERT FOOTER HERE'
 print(doc)
 # <?xml version="1.0" encoding="utf-8"?>
 # <document><content/><footer>Here's the footer</footer></document>

Εφόσων το αντικέιμενο ``BeautifulSoup`` δεν αντιστοιχεί σε κάποιο πραγματικό
HTML ή XML tag, δεν έχει όνομα και ιδιότητες. Μερικές φορές είναι χρήσιμο
να δείτε το ``.name`` του, οπότε του έχει δωθεί το ειδικό
``.name`` "[document]"::

 soup.name
 # '[document]'

Σχόλια και άλλες ειδικές συμβολοσειρές
----------------------------------

Τα ``Tag``, ``NavigableString``, και ``BeautifulSoup`` καλύπτουν σχεδόν
τα πάντα που θα βρείτε σε ένα αρχείο HTML ή XML, αλλά υπάρχουν και μερικά
κομμάτια που έχουν μείνει. Το κυριότερο που πιθανώς να συναντήσετε
είναι το σχόλιο::

 markup = "<b><!--Hey, buddy. Want to buy a used parser?--></b>"
 soup = BeautifulSoup(markup, 'html.parser')
 comment = soup.b.string
 type(comment)
 # <class 'bs4.element.Comment'>

Το αντικέιμενο ``Comment`` είναι απλά ένας ειδικός τύπος της κλάσης ``NavigableString``::

 comment
 # 'Hey, buddy. Want to buy a used parser'

Αλλά όταν εμφανίζεται ως μέρος ενός HTML εγγράφου, ένα ``Comment`` εμφανίζεται
με ειδική μορφοποίηση::

 print(soup.b.prettify())
 # <b>
 #  <!--Hey, buddy. Want to buy a used parser?-->
 # </b>

Η Beautiful Soup επιπλέον ορίζει κλάσεις που ονομάζονται ``Stylesheet``, ``Script``,
και ``TemplateString``, για ενσωματωμένα CSS φύλλα (stylesheets) (οποιεσδήποτε
συμβολοσειρές βρίσκονται μέσα σε ένα ``<style>`` tag), ενσωματομένη Javascript (οποιεσδήποτε
συμβολοσειρές βρίσκονται μέσα σε ένα ``<script>`` tag), και HTML πρότυπα (οποιεσδήποτε
συμβολοσειρές βρίσκονται μέσα σε ένα ``<template>`` tag). Αυτές οι κλάσεις δουλεύουν ακριβώς όπως η
``NavigableString``; ο μόνος σκοπός τους είναι να διευκολύνουν την εύρεση του κύριου σώματος
της σελίδας, αγνοώντας όποιες συμβολοσειρές αντιπροσωπεύουν κάτι άλλο. `(Αυτές οι κλάσεις είναι νέες στην
Beautiful Soup 4.9.0, και ο html5lib αναλυτής δε τις χρησιμοποιεί.)`
 
Η Beautiful Soup ορίζει κλάσεις για οτιδήποτε άλλο μπορεί να εμφανιστεί σε
ένα XML έγγραφο: ``CData``, ``ProcessingInstruction``,
``Declaration``, και ``Doctype``. Όπως η ``Comment``, αυτές οι κλάσεις
είναι υπο-κλάσεις της ``NavigableString`` οι οποίες προσθέτουν κάτι έξτρα στην ακολουθία
χαρακτήρων. Παρακάτω είναι ένα παράδειγμα στο οποίο ένα σχόλιο αντικαθίσταται με ένα μπλόκ CDATA::

 from bs4 import CData
 cdata = CData("A CDATA block")
 comment.replace_with(cdata)

 print(soup.b.prettify())
 # <b>
 #  <![CDATA[A CDATA block]]>
 # </b>
 

Πλοήγηση στο δέντρο
===================

Παρακάτω βρίσκεται το HTML έγγραφο "Three sisters" ξανά::

 html_doc = """
 <html><head><title>The Dormouse's story</title></head>
 <body>
 <p class="title"><b>The Dormouse's story</b></p>

 <p class="story">Once upon a time there were three little sisters; and their names were
 <a href="http://example.com/elsie" class="sister" id="link1">Elsie</a>,
 <a href="http://example.com/lacie" class="sister" id="link2">Lacie</a> and
 <a href="http://example.com/tillie" class="sister" id="link3">Tillie</a>;
 and they lived at the bottom of a well.</p>

 <p class="story">...</p>
 """

 from bs4 import BeautifulSoup
 soup = BeautifulSoup(html_doc, 'html.parser')

Θα χρησιμοποιηθεί σαν παράδειγμα για το πως μπορείτε να μετακινηθείτε απο 
ένα μέρος του εγγράφου σε άλλο.

Προχωρώντας προς τα κάτω
----------

Τα Tags μπορεί να περιέχουν συμβολοσειρές και άλλα tags. Αυτά τα στοιχεία είναι τα
`παιδιά` του tag. Η Beautiful Soup παρέχει πολλές διαφορετικές ιδιότητες για την πλοήγηση
και την επανάληψη (iterating) πάνω στα παιδιά ενός tag.

Σημειλώστε οτι οι ακολουθίες χαραλτήρων της Beautiful Soup δεν υποστηρίζουν καμία απο αυτές τις
ιδιότητες, γιατί δεν μπορούν να έχουν παιδιά.

Πλοήγηση χρησιμοποιώντας ονόματα των tag
^^^^^^^^^^^^^^^^^^^^^^^^^^

Ο απλούστερος τρόπος να πλοηγηθείτε στο δέντρο ανάλυσης είναι να πείτε το όνομα του
tag που θέλετε. Αν θέλετε το tag <head>, απλά πείτε ``soup.head``::

 soup.head
 # <head><title>The Dormouse's story</title></head>

 soup.title
 # <title>The Dormouse's story</title>

Μπορείτε να χρησιμοποιήσετε αυτόν τον τρόπο ξανά και ξανά για να εμβαθύνετε σε ένα συγκεκριμένο μέρος
του δέντρου ανάλυσης. Αυτός ο κώδικας φέρνει το πρώτο <b> tag κάτω απο το <body> tag::

 soup.body.b
 # <b>The Dormouse's story</b>

Χρησιμοποιώντας το όνομα ενός tag σαν ιδιότητα θα σας δώσει μόνο το `πρώτο` tag με αυτό το
όνομα::

 soup.a
 # <a class="sister" href="http://example.com/elsie" id="link1">Elsie</a>

Αν θέλετε `όλα` τα <a> tags, ή οτιδήποτε πιο περίπλοκο
απο το πρώτο tag με συγκεκριμένο όνομα, θα χρειαστεί να χρησιμοποιήσετε μια απο τις
μεθόδους που περιγράφονται στο `Αναζήτηση στο δέντρο`_, όπως η `find_all()`::

 soup.find_all('a')
 # [<a class="sister" href="http://example.com/elsie" id="link1">Elsie</a>,
 #  <a class="sister" href="http://example.com/lacie" id="link2">Lacie</a>,
 #  <a class="sister" href="http://example.com/tillie" id="link3">Tillie</a>]

``.contents`` και ``.children``
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

Τα παιδιά ενός tag είναι διαθέσιμα σε μια λίστα με όνομα ``.contents``::

 head_tag = soup.head
 head_tag
 # <head><title>The Dormouse's story</title></head>

 head_tag.contents
 # [<title>The Dormouse's story</title>]

 title_tag = head_tag.contents[0]
 title_tag
 # <title>The Dormouse's story</title>
 title_tag.contents
 # ['The Dormouse's story']

Το ίδιο το αντικείμενο ``BeautifulSoup`` έχει παιδιά. Σε αυτή την περίπτωση, το
<html> tag είναι παιδί του αντικειμένου ``BeautifulSoup``::

 len(soup.contents)
 # 1
 soup.contents[0].name
 # 'html'

Μια συμβολοσειρά δεν έχει την ιδιότητα ``.contents``, γιατί δεν μπορεί να περιέχει
τίποτα::

 text = title_tag.contents[0]
 text.contents
 # AttributeError: 'NavigableString' object has no attribute 'contents'

Αντί να έχετε πρόσβαση σε αυτά ως λίστα, μπορείτε να έχετε πρόσβαση στα παιδιά 
ενός tag χρησιμοποιώντας την γεννήτρια (generator) ``.children``::

 for child in title_tag.children:
     print(child)
 # The Dormouse's story

Αν θέλετε να τροποποιήσετε τα παιδιά ενός tag, χρησιμοποιήστε τις μεθόδους  που περιγράφονται
στο `Τροποποίηση του δέντρου`_. Μην τροποποιείτε την λίστα ``.contents``
απευθείας: μπορεί να οδηγήσει σε προβλήματα τα οποία είναι ανεπαίσθητα και δύσκολο να βρεθούν.

 
``.descendants``
^^^^^^^^^^^^^^^^

Οι ιδιότητες ``.contents`` και ``.children`` λαμβάνουν υπόψη μόνο τα
`άμμεσα` παιδιά του των tag. Για παράδειγμα, το <head> tag έχει ένα μοναδικό άμμεσο
παιδί--το <title> tag::

 head_tag.contents
 # [<title>The Dormouse's story</title>]

'Ομως το <title> tag το ίδιο έχει ένα παιδί: την συμβολοσειρά "The Dormouse's
story". Υπάρχει μια αίσθηση ότι αυτή η συμβολοσειρά είναι επίσης παιδί του
<head> tag. Το χαρακτηριστικό ``.descendants`` σάς επιτρέπει να προσπελαύνετε `όλα`.
τα παιδιά ενός tag, αναδρομικά: τα άμεσα παιδιά του, τα παιδιά των
άμεσων παιδιών του και ούτω καθεξής::

 for child in head_tag.descendants:
     print(child)
 # <title>The Dormouse's story</title>
 # The Dormouse's story

Το <head> tag έχει μόνο ένα παιδί, αλλά έχει δύο απογόνους: το
<title> tag και το παιδί του <title> tag. Το ``Beautiful Soup`` αντικείμενο
έχει μόνο ένα άμεσο παιδί (το <html> tag), αλλά έχει πάρα πολλούς
απογόνους::

 len(list(soup.children))
 # 1
 len(list(soup.descendants))
 # 26

.. _.string:

``.string``
^^^^^^^^^^^

Εάν ένα tag έχει μόνο ένα παιδί και αυτό το παιδί είναι ένα ``NavigableString``,
το παιδί διατίθεται ως ``.string``::

 title_tag.string
 # 'The Dormouse's story'


Εάν το μοναδικό παιδί ενός tag είναι ένα άλλο tag, και `εκείνο` το tag έχει ένα
``.string``, τότε το tag-γονιός θεωρείται ότι έχει το ίδιο
``.string`` ως παιδί του::

 head_tag.contents
 # [<title>The Dormouse's story</title>]

 head_tag.string
 # 'The Dormouse's story'

Εάν ένα tag περιέχει περισσότερα από ένα πράγματα, τότε δεν είναι σαφές σε τι
θα πρέπει να αναφέρεται το ``.string``, επομένως το ``.string`` ορίζεται ως
``None``::

 print(soup.html.string)
 # None

.. _string-generators:

``.strings`` και ``stripped_strings``
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

Εάν υπάρχουν περισσότερα από ένα πράγματα μέσα σε ένα tag, μπορείτε να προσπελάσετε
μόνο τις συμβολοσειρές. Χρησιμοποιήστε τη γεννήτρια ``.strings``::

 for string in soup.strings:
     print(repr(string))
     '\n'
 # "The Dormouse's story"
 # '\n'
 # '\n'
 # "The Dormouse's story"
 # '\n'
 # 'Once upon a time there were three little sisters; and their names were\n'
 # 'Elsie'
 # ',\n'
 # 'Lacie'
 # ' and\n'
 # 'Tillie'
 # ';\nand they lived at the bottom of a well.'
 # '\n'
 # '...'
 # '\n'

Αυτές οι συμβολοσειρές τείνουν να έχουν πολύ επιπλέον κενό διάστημα, το οποίο μπορείτε
αφαιρέσετε χρησιμοποιώντας τη γεννήτρια ``.stripped_strings`` αντί::

 for string in soup.stripped_strings:
     print(repr(string))
 # "The Dormouse's story"
 # "The Dormouse's story"
 # 'Once upon a time there were three little sisters; and their names were'
 # 'Elsie'
 # ','
 # 'Lacie'
 # 'and'
 # 'Tillie'
 # ';\n and they lived at the bottom of a well.'
 # '...'

Εδώ, οι συμβολοσειρές που αποτελούνται εξ ολοκλήρου από κενό διάστημα αγνοούνται και
το κενό διάστημα στην αρχή και το τέλος των συμβολοσειρών αφαιρείται.

Ανεβαίνοντας
--------

Συνεχίζοντας με την αναλογία "οικογενειακό δέντρο", κάθε tag και κάθε συμβολοσειρά έχει ένα
`γονέα`: το tag που την περιέχει.

.. _.parent:

``.parent``
^^^^^^^^^^^

Μπορείτε να προσπελάσετε τον γονιό ενός στοιχείου με την ιδιότητα ``.parent``. Στο
παράδειγμα "three sisters", το <head> tag είναι ο γονέας
του <title> tag::

 title_tag = soup.title
 title_tag
 # <title>The Dormouse's story</title>
 title_tag.parent
 # <head><title>The Dormouse's story</title></head>

Η ίδια η συμβολοσειρά τίτλου έχει έναν γονέα: τo <title> tag που περιέχει
το::

 title_tag.string.parent
 # <title>The Dormouse's story</title>

Ο γονέας ενός ανώτατου επιπέδου (top-level) tag όπως το <html> είναι το ίδιο το 
αντικείμενο `BeautifulSoup`::

 html_tag = soup.html
 type(html_tag.parent)
 # <class 'bs4.BeautifulSoup'>

Και ο ``.parent`` ενός ``BeautifulSoup`` αντικειμένου ορίζεται ώς None::

 print(soup.parent)
 # None

.. _.parents:

``.parents``
^^^^^^^^^^^^

Μπορείτε να προσπελάσετε όλους τους γονείς ενός στοιχείου με το
``.parents``. Αυτό το παράδειγμα χρησιμοποιεί το ``.parents`` για να μετακινηθεί από ένα <a>
tag θαμμένο βαθιά μέσα στο έγγραφο, μέχρι την αρχή του εγγράφου::

 link = soup.a
 link
 # <a class="sister" href="http://example.com/elsie" id="link1">Elsie</a>
 for parent in link.parents:
     print(parent.name)
 # p
 # body
 # html
 # [document]

Μετακίνηση στο 'πλάι'
--------------

Θεωρήστε ένα απλό έγγραφο όπως αυτό::

 sibling_soup = BeautifulSoup("<a><b>text1</b><c>text2</c></a>", 'html.parser')
 print(sibling_soup.prettify())
 #   <a>
 #    <b>
 #     text1
 #    </b>
 #    <c>
 #     text2
 #    </c>
 #   </a>

Η ετικέτα <b> και η ετικέτα <c> βρίσκονται στο ίδιο επίπεδο: είναι και οι δύο άμεσα
παιδιά της ίδιας ετικέτας. Τα ονομάζουμε αδέρφια (`siblings`). Όταν ένα έγγραφο είναι
όμορφα τυπωμένο, τα αδέρφια εμφανίζονται στο ίδιο επίπεδο εσοχής. Μπορείτε
επίσης να χρησιμοποιήσετε αυτή τη σχέση στον κώδικα που γράφετε.

``.next_sibling`` και ``.previous_sibling``
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

Μπορείτε να χρησιμοποιήσετε τις ``.next_sibling`` και ``.previous_sibling`` για πλοήγηση
μεταξύ στοιχείων της σελίδας που βρίσκονται στο ίδιο επίπεδο του δέντρου ανάλυσης::

 sibling_soup.b.next_sibling
 # <c>text2</c>

 sibling_soup.c.previous_sibling
 # <b>text1</b>

Η ετικέτα <b> έχει την ``.next_sibling``, αλλά όχι την ``.previous_sibling``,
γιατί δεν υπάρχει τίποτα πριν από την ετικέτα <b> `στο ίδιο επίπεδο του
δέντρο`. Για τον ίδιο λόγο, η ετικέτα <c> έχει την ``.previous_sibling``
αλλά όχι την ``.next_sibling``::

 print(sibling_soup.b.previous_sibling)
 # None
 print(sibling_soup.c.next_sibling)
 # None

Οι συμβολοσειρές "text1" και "text2" `δεν` είναι αδέρφια, επειδή δεν
έχουν τον ίδιο γονέα::

 sibling_soup.b.string
 # 'text1'

 print(sibling_soup.b.string.next_sibling)
 # None

Σε πραγματικά έγγραφα, η ``.next_sibling`` ή ``.previous_sibling`` μιας
ετικέτας θα είναι συνήθως μια συμβολοσειρά που περιέχει κενό διάστημα. Επιστρέφοντας στο
έγγραφο "three sisters"::

 # <a href="http://example.com/elsie" class="sister" id="link1">Elsie</a>
 # <a href="http://example.com/lacie" class="sister" id="link2">Lacie</a>
 # <a href="http://example.com/tillie" class="sister" id="link3">Tillie</a>

Ίσως πιστεύετε ότι η ``.next_sibling`` της πρώτης ετικέτας <a>
να είναι η δεύτερη ετικέτα <a>. Αλλά στην πραγματικότητα, είναι μια συμβολοσειρά: το κόμμα και
η νέα γραμμή που διαχωρίζουν την πρώτη ετικέτα <a> από τη δεύτερη::

 link = soup.a
 link
 # <a class="sister" href="http://example.com/elsie" id="link1">Elsie</a>

 link.next_sibling
 # ',\n '

Η δεύτερη ετικέτα <a> είναι στην πραγματικότητα η ``.next_sibling`` του κόμματος::

 link.next_sibling.next_sibling
 # <a class="sister" href="http://example.com/lacie" id="link2">Lacie</a>

.. _sibling-generators:

``.next_siblings`` και ``.previous_siblings``
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

Μπορείτε να προσπελάσετε τα αδέρφια μιας ετικέτας με την ``.next_siblings`` ή
``.previous_siblings``:

 for sibling in soup.a.next_siblings:
     print(repr(sibling))
 # ',\n'
 # <a class="sister" href="http://example.com/lacie" id="link2">Lacie</a>
 # ' and\n'
 # <a class="sister" href="http://example.com/tillie" id="link3">Tillie</a>
 # '; and they lived at the bottom of a well.'

 for sibling in soup.find(id="link3").previous_siblings:
     print(repr(sibling))
 # ' and\n'
 # <a class="sister" href="http://example.com/lacie" id="link2">Lacie</a>
 # ',\n'
 # <a class="sister" href="http://example.com/elsie" id="link1">Elsie</a>
 # 'Once upon a time there were three little sisters; and their names were\n'

Πηγαίνοντας μπρός-πίσω
--------------------

Ρίξτε μια ματιά στην αρχή του εγγράφου "three sisters"::

 # <html><head><title>The Dormouse's story</title></head>
 # <p class="title"><b>The Dormouse's story</b></p>

Ένας αναλυτής HTML παίρνει αυτή τη σειρά χαρακτήρων και τη μετατρέπει σε α
σειρά συμβάντων: "άνοιξε μια ετικέτα <html>", "άνοιξε μια ετικέτα <head>", "άνοιξε μια ετικέτα
ετικέτα <title>", "add a string", "close the <title> tag", "open a <p>
ετικέτα", και ούτω καθεξής. Η Beautiful Soup προσφέρει εργαλεία για την ανακατασκευή της
αρχικής ανάλυσης του εγγράφου.

.. _element-generators:

``.next_element`` και ``.previous_element``
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

Η ιδιότητα ``.next_element`` μιας συμβολοσειράς ή ετικέτας οδηγεί σε οτιδήποτε
αναλύθηκε αμέσως μετά. Μπορεί να είναι το ίδιο με την
``.next_sibling``, αλλά συνήθως είναι δραματικά διαφορετικό.

Εδώ είναι η τελευταία ετικέτα <a> στο έγγραφο "three sisters". Η
``.next_sibling`` της είναι μια συμβολοσειρά: το συμπέρασμα της πρότασης που
διακόπηκε από την έναρξη της ετικέτας <a>.::

 last_a_tag = soup.find("a", id="link3")
 last_a_tag
 # <a class="sister" href="http://example.com/tillie" id="link3">Tillie</a>

 last_a_tag.next_sibling
 # ';\nand they lived at the bottom of a well.'

Αλλά η ``.next_element`` αυτής της ετικέτας <a>, το πράγμα που αναλύθηκε
αμέσως μετά την ετικέτα <a>, `δεν`` είναι το υπόλοιπο αυτής της πρότασης:
είναι η λέξη "Tillie"::

 last_a_tag.next_element
 # 'Tillie'

Αυτό συμβαίνει επειδή στην αρχική σήμανση εμφανίστηκε η λέξη "Tillie".
πριν από αυτό το ερωτηματικό. Ο αναλυτής συνάντησε μια ετικέτα <a>, μετά τη
λέξη "Tillie", μετά την ετικέτα κλεισίματος </a>, μετά το ερωτηματικό και την υπόλοιπη
πρόταση. Το ερωτηματικό είναι στο ίδιο επίπεδο με την ετικέτα <a>, αλλά
η λέξη "Tillie" συναντήθηκε πρώτη.

Η ιδιότητα ``.previous_element`` είναι ακριβώς το αντίθετο της
``.next_element``. Δείχνει οποιοδήποτε στοιχείο αναλύθηκε
αμέσως πριν από αυτό::

 last_a_tag.previous_element
 # ' and\n'
 last_a_tag.previous_element.next_element
 # <a class="sister" href="http://example.com/tillie" id="link3">Tillie</a>

``.next_elements`` και ``.previous_elements``
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

Θα πρέπει να έχετε κατανοήσει την ιδέα μέχρι τώρα. Μπορείτε να χρησιμοποιήσετε αυτούς τους επαναλήπτες (iterators) για να μετακινηθείτε
προς τα εμπρός ή προς τα πίσω στο έγγραφο όπως αναλύθηκε::

 for element in last_a_tag.next_elements:
     print(repr(element))
 # 'Tillie'
 # ';\nand they lived at the bottom of a well.'
 # '\n'
 # <p class="story">...</p>
 # '...'
 # '\n'

Αναζήτηση στο δέντρο
==================

Η Beautiful Soup ορίζει πολλές μεθόδους για την αναζήτηση στο δέντρο ανάλυσης,
αλλά είναι όλα πολύ παρόμοια. Θα αφιερώσουμε πολύ χρόνο για να εξηγήσουμε
τις δύο πιο δημοφιλείς μεθόδους: ``find()`` και ``find_all()``. Οι άλλοι
μέθοδοι λαμβάνουν σχεδόν ακριβώς τις ίδιες παραμέτρους, οπότε θα τις καλύψουμε
εν συντομία.

Για άλλη μια φορά, θα χρησιμοποιήσουμε το έγγραφο "three sisters" ως παράδειγμα::

 html_doc = """
 <html><head><title>The Dormouse's story</title></head>
 <body>
 <p class="title"><b>The Dormouse's story</b></p>

 <p class="story">Once upon a time there were three little sisters; and their names were
 <a href="http://example.com/elsie" class="sister" id="link1">Elsie</a>,
 <a href="http://example.com/lacie" class="sister" id="link2">Lacie</a> and
 <a href="http://example.com/tillie" class="sister" id="link3">Tillie</a>;
 and they lived at the bottom of a well.</p>

 <p class="story">...</p>
 """

 from bs4 import BeautifulSoup
 soup = BeautifulSoup(html_doc, 'html.parser')

Περνώντας ένα φίλτρο σε ένα όρισμα όπως το ``find_all()``, μπορείτε
"μεγεθύνετε" στα μέρη του εγγράφου που σας ενδιαφέρουν.

Είδη φίλτρων
----------------

Πριν μιλήσουμε λεπτομερώς για την ``find_all()`` και παρόμοιες μεθόδους, Θα
σας δείξουμε παραδείγματα διαφορετικών φίλτρων που μπορείτε να περάσετε σε αυτές τις
μεθόδους. Αυτά τα φίλτρα εμφανίζονται ξανά και ξανά, στο API
αναζήτησης. Μπορείτε να τα χρησιμοποιήσετε για να φιλτράρετε με βάση το όνομα μιας ετικέτας,
στα χαρακτηριστικά του, στο κείμενο μιας συμβολοσειράς ή σε κάποιο συνδυασμό αυτών.

.. _a string:

Μια συμβολοσειρά
^^^^^^^^

Το απλούστερο φίλτρο είναι μια συμβολοσειρά. Περάστε μια συμβολοσειρά σε μια μέθοδο αναζήτησης και
η Beautiful Soup θα εκτελέσει μια αντιστοίχιση σε ακριβώς αυτή τη συμβολοσειρά. Αυτός
κώδικας βρίσκει όλες τις ετικέτες <b> στο έγγραφο::

 soup.find_all('b')
 # [<b>The Dormouse's story</b>]

Εάν περάσετε μια συμβολοσειρά byte, η Beautiful Soup θα υποθέσει ότι η συμβολοσειρά είναι
κωδικοποιημένη ως UTF-8. Μπορείτε να το αποφύγετε αυτό περνώντας μια συμβολοσειρά Unicode.

.. _a regular expression:

Μια κανονική έκφραση (regular expression)
^^^^^^^^^^^^^^^^^^^^

Εάν περάσετε ένα αντικείμενο κανονικής έκφρασης, η Beautiful Soup θα φιλτράρει
έναντι αυτής της κανονικής έκφρασης χρησιμοποιώντας τη μέθοδο ``search()``. Αυτός ο κώδικας
βρίσκει όλες τις ετικέτες των οποίων τα ονόματα αρχίζουν με το γράμμα "b"; σε αυτή την
περίπτωση, την ετικέτα <body> και την ετικέτα <b>::

 import re
 for tag in soup.find_all(re.compile("^b")):
     print(tag.name)
 # body
 # b

Αυτός ο κώδικας βρίσκει όλες τις ετικέτες των οποίων τα ονόματα περιέχουν το γράμμα 't'::

 for tag in soup.find_all(re.compile("t")):
     print(tag.name)
 # html
 # title

.. _a list:

Μια λίστα
^^^^^^

Εάν περάσετε μια λίστα, η Beautiful Soup θα επιτρέψει μια αντιστοιχία συμβολοσειρών
έναντι `οποιουδήποτε` στοιχείου σε αυτήν τη λίστα. Αυτός ο κώδικας βρίσκει όλες τις ετικέτες <a>
`και` όλες οι ετικέτες <b>::

 soup.find_all(["a", "b"])
 # [<b>The Dormouse's story</b>,
 #  <a class="sister" href="http://example.com/elsie" id="link1">Elsie</a>,
 #  <a class="sister" href="http://example.com/lacie" id="link2">Lacie</a>,
 #  <a class="sister" href="http://example.com/tillie" id="link3">Tillie</a>]

.. _the value True:

``True``
^^^^^^^^

Η τιμή ``True`` ταιριάζει με ότι μπορεί. Αυτός ο κώδικας βρίσκει `όλες`.
τις ετικέτες στο έγγραφο, αλλά καμία από τις συμβολοσειρές κειμένου::

 for tag in soup.find_all(True):
     print(tag.name)
 # html
 # head
 # title
 # body
 # p
 # b
 # p
 # a
 # a
 # a
 # p

.. a function:

Μια συνάρτηση
^^^^^^^^^^

Εάν κανένα από τα παραπάνω δεν ταιριάζει για εσάς, ορίστε μια συνάρτηση που
παίρνει ένα στοιχείο ως μοναδικό όρισμα. Η συνάρτηση πρέπει να επιστρέψει
``True`` εάν το όρισμα ταιριάζει, και ``False`` διαφορετικά.

Εδώ είναι μια συνάρτηση που επιστρέφει ``True`` εάν μια ετικέτα ορίζει την ιδιότητα
"κλάση" αλλά δεν ορίζει την "id"::

 def has_class_but_no_id(tag):
     return tag.has_attr('class') and not tag.has_attr('id')

Περάστε αυτή τη συνάρτηση στην ``find_all()`` και θα βρείτε όλες τις <p>
ετικέτες::

 soup.find_all(has_class_but_no_id)
 # [<p class="title"><b>The Dormouse's story</b></p>,
 #  <p class="story">Once upon a time there were…bottom of a well.</p>,
 #  <p class="story">...</p>]

Αυτή η λειτουργία λαμβάνει μόνο τις ετικέτες <p>. Δεν βρίσκει τις <a>
ετικέτες, επειδή αυτές οι ετικέτες ορίζουν και την "class" και την "id". Δεν διαλέγει
ετικέτες όπως <html> και <title>, επειδή αυτές οι ετικέτες δεν ορίζουν την
"class".

Εάν περάσετε σε μια συνάρτηση για φιλτράρισμα μια συγκεκριμένη ιδιότητα όπως την
``href``, το όρισμα που μεταβιβάζεται στη συνάρτηση θα είναι η τιμή της ιδιότητας,
όχι ολόκληρη η ετικέτα. Εδώ είναι μια συνάρτηση που βρίσκει όλες τις ετικέτες ``a``
του οποίου το χαρακτηριστικό ``href`` *δεν* ταιριάζει με μια κανονική έκφραση::

 import re
 def not_lacie(href):
     return href and not re.compile("lacie").search(href)
 
 soup.find_all(href=not_lacie)
 # [<a class="sister" href="http://example.com/elsie" id="link1">Elsie</a>,
 #  <a class="sister" href="http://example.com/tillie" id="link3">Tillie</a>]

Η συνάρτηση μπορεί να είναι τόσο περίπλοκη όσο χρειάζεστε. Εδώ είναι μια
συνάρτηση που επιστρέφει ``True`` εάν μια ετικέτα περιβάλλεται από αντικείμενα
συμβολοσειράς::

 from bs4 import NavigableString
 def surrounded_by_strings(tag):
     return (isinstance(tag.next_element, NavigableString)
             and isinstance(tag.previous_element, NavigableString))

 for tag in soup.find_all(surrounded_by_strings):
     print(tag.name)
 # body
 # p
 # a
 # a
 # a
 # p

Τώρα είμαστε έτοιμοι να κοιτάξουμε τις μεθόδους αναζήτησης λεπτομερώς.

``find_all()``
--------------

Ορισμός μεθόδου: find_all(:ref:`name <name>`, :ref:`attrs <attrs>`, :ref:`recursive
<recursive>`, :ref:`string <string>`, :ref:`limit <limit>`, :ref:`**kwargs <kwargs>`)

Η μέθοδος ``find_all()`` προσπελαύνει τους απογόνους μιας ετικέτας και
ανακτά `όλους` τους απογόνους που ταιριάζουν με τα φίλτρα σας. Δώσαμε αρκετά
παραδείγματα στο `Είδη φίλτρων`_, αλλά εδώ είναι μερικά ακόμη::

 soup.find_all("title")
 # [<title>The Dormouse's story</title>]

 soup.find_all("p", "title")
 # [<p class="title"><b>The Dormouse's story</b></p>]

 soup.find_all("a")
 # [<a class="sister" href="http://example.com/elsie" id="link1">Elsie</a>,
 #  <a class="sister" href="http://example.com/lacie" id="link2">Lacie</a>,
 #  <a class="sister" href="http://example.com/tillie" id="link3">Tillie</a>]

 soup.find_all(id="link2")
 # [<a class="sister" href="http://example.com/lacie" id="link2">Lacie</a>]

 import re
 soup.find(string=re.compile("sisters"))
 # 'Once upon a time there were three little sisters; and their names were\n'

Μερικά από αυτά θα πρέπει να φαίνονται οικεία, αλλά άλλα είναι νέα. Τι σημαίνει
να περάσετε μια ``string`` ή ``id`` τιμή; Γιατί η
``find_all("p", "title")`` βρίσκει μια ετικέτα <p> με την CSS κλάση "title";
Ας δούμε τα ορίσματα για την ``find_all()``.

.. _name:

Το όρισμα ``name``
^^^^^^^^^^^^^^^^^^^^^

Περάστε μια τιμή για το ``όνομα`` και θα ενημερώσετε την Beautiful Soup
οτι ενδιαφέρεστε για ετικέτες με συγκεκριμένα ονόματα. Οι συμβολοσειρές κειμένου θα αγνοηθούν, καθώς
και ετικέτες των οποίων τα ονόματα δεν ταιριάζουν.

Αυτή είναι η απλούστερη χρήση::

 soup.find_all("title")
 # [<title>The Dormouse's story</title>]

Θυμηθείτε απο το `Kinds of filters`_ οτι η τιμή της ``name`` μπορεί να είναι `μια
συμβολοσειρά`_, `μια κανονική έκφραση`_, `μια λίστα`_, `μια συνάρτηση`_, ή `η τιμή True`_.

.. _kwargs:

Τα ορίσματα λέξεων-κλειδιών
^^^^^^^^^^^^^^^^^^^^^

Any argument that's not recognized will be turned into a filter on one
of a tag's attributes. If you pass in a value for an argument called ``id``,
Beautiful Soup will filter against each tag's 'id' attribute::

 soup.find_all(id='link2')
 # [<a class="sister" href="http://example.com/lacie" id="link2">Lacie</a>]

If you pass in a value for ``href``, Beautiful Soup will filter
against each tag's 'href' attribute::

 soup.find_all(href=re.compile("elsie"))
 # [<a class="sister" href="http://example.com/elsie" id="link1">Elsie</a>]

You can filter an attribute based on `a string`_, `a regular
expression`_, `a list`_, `a function`_, or `the value True`_.

This code finds all tags whose ``id`` attribute has a value,
regardless of what the value is::

 soup.find_all(id=True)
 # [<a class="sister" href="http://example.com/elsie" id="link1">Elsie</a>,
 #  <a class="sister" href="http://example.com/lacie" id="link2">Lacie</a>,
 #  <a class="sister" href="http://example.com/tillie" id="link3">Tillie</a>]

You can filter multiple attributes at once by passing in more than one
keyword argument::

 soup.find_all(href=re.compile("elsie"), id='link1')
 # [<a class="sister" href="http://example.com/elsie" id="link1">Elsie</a>]

Some attributes, like the data-* attributes in HTML 5, have names that
can't be used as the names of keyword arguments::

 data_soup = BeautifulSoup('<div data-foo="value">foo!</div>', 'html.parser')
 data_soup.find_all(data-foo="value")
 # SyntaxError: keyword can't be an expression

You can use these attributes in searches by putting them into a
dictionary and passing the dictionary into ``find_all()`` as the
``attrs`` argument::

 data_soup.find_all(attrs={"data-foo": "value"})
 # [<div data-foo="value">foo!</div>]

You can't use a keyword argument to search for HTML's 'name' element,
because Beautiful Soup uses the ``name`` argument to contain the name
of the tag itself. Instead, you can give a value to 'name' in the
``attrs`` argument::

 name_soup = BeautifulSoup('<input name="email"/>', 'html.parser')
 name_soup.find_all(name="email")
 # []
 name_soup.find_all(attrs={"name": "email"})
 # [<input name="email"/>]

.. _attrs:

Searching by CSS class
^^^^^^^^^^^^^^^^^^^^^^

It's very useful to search for a tag that has a certain CSS class, but
the name of the CSS attribute, "class", is a reserved word in
Python. Using ``class`` as a keyword argument will give you a syntax
error. As of Beautiful Soup 4.1.2, you can search by CSS class using
the keyword argument ``class_``::

 soup.find_all("a", class_="sister")
 # [<a class="sister" href="http://example.com/elsie" id="link1">Elsie</a>,
 #  <a class="sister" href="http://example.com/lacie" id="link2">Lacie</a>,
 #  <a class="sister" href="http://example.com/tillie" id="link3">Tillie</a>]

As with any keyword argument, you can pass ``class_`` a string, a regular
expression, a function, or ``True``::

 soup.find_all(class_=re.compile("itl"))
 # [<p class="title"><b>The Dormouse's story</b></p>]

 def has_six_characters(css_class):
     return css_class is not None and len(css_class) == 6

 soup.find_all(class_=has_six_characters)
 # [<a class="sister" href="http://example.com/elsie" id="link1">Elsie</a>,
 #  <a class="sister" href="http://example.com/lacie" id="link2">Lacie</a>,
 #  <a class="sister" href="http://example.com/tillie" id="link3">Tillie</a>]

:ref:`Remember <multivalue>` that a single tag can have multiple
values for its "class" attribute. When you search for a tag that
matches a certain CSS class, you're matching against `any` of its CSS
classes::

 css_soup = BeautifulSoup('<p class="body strikeout"></p>', 'html.parser')
 css_soup.find_all("p", class_="strikeout")
 # [<p class="body strikeout"></p>]

 css_soup.find_all("p", class_="body")
 # [<p class="body strikeout"></p>]

You can also search for the exact string value of the ``class`` attribute::

 css_soup.find_all("p", class_="body strikeout")
 # [<p class="body strikeout"></p>]

But searching for variants of the string value won't work::

 css_soup.find_all("p", class_="strikeout body")
 # []

If you want to search for tags that match two or more CSS classes, you
should use a CSS selector::

 css_soup.select("p.strikeout.body")
 # [<p class="body strikeout"></p>]

In older versions of Beautiful Soup, which don't have the ``class_``
shortcut, you can use the ``attrs`` trick mentioned above. Create a
dictionary whose value for "class" is the string (or regular
expression, or whatever) you want to search for::

 soup.find_all("a", attrs={"class": "sister"})
 # [<a class="sister" href="http://example.com/elsie" id="link1">Elsie</a>,
 #  <a class="sister" href="http://example.com/lacie" id="link2">Lacie</a>,
 #  <a class="sister" href="http://example.com/tillie" id="link3">Tillie</a>]

.. _string:

The ``string`` argument
^^^^^^^^^^^^^^^^^^^^^^^

With ``string`` you can search for strings instead of tags. As with
``name`` and the keyword arguments, you can pass in `a string`_, `a
regular expression`_, `a list`_, `a function`_, or `the value True`_.
Here are some examples::

 soup.find_all(string="Elsie")
 # ['Elsie']

 soup.find_all(string=["Tillie", "Elsie", "Lacie"])
 # ['Elsie', 'Lacie', 'Tillie']

 soup.find_all(string=re.compile("Dormouse"))
 # ["The Dormouse's story", "The Dormouse's story"]

 def is_the_only_string_within_a_tag(s):
     """Return True if this string is the only child of its parent tag."""
     return (s == s.parent.string)

 soup.find_all(string=is_the_only_string_within_a_tag)
 # ["The Dormouse's story", "The Dormouse's story", 'Elsie', 'Lacie', 'Tillie', '...']

Although ``string`` is for finding strings, you can combine it with
arguments that find tags: Beautiful Soup will find all tags whose
``.string`` matches your value for ``string``. This code finds the <a>
tags whose ``.string`` is "Elsie"::

 soup.find_all("a", string="Elsie")
 # [<a href="http://example.com/elsie" class="sister" id="link1">Elsie</a>]

The ``string`` argument is new in Beautiful Soup 4.4.0. In earlier
versions it was called ``text``::

 soup.find_all("a", text="Elsie")
 # [<a href="http://example.com/elsie" class="sister" id="link1">Elsie</a>]

.. _limit:

The ``limit`` argument
^^^^^^^^^^^^^^^^^^^^^^

``find_all()`` returns all the tags and strings that match your
filters. This can take a while if the document is large. If you don't
need `all` the results, you can pass in a number for ``limit``. This
works just like the LIMIT keyword in SQL. It tells Beautiful Soup to
stop gathering results after it's found a certain number.

There are three links in the "three sisters" document, but this code
only finds the first two::

 soup.find_all("a", limit=2)
 # [<a class="sister" href="http://example.com/elsie" id="link1">Elsie</a>,
 #  <a class="sister" href="http://example.com/lacie" id="link2">Lacie</a>]

.. _recursive:

The ``recursive`` argument
^^^^^^^^^^^^^^^^^^^^^^^^^^

If you call ``mytag.find_all()``, Beautiful Soup will examine all the
descendants of ``mytag``: its children, its children's children, and
so on. If you only want Beautiful Soup to consider direct children,
you can pass in ``recursive=False``. See the difference here::

 soup.html.find_all("title")
 # [<title>The Dormouse's story</title>]

 soup.html.find_all("title", recursive=False)
 # []

Here's that part of the document::

 <html>
  <head>
   <title>
    The Dormouse's story
   </title>
  </head>
 ...

The <title> tag is beneath the <html> tag, but it's not `directly`
beneath the <html> tag: the <head> tag is in the way. Beautiful Soup
finds the <title> tag when it's allowed to look at all descendants of
the <html> tag, but when ``recursive=False`` restricts it to the
<html> tag's immediate children, it finds nothing.

Beautiful Soup offers a lot of tree-searching methods (covered below),
and they mostly take the same arguments as ``find_all()``: ``name``,
``attrs``, ``string``, ``limit``, and the keyword arguments. But the
``recursive`` argument is different: ``find_all()`` and ``find()`` are
the only methods that support it. Passing ``recursive=False`` into a
method like ``find_parents()`` wouldn't be very useful.

Calling a tag is like calling ``find_all()``
--------------------------------------------

Because ``find_all()`` is the most popular method in the Beautiful
Soup search API, you can use a shortcut for it. If you treat the
``BeautifulSoup`` object or a ``Tag`` object as though it were a
function, then it's the same as calling ``find_all()`` on that
object. These two lines of code are equivalent::

 soup.find_all("a")
 soup("a")

These two lines are also equivalent::

 soup.title.find_all(string=True)
 soup.title(string=True)

``find()``
----------

Method signature: find(:ref:`name <name>`, :ref:`attrs <attrs>`, :ref:`recursive
<recursive>`, :ref:`string <string>`, :ref:`**kwargs <kwargs>`)

The ``find_all()`` method scans the entire document looking for
results, but sometimes you only want to find one result. If you know a
document only has one <body> tag, it's a waste of time to scan the
entire document looking for more. Rather than passing in ``limit=1``
every time you call ``find_all``, you can use the ``find()``
method. These two lines of code are `nearly` equivalent::

 soup.find_all('title', limit=1)
 # [<title>The Dormouse's story</title>]

 soup.find('title')
 # <title>The Dormouse's story</title>

The only difference is that ``find_all()`` returns a list containing
the single result, and ``find()`` just returns the result.

If ``find_all()`` can't find anything, it returns an empty list. If
``find()`` can't find anything, it returns ``None``::

 print(soup.find("nosuchtag"))
 # None

Remember the ``soup.head.title`` trick from `Navigating using tag
names`_? That trick works by repeatedly calling ``find()``::

 soup.head.title
 # <title>The Dormouse's story</title>

 soup.find("head").find("title")
 # <title>The Dormouse's story</title>

``find_parents()`` and ``find_parent()``
----------------------------------------

Method signature: find_parents(:ref:`name <name>`, :ref:`attrs <attrs>`, :ref:`string <string>`, :ref:`limit <limit>`, :ref:`**kwargs <kwargs>`)

Method signature: find_parent(:ref:`name <name>`, :ref:`attrs <attrs>`, :ref:`string <string>`, :ref:`**kwargs <kwargs>`)

I spent a lot of time above covering ``find_all()`` and
``find()``. The Beautiful Soup API defines ten other methods for
searching the tree, but don't be afraid. Five of these methods are
basically the same as ``find_all()``, and the other five are basically
the same as ``find()``. The only differences are in what parts of the
tree they search.

First let's consider ``find_parents()`` and
``find_parent()``. Remember that ``find_all()`` and ``find()`` work
their way down the tree, looking at tag's descendants. These methods
do the opposite: they work their way `up` the tree, looking at a tag's
(or a string's) parents. Let's try them out, starting from a string
buried deep in the "three daughters" document::

 a_string = soup.find(string="Lacie")
 a_string
 # 'Lacie'

 a_string.find_parents("a")
 # [<a class="sister" href="http://example.com/lacie" id="link2">Lacie</a>]

 a_string.find_parent("p")
 # <p class="story">Once upon a time there were three little sisters; and their names were
 #  <a class="sister" href="http://example.com/elsie" id="link1">Elsie</a>,
 #  <a class="sister" href="http://example.com/lacie" id="link2">Lacie</a> and
 #  <a class="sister" href="http://example.com/tillie" id="link3">Tillie</a>;
 #  and they lived at the bottom of a well.</p>

 a_string.find_parents("p", class_="title")
 # []

One of the three <a> tags is the direct parent of the string in
question, so our search finds it. One of the three <p> tags is an
indirect parent of the string, and our search finds that as
well. There's a <p> tag with the CSS class "title" `somewhere` in the
document, but it's not one of this string's parents, so we can't find
it with ``find_parents()``.

You may have made the connection between ``find_parent()`` and
``find_parents()``, and the `.parent`_ and `.parents`_ attributes
mentioned earlier. The connection is very strong. These search methods
actually use ``.parents`` to iterate over all the parents, and check
each one against the provided filter to see if it matches.

``find_next_siblings()`` and ``find_next_sibling()``
----------------------------------------------------

Method signature: find_next_siblings(:ref:`name <name>`, :ref:`attrs <attrs>`, :ref:`string <string>`, :ref:`limit <limit>`, :ref:`**kwargs <kwargs>`)

Method signature: find_next_sibling(:ref:`name <name>`, :ref:`attrs <attrs>`, :ref:`string <string>`, :ref:`**kwargs <kwargs>`)

These methods use :ref:`.next_siblings <sibling-generators>` to
iterate over the rest of an element's siblings in the tree. The
``find_next_siblings()`` method returns all the siblings that match,
and ``find_next_sibling()`` only returns the first one::

 first_link = soup.a
 first_link
 # <a class="sister" href="http://example.com/elsie" id="link1">Elsie</a>

 first_link.find_next_siblings("a")
 # [<a class="sister" href="http://example.com/lacie" id="link2">Lacie</a>,
 #  <a class="sister" href="http://example.com/tillie" id="link3">Tillie</a>]

 first_story_paragraph = soup.find("p", "story")
 first_story_paragraph.find_next_sibling("p")
 # <p class="story">...</p>

``find_previous_siblings()`` and ``find_previous_sibling()``
------------------------------------------------------------

Method signature: find_previous_siblings(:ref:`name <name>`, :ref:`attrs <attrs>`, :ref:`string <string>`, :ref:`limit <limit>`, :ref:`**kwargs <kwargs>`)

Method signature: find_previous_sibling(:ref:`name <name>`, :ref:`attrs <attrs>`, :ref:`string <string>`, :ref:`**kwargs <kwargs>`)

These methods use :ref:`.previous_siblings <sibling-generators>` to iterate over an element's
siblings that precede it in the tree. The ``find_previous_siblings()``
method returns all the siblings that match, and
``find_previous_sibling()`` only returns the first one::

 last_link = soup.find("a", id="link3")
 last_link
 # <a class="sister" href="http://example.com/tillie" id="link3">Tillie</a>

 last_link.find_previous_siblings("a")
 # [<a class="sister" href="http://example.com/lacie" id="link2">Lacie</a>,
 #  <a class="sister" href="http://example.com/elsie" id="link1">Elsie</a>]

 first_story_paragraph = soup.find("p", "story")
 first_story_paragraph.find_previous_sibling("p")
 # <p class="title"><b>The Dormouse's story</b></p>


``find_all_next()`` and ``find_next()``
---------------------------------------

Method signature: find_all_next(:ref:`name <name>`, :ref:`attrs <attrs>`, :ref:`string <string>`, :ref:`limit <limit>`, :ref:`**kwargs <kwargs>`)

Method signature: find_next(:ref:`name <name>`, :ref:`attrs <attrs>`, :ref:`string <string>`, :ref:`**kwargs <kwargs>`)

These methods use :ref:`.next_elements <element-generators>` to
iterate over whatever tags and strings that come after it in the
document. The ``find_all_next()`` method returns all matches, and
``find_next()`` only returns the first match::

 first_link = soup.a
 first_link
 # <a class="sister" href="http://example.com/elsie" id="link1">Elsie</a>

 first_link.find_all_next(string=True)
 # ['Elsie', ',\n', 'Lacie', ' and\n', 'Tillie',
 #  ';\nand they lived at the bottom of a well.', '\n', '...', '\n']

 first_link.find_next("p")
 # <p class="story">...</p>

In the first example, the string "Elsie" showed up, even though it was
contained within the <a> tag we started from. In the second example,
the last <p> tag in the document showed up, even though it's not in
the same part of the tree as the <a> tag we started from. For these
methods, all that matters is that an element match the filter, and
show up later in the document than the starting element.

``find_all_previous()`` and ``find_previous()``
-----------------------------------------------

Method signature: find_all_previous(:ref:`name <name>`, :ref:`attrs <attrs>`, :ref:`string <string>`, :ref:`limit <limit>`, :ref:`**kwargs <kwargs>`)

Method signature: find_previous(:ref:`name <name>`, :ref:`attrs <attrs>`, :ref:`string <string>`, :ref:`**kwargs <kwargs>`)

These methods use :ref:`.previous_elements <element-generators>` to
iterate over the tags and strings that came before it in the
document. The ``find_all_previous()`` method returns all matches, and
``find_previous()`` only returns the first match::

 first_link = soup.a
 first_link
 # <a class="sister" href="http://example.com/elsie" id="link1">Elsie</a>

 first_link.find_all_previous("p")
 # [<p class="story">Once upon a time there were three little sisters; ...</p>,
 #  <p class="title"><b>The Dormouse's story</b></p>]

 first_link.find_previous("title")
 # <title>The Dormouse's story</title>

The call to ``find_all_previous("p")`` found the first paragraph in
the document (the one with class="title"), but it also finds the
second paragraph, the <p> tag that contains the <a> tag we started
with. This shouldn't be too surprising: we're looking at all the tags
that show up earlier in the document than the one we started with. A
<p> tag that contains an <a> tag must have shown up before the <a>
tag it contains.

CSS selectors
-------------

``BeautifulSoup`` has a ``.select()`` method which uses the `SoupSieve
<https://facelessuser.github.io/soupsieve/>`_ package to run a CSS
selector against a parsed document and return all the matching
elements. ``Tag`` has a similar method which runs a CSS selector
against the contents of a single tag.

(The SoupSieve integration was added in Beautiful Soup 4.7.0. Earlier
versions also have the ``.select()`` method, but only the most
commonly-used CSS selectors are supported. If you installed Beautiful
Soup through ``pip``, SoupSieve was installed at the same time, so you
don't have to do anything extra.)

The SoupSieve `documentation
<https://facelessuser.github.io/soupsieve/>`_ lists all the currently
supported CSS selectors, but here are some of the basics:

You can find tags::

 soup.select("title")
 # [<title>The Dormouse's story</title>]

 soup.select("p:nth-of-type(3)")
 # [<p class="story">...</p>]

Find tags beneath other tags::

 soup.select("body a")
 # [<a class="sister" href="http://example.com/elsie" id="link1">Elsie</a>,
 #  <a class="sister" href="http://example.com/lacie"  id="link2">Lacie</a>,
 #  <a class="sister" href="http://example.com/tillie" id="link3">Tillie</a>]

 soup.select("html head title")
 # [<title>The Dormouse's story</title>]

Find tags `directly` beneath other tags::

 soup.select("head > title")
 # [<title>The Dormouse's story</title>]

 soup.select("p > a")
 # [<a class="sister" href="http://example.com/elsie" id="link1">Elsie</a>,
 #  <a class="sister" href="http://example.com/lacie"  id="link2">Lacie</a>,
 #  <a class="sister" href="http://example.com/tillie" id="link3">Tillie</a>]

 soup.select("p > a:nth-of-type(2)")
 # [<a class="sister" href="http://example.com/lacie" id="link2">Lacie</a>]

 soup.select("p > #link1")
 # [<a class="sister" href="http://example.com/elsie" id="link1">Elsie</a>]

 soup.select("body > a")
 # []

Find the siblings of tags::

 soup.select("#link1 ~ .sister")
 # [<a class="sister" href="http://example.com/lacie" id="link2">Lacie</a>,
 #  <a class="sister" href="http://example.com/tillie"  id="link3">Tillie</a>]

 soup.select("#link1 + .sister")
 # [<a class="sister" href="http://example.com/lacie" id="link2">Lacie</a>]

Find tags by CSS class::

 soup.select(".sister")
 # [<a class="sister" href="http://example.com/elsie" id="link1">Elsie</a>,
 #  <a class="sister" href="http://example.com/lacie" id="link2">Lacie</a>,
 #  <a class="sister" href="http://example.com/tillie" id="link3">Tillie</a>]

 soup.select("[class~=sister]")
 # [<a class="sister" href="http://example.com/elsie" id="link1">Elsie</a>,
 #  <a class="sister" href="http://example.com/lacie" id="link2">Lacie</a>,
 #  <a class="sister" href="http://example.com/tillie" id="link3">Tillie</a>]

Find tags by ID::

 soup.select("#link1")
 # [<a class="sister" href="http://example.com/elsie" id="link1">Elsie</a>]

 soup.select("a#link2")
 # [<a class="sister" href="http://example.com/lacie" id="link2">Lacie</a>]

Find tags that match any selector from a list of selectors::

 soup.select("#link1,#link2")
 # [<a class="sister" href="http://example.com/elsie" id="link1">Elsie</a>,
 #  <a class="sister" href="http://example.com/lacie" id="link2">Lacie</a>]

Test for the existence of an attribute::

 soup.select('a[href]')
 # [<a class="sister" href="http://example.com/elsie" id="link1">Elsie</a>,
 #  <a class="sister" href="http://example.com/lacie" id="link2">Lacie</a>,
 #  <a class="sister" href="http://example.com/tillie" id="link3">Tillie</a>]

Find tags by attribute value::

 soup.select('a[href="http://example.com/elsie"]')
 # [<a class="sister" href="http://example.com/elsie" id="link1">Elsie</a>]

 soup.select('a[href^="http://example.com/"]')
 # [<a class="sister" href="http://example.com/elsie" id="link1">Elsie</a>,
 #  <a class="sister" href="http://example.com/lacie" id="link2">Lacie</a>,
 #  <a class="sister" href="http://example.com/tillie" id="link3">Tillie</a>]

 soup.select('a[href$="tillie"]')
 # [<a class="sister" href="http://example.com/tillie" id="link3">Tillie</a>]

 soup.select('a[href*=".com/el"]')
 # [<a class="sister" href="http://example.com/elsie" id="link1">Elsie</a>]

There's also a method called ``select_one()``, which finds only the
first tag that matches a selector::

 soup.select_one(".sister")
 # <a class="sister" href="http://example.com/elsie" id="link1">Elsie</a>

If you've parsed XML that defines namespaces, you can use them in CSS
selectors.::

 from bs4 import BeautifulSoup
 xml = """<tag xmlns:ns1="http://namespace1/" xmlns:ns2="http://namespace2/">
  <ns1:child>I'm in namespace 1</ns1:child>
  <ns2:child>I'm in namespace 2</ns2:child>
 </tag> """
 soup = BeautifulSoup(xml, "xml")

 soup.select("child")
 # [<ns1:child>I'm in namespace 1</ns1:child>, <ns2:child>I'm in namespace 2</ns2:child>]

 soup.select("ns1|child")
 # [<ns1:child>I'm in namespace 1</ns1:child>]
 
When handling a CSS selector that uses namespaces, Beautiful Soup
always tries to use namespace prefixes that make sense based on what
it saw while parsing the document. You can always provide your own
dictionary of abbreviations::

 namespaces = dict(first="http://namespace1/", second="http://namespace2/")
 soup.select("second|child", namespaces=namespaces)
 # [<ns1:child>I'm in namespace 2</ns1:child>]
 
All this CSS selector stuff is a convenience for people who already
know the CSS selector syntax. You can do all of this with the
Beautiful Soup API. And if CSS selectors are all you need, you should
parse the document with lxml: it's a lot faster. But this lets you
`combine` CSS selectors with the Beautiful Soup API.

Modifying the tree
==================

Beautiful Soup's main strength is in searching the parse tree, but you
can also modify the tree and write your changes as a new HTML or XML
document.

Changing tag names and attributes
---------------------------------

I covered this earlier, in `Attributes`_, but it bears repeating. You
can rename a tag, change the values of its attributes, add new
attributes, and delete attributes::

 soup = BeautifulSoup('<b class="boldest">Extremely bold</b>', 'html.parser')
 tag = soup.b

 tag.name = "blockquote"
 tag['class'] = 'verybold'
 tag['id'] = 1
 tag
 # <blockquote class="verybold" id="1">Extremely bold</blockquote>

 del tag['class']
 del tag['id']
 tag
 # <blockquote>Extremely bold</blockquote>

Modifying ``.string``
---------------------

If you set a tag's ``.string`` attribute to a new string, the tag's contents are
replaced with that string::

 markup = '<a href="http://example.com/">I linked to <i>example.com</i></a>'
 soup = BeautifulSoup(markup, 'html.parser')

 tag = soup.a
 tag.string = "New link text."
 tag
 # <a href="http://example.com/">New link text.</a>
  
Be careful: if the tag contained other tags, they and all their
contents will be destroyed.  

``append()``
------------

You can add to a tag's contents with ``Tag.append()``. It works just
like calling ``.append()`` on a Python list::

 soup = BeautifulSoup("<a>Foo</a>", 'html.parser')
 soup.a.append("Bar")

 soup
 # <a>FooBar</a>
 soup.a.contents
 # ['Foo', 'Bar']

``extend()``
------------

Starting in Beautiful Soup 4.7.0, ``Tag`` also supports a method
called ``.extend()``, which adds every element of a list to a ``Tag``,
in order::

 soup = BeautifulSoup("<a>Soup</a>", 'html.parser')
 soup.a.extend(["'s", " ", "on"])

 soup
 # <a>Soup's on</a>
 soup.a.contents
 # ['Soup', ''s', ' ', 'on']
   
``NavigableString()`` and ``.new_tag()``
-------------------------------------------------

If you need to add a string to a document, no problem--you can pass a
Python string in to ``append()``, or you can call the ``NavigableString``
constructor::

 soup = BeautifulSoup("<b></b>", 'html.parser')
 tag = soup.b
 tag.append("Hello")
 new_string = NavigableString(" there")
 tag.append(new_string)
 tag
 # <b>Hello there.</b>
 tag.contents
 # ['Hello', ' there']

If you want to create a comment or some other subclass of
``NavigableString``, just call the constructor::

 from bs4 import Comment
 new_comment = Comment("Nice to see you.")
 tag.append(new_comment)
 tag
 # <b>Hello there<!--Nice to see you.--></b>
 tag.contents
 # ['Hello', ' there', 'Nice to see you.']

`(This is a new feature in Beautiful Soup 4.4.0.)`

What if you need to create a whole new tag?  The best solution is to
call the factory method ``BeautifulSoup.new_tag()``::

 soup = BeautifulSoup("<b></b>", 'html.parser')
 original_tag = soup.b

 new_tag = soup.new_tag("a", href="http://www.example.com")
 original_tag.append(new_tag)
 original_tag
 # <b><a href="http://www.example.com"></a></b>

 new_tag.string = "Link text."
 original_tag
 # <b><a href="http://www.example.com">Link text.</a></b>

Only the first argument, the tag name, is required.

``insert()``
------------

``Tag.insert()`` is just like ``Tag.append()``, except the new element
doesn't necessarily go at the end of its parent's
``.contents``. It'll be inserted at whatever numeric position you
say. It works just like ``.insert()`` on a Python list::

 markup = '<a href="http://example.com/">I linked to <i>example.com</i></a>'
 soup = BeautifulSoup(markup, 'html.parser')
 tag = soup.a

 tag.insert(1, "but did not endorse ")
 tag
 # <a href="http://example.com/">I linked to but did not endorse <i>example.com</i></a>
 tag.contents
 # ['I linked to ', 'but did not endorse', <i>example.com</i>]

``insert_before()`` and ``insert_after()``
------------------------------------------

The ``insert_before()`` method inserts tags or strings immediately
before something else in the parse tree::

 soup = BeautifulSoup("<b>leave</b>", 'html.parser')
 tag = soup.new_tag("i")
 tag.string = "Don't"
 soup.b.string.insert_before(tag)
 soup.b
 # <b><i>Don't</i>leave</b>

The ``insert_after()`` method inserts tags or strings immediately
following something else in the parse tree::

 div = soup.new_tag('div')
 div.string = 'ever'
 soup.b.i.insert_after(" you ", div)
 soup.b
 # <b><i>Don't</i> you <div>ever</div> leave</b>
 soup.b.contents
 # [<i>Don't</i>, ' you', <div>ever</div>, 'leave']

``clear()``
-----------

``Tag.clear()`` removes the contents of a tag::

 markup = '<a href="http://example.com/">I linked to <i>example.com</i></a>'
 soup = BeautifulSoup(markup, 'html.parser')
 tag = soup.a

 tag.clear()
 tag
 # <a href="http://example.com/"></a>

``extract()``
-------------

``PageElement.extract()`` removes a tag or string from the tree. It
returns the tag or string that was extracted::

 markup = '<a href="http://example.com/">I linked to <i>example.com</i></a>'
 soup = BeautifulSoup(markup, 'html.parser')
 a_tag = soup.a

 i_tag = soup.i.extract()

 a_tag
 # <a href="http://example.com/">I linked to</a>

 i_tag
 # <i>example.com</i>

 print(i_tag.parent)
 # None

At this point you effectively have two parse trees: one rooted at the
``BeautifulSoup`` object you used to parse the document, and one rooted
at the tag that was extracted. You can go on to call ``extract`` on
a child of the element you extracted::

 my_string = i_tag.string.extract()
 my_string
 # 'example.com'

 print(my_string.parent)
 # None
 i_tag
 # <i></i>


``decompose()``
---------------

``Tag.decompose()`` removes a tag from the tree, then `completely
destroys it and its contents`::

 markup = '<a href="http://example.com/">I linked to <i>example.com</i></a>'
 soup = BeautifulSoup(markup, 'html.parser')
 a_tag = soup.a
 i_tag = soup.i

 i_tag.decompose()
 a_tag
 # <a href="http://example.com/">I linked to</a>

The behavior of a decomposed ``Tag`` or ``NavigableString`` is not
defined and you should not use it for anything. If you're not sure
whether something has been decomposed, you can check its
``.decomposed`` property `(new in Beautiful Soup 4.9.0)`::

 i_tag.decomposed
 # True

 a_tag.decomposed
 # False


.. _replace_with():

``replace_with()``
------------------

``PageElement.replace_with()`` removes a tag or string from the tree,
and replaces it with one or more tags or strings of your choice::

 markup = '<a href="http://example.com/">I linked to <i>example.com</i></a>'
 soup = BeautifulSoup(markup, 'html.parser')
 a_tag = soup.a

 new_tag = soup.new_tag("b")
 new_tag.string = "example.com"
 a_tag.i.replace_with(new_tag)

 a_tag
 # <a href="http://example.com/">I linked to <b>example.com</b></a>

 bold_tag = soup.new_tag("b")
 bold_tag.string = "example"
 i_tag = soup.new_tag("i")
 i_tag.string = "net"
 a_tag.b.replace_with(bold_tag, ".", i_tag)

 a_tag
 # <a href="http://example.com/">I linked to <b>example</b>.<i>net</i></a>

``replace_with()`` returns the tag or string that got replaced, so
that you can examine it or add it back to another part of the tree.

`The ability to pass multiple arguments into replace_with() is new
in Beautiful Soup 4.10.0.`


``wrap()``
----------

``PageElement.wrap()`` wraps an element in the tag you specify. It
returns the new wrapper::

 soup = BeautifulSoup("<p>I wish I was bold.</p>", 'html.parser')
 soup.p.string.wrap(soup.new_tag("b"))
 # <b>I wish I was bold.</b>

 soup.p.wrap(soup.new_tag("div"))
 # <div><p><b>I wish I was bold.</b></p></div>

`This method is new in Beautiful Soup 4.0.5.`

``unwrap()``
---------------------------

``Tag.unwrap()`` is the opposite of ``wrap()``. It replaces a tag with
whatever's inside that tag. It's good for stripping out markup::

 markup = '<a href="http://example.com/">I linked to <i>example.com</i></a>'
 soup = BeautifulSoup(markup, 'html.parser')
 a_tag = soup.a

 a_tag.i.unwrap()
 a_tag
 # <a href="http://example.com/">I linked to example.com</a>

Like ``replace_with()``, ``unwrap()`` returns the tag
that was replaced.

``smooth()``
---------------------------

After calling a bunch of methods that modify the parse tree, you may end up with two or more ``NavigableString`` objects next to each other. Beautiful Soup doesn't have any problems with this, but since it can't happen in a freshly parsed document, you might not expect behavior like the following::

 soup = BeautifulSoup("<p>A one</p>", 'html.parser')
 soup.p.append(", a two")

 soup.p.contents
 # ['A one', ', a two']

 print(soup.p.encode())
 # b'<p>A one, a two</p>'

 print(soup.p.prettify())
 # <p>
 #  A one
 #  , a two
 # </p>

You can call ``Tag.smooth()`` to clean up the parse tree by consolidating adjacent strings::

 soup.smooth()

 soup.p.contents
 # ['A one, a two']

 print(soup.p.prettify())
 # <p>
 #  A one, a two
 # </p>

`This method is new in Beautiful Soup 4.8.0.`

Output
======

.. _.prettyprinting:

Pretty-printing
---------------

The ``prettify()`` method will turn a Beautiful Soup parse tree into a
nicely formatted Unicode string, with a separate line for each
tag and each string::

 markup = '<html><head><body><a href="http://example.com/">I linked to <i>example.com</i></a>'
 soup = BeautifulSoup(markup, 'html.parser')
 soup.prettify()
 # '<html>\n <head>\n </head>\n <body>\n  <a href="http://example.com/">\n...'

 print(soup.prettify())
 # <html>
 #  <head>
 #  </head>
 #  <body>
 #   <a href="http://example.com/">
 #    I linked to
 #    <i>
 #     example.com
 #    </i>
 #   </a>
 #  </body>
 # </html>

You can call ``prettify()`` on the top-level ``BeautifulSoup`` object,
or on any of its ``Tag`` objects::

 print(soup.a.prettify())
 # <a href="http://example.com/">
 #  I linked to
 #  <i>
 #   example.com
 #  </i>
 # </a>

Since it adds whitespace (in the form of newlines), ``prettify()``
changes the meaning of an HTML document and should not be used to
reformat one. The goal of ``prettify()`` is to help you visually
understand the structure of the documents you work with.
  
Non-pretty printing
-------------------

If you just want a string, with no fancy formatting, you can call
``str()`` on a ``BeautifulSoup`` object, or on a ``Tag`` within it::

 str(soup)
 # '<html><head></head><body><a href="http://example.com/">I linked to <i>example.com</i></a></body></html>'

 str(soup.a)
 # '<a href="http://example.com/">I linked to <i>example.com</i></a>'

The ``str()`` function returns a string encoded in UTF-8. See
`Encodings`_ for other options.

You can also call ``encode()`` to get a bytestring, and ``decode()``
to get Unicode.

.. _output_formatters:

Output formatters
-----------------

If you give Beautiful Soup a document that contains HTML entities like
"&lquot;", they'll be converted to Unicode characters::

 soup = BeautifulSoup("&ldquo;Dammit!&rdquo; he said.", 'html.parser')
 str(soup)
 # '“Dammit!” he said.'

If you then convert the document to a bytestring, the Unicode characters
will be encoded as UTF-8. You won't get the HTML entities back::

 soup.encode("utf8")
 # b'\xe2\x80\x9cDammit!\xe2\x80\x9d he said.'

By default, the only characters that are escaped upon output are bare
ampersands and angle brackets. These get turned into "&amp;", "&lt;",
and "&gt;", so that Beautiful Soup doesn't inadvertently generate
invalid HTML or XML::

 soup = BeautifulSoup("<p>The law firm of Dewey, Cheatem, & Howe</p>", 'html.parser')
 soup.p
 # <p>The law firm of Dewey, Cheatem, &amp; Howe</p>

 soup = BeautifulSoup('<a href="http://example.com/?foo=val1&bar=val2">A link</a>', 'html.parser')
 soup.a
 # <a href="http://example.com/?foo=val1&amp;bar=val2">A link</a>

You can change this behavior by providing a value for the
``formatter`` argument to ``prettify()``, ``encode()``, or
``decode()``. Beautiful Soup recognizes five possible values for
``formatter``.

The default is ``formatter="minimal"``. Strings will only be processed
enough to ensure that Beautiful Soup generates valid HTML/XML::

 french = "<p>Il a dit &lt;&lt;Sacr&eacute; bleu!&gt;&gt;</p>"
 soup = BeautifulSoup(french, 'html.parser')
 print(soup.prettify(formatter="minimal"))
 # <p>
 #  Il a dit &lt;&lt;Sacré bleu!&gt;&gt;
 # </p>

If you pass in ``formatter="html"``, Beautiful Soup will convert
Unicode characters to HTML entities whenever possible::

 print(soup.prettify(formatter="html"))
 # <p>
 #  Il a dit &lt;&lt;Sacr&eacute; bleu!&gt;&gt;
 # </p>

If you pass in ``formatter="html5"``, it's similar to
``formatter="html"``, but Beautiful Soup will
omit the closing slash in HTML void tags like "br"::

 br = BeautifulSoup("<br>", 'html.parser').br
 
 print(br.encode(formatter="html"))
 # b'<br/>'
 
 print(br.encode(formatter="html5"))
 # b'<br>'

In addition, any attributes whose values are the empty string
will become HTML-style boolean attributes::

 option = BeautifulSoup('<option selected=""></option>').option
 print(option.encode(formatter="html"))
 # b'<option selected=""></option>'
 
 print(option.encode(formatter="html5"))
 # b'<option selected></option>'

*(This behavior is new as of Beautiful Soup 4.10.0.)*
 
If you pass in ``formatter=None``, Beautiful Soup will not modify
strings at all on output. This is the fastest option, but it may lead
to Beautiful Soup generating invalid HTML/XML, as in these examples::

 print(soup.prettify(formatter=None))
 # <p>
 #  Il a dit <<Sacré bleu!>>
 # </p>

 link_soup = BeautifulSoup('<a href="http://example.com/?foo=val1&bar=val2">A link</a>', 'html.parser')
 print(link_soup.a.encode(formatter=None))
 # b'<a href="http://example.com/?foo=val1&bar=val2">A link</a>'

If you need more sophisticated control over your output, you can
use Beautiful Soup's ``Formatter`` class. Here's a formatter that
converts strings to uppercase, whether they occur in a text node or in an
attribute value::

 from bs4.formatter import HTMLFormatter
 def uppercase(str):
     return str.upper()
 
 formatter = HTMLFormatter(uppercase)

 print(soup.prettify(formatter=formatter))
 # <p>
 #  IL A DIT <<SACRÉ BLEU!>>
 # </p>

 print(link_soup.a.prettify(formatter=formatter))
 # <a href="HTTP://EXAMPLE.COM/?FOO=VAL1&BAR=VAL2">
 #  A LINK
 # </a>

Here's a formatter that increases the indentation when pretty-printing::

 formatter = HTMLFormatter(indent=8)
 print(link_soup.a.prettify(formatter=formatter))
 # <a href="http://example.com/?foo=val1&bar=val2">
 #         A link
 # </a>
 
Subclassing ``HTMLFormatter`` or ``XMLFormatter`` will give you even
more control over the output. For example, Beautiful Soup sorts the
attributes in every tag by default::

 attr_soup = BeautifulSoup(b'<p z="1" m="2" a="3"></p>', 'html.parser')
 print(attr_soup.p.encode())
 # <p a="3" m="2" z="1"></p>

To turn this off, you can subclass the ``Formatter.attributes()``
method, which controls which attributes are output and in what
order. This implementation also filters out the attribute called "m"
whenever it appears::

 class UnsortedAttributes(HTMLFormatter):
     def attributes(self, tag):
         for k, v in tag.attrs.items():
             if k == 'm':
                 continue
             yield k, v
 
 print(attr_soup.p.encode(formatter=UnsortedAttributes())) 
 # <p z="1" a="3"></p>

One last caveat: if you create a ``CData`` object, the text inside
that object is always presented `exactly as it appears, with no
formatting`. Beautiful Soup will call your entity substitution
function, just in case you've written a custom function that counts
all the strings in the document or something, but it will ignore the
return value::

 from bs4.element import CData
 soup = BeautifulSoup("<a></a>", 'html.parser')
 soup.a.string = CData("one < three")
 print(soup.a.prettify(formatter="html"))
 # <a>
 #  <![CDATA[one < three]]>
 # </a>


``get_text()``
--------------

If you only want the human-readable text inside a document or tag, you can use the
``get_text()`` method. It returns all the text in a document or
beneath a tag, as a single Unicode string::

 markup = '<a href="http://example.com/">\nI linked to <i>example.com</i>\n</a>'
 soup = BeautifulSoup(markup, 'html.parser')

 soup.get_text()
 '\nI linked to example.com\n'
 soup.i.get_text()
 'example.com'

You can specify a string to be used to join the bits of text
together::

 # soup.get_text("|")
 '\nI linked to |example.com|\n'

You can tell Beautiful Soup to strip whitespace from the beginning and
end of each bit of text::

 # soup.get_text("|", strip=True)
 'I linked to|example.com'

But at that point you might want to use the :ref:`.stripped_strings <string-generators>`
generator instead, and process the text yourself::

 [text for text in soup.stripped_strings]
 # ['I linked to', 'example.com']

*As of Beautiful Soup version 4.9.0, when lxml or html.parser are in
use, the contents of <script>, <style>, and <template>
tags are generally not considered to be 'text', since those tags are not part of
the human-visible content of the page.*

*As of Beautiful Soup version 4.10.0, you can call get_text(),
.strings, or .stripped_strings on a NavigableString object. It will
either return the object itself, or nothing, so the only reason to do
this is when you're iterating over a mixed list.*

 
Specifying the parser to use
============================

If you just need to parse some HTML, you can dump the markup into the
``BeautifulSoup`` constructor, and it'll probably be fine. Beautiful
Soup will pick a parser for you and parse the data. But there are a
few additional arguments you can pass in to the constructor to change
which parser is used.

The first argument to the ``BeautifulSoup`` constructor is a string or
an open filehandle--the markup you want parsed. The second argument is
`how` you'd like the markup parsed.

If you don't specify anything, you'll get the best HTML parser that's
installed. Beautiful Soup ranks lxml's parser as being the best, then
html5lib's, then Python's built-in parser. You can override this by
specifying one of the following:

* What type of markup you want to parse. Currently supported are
  "html", "xml", and "html5".

* The name of the parser library you want to use. Currently supported
  options are "lxml", "html5lib", and "html.parser" (Python's
  built-in HTML parser).

The section `Installing a parser`_ contrasts the supported parsers.

If you don't have an appropriate parser installed, Beautiful Soup will
ignore your request and pick a different parser. Right now, the only
supported XML parser is lxml. If you don't have lxml installed, asking
for an XML parser won't give you one, and asking for "lxml" won't work
either.

Differences between parsers
---------------------------

Beautiful Soup presents the same interface to a number of different
parsers, but each parser is different. Different parsers will create
different parse trees from the same document. The biggest differences
are between the HTML parsers and the XML parsers. Here's a short
document, parsed as HTML using the parser that comes with Python::

 BeautifulSoup("<a><b/></a>", "html.parser")
 # <a><b></b></a>

Since a standalone <b/> tag is not valid HTML, html.parser turns it into
a <b></b> tag pair.

Here's the same document parsed as XML (running this requires that you
have lxml installed). Note that the standalone <b/> tag is left alone, and
that the document is given an XML declaration instead of being put
into an <html> tag.::

 print(BeautifulSoup("<a><b/></a>", "xml"))
 # <?xml version="1.0" encoding="utf-8"?>
 # <a><b/></a>

There are also differences between HTML parsers. If you give Beautiful
Soup a perfectly-formed HTML document, these differences won't
matter. One parser will be faster than another, but they'll all give
you a data structure that looks exactly like the original HTML
document.

But if the document is not perfectly-formed, different parsers will
give different results. Here's a short, invalid document parsed using
lxml's HTML parser. Note that the <a> tag gets wrapped in <body> and
<html> tags, and the dangling </p> tag is simply ignored::

 BeautifulSoup("<a></p>", "lxml")
 # <html><body><a></a></body></html>

Here's the same document parsed using html5lib::

 BeautifulSoup("<a></p>", "html5lib")
 # <html><head></head><body><a><p></p></a></body></html>

Instead of ignoring the dangling </p> tag, html5lib pairs it with an
opening <p> tag. html5lib also adds an empty <head> tag; lxml didn't
bother.

Here's the same document parsed with Python's built-in HTML
parser::

 BeautifulSoup("<a></p>", "html.parser")
 # <a></a>

Like lxml, this parser ignores the closing </p> tag. Unlike
html5lib or lxml, this parser makes no attempt to create a
well-formed HTML document by adding <html> or <body> tags.

Since the document "<a></p>" is invalid, none of these techniques is
the 'correct' way to handle it. The html5lib parser uses techniques
that are part of the HTML5 standard, so it has the best claim on being
the 'correct' way, but all three techniques are legitimate.

Differences between parsers can affect your script. If you're planning
on distributing your script to other people, or running it on multiple
machines, you should specify a parser in the ``BeautifulSoup``
constructor. That will reduce the chances that your users parse a
document differently from the way you parse it.

Encodings
=========

Any HTML or XML document is written in a specific encoding like ASCII
or UTF-8.  But when you load that document into Beautiful Soup, you'll
discover it's been converted to Unicode::

 markup = "<h1>Sacr\xc3\xa9 bleu!</h1>"
 soup = BeautifulSoup(markup, 'html.parser')
 soup.h1
 # <h1>Sacré bleu!</h1>
 soup.h1.string
 # 'Sacr\xe9 bleu!'

It's not magic. (That sure would be nice.) Beautiful Soup uses a
sub-library called `Unicode, Dammit`_ to detect a document's encoding
and convert it to Unicode. The autodetected encoding is available as
the ``.original_encoding`` attribute of the ``BeautifulSoup`` object::

 soup.original_encoding
 'utf-8'

Unicode, Dammit guesses correctly most of the time, but sometimes it
makes mistakes. Sometimes it guesses correctly, but only after a
byte-by-byte search of the document that takes a very long time. If
you happen to know a document's encoding ahead of time, you can avoid
mistakes and delays by passing it to the ``BeautifulSoup`` constructor
as ``from_encoding``.

Here's a document written in ISO-8859-8. The document is so short that
Unicode, Dammit can't get a lock on it, and misidentifies it as
ISO-8859-7::

 markup = b"<h1>\xed\xe5\xec\xf9</h1>"
 soup = BeautifulSoup(markup, 'html.parser')
 print(soup.h1)
 # <h1>νεμω</h1>
 print(soup.original_encoding)
 # iso-8859-7

We can fix this by passing in the correct ``from_encoding``::

 soup = BeautifulSoup(markup, 'html.parser', from_encoding="iso-8859-8")
 print(soup.h1)
 # <h1>םולש</h1>
 print(soup.original_encoding)
 # iso8859-8

If you don't know what the correct encoding is, but you know that
Unicode, Dammit is guessing wrong, you can pass the wrong guesses in
as ``exclude_encodings``::

 soup = BeautifulSoup(markup, 'html.parser', exclude_encodings=["iso-8859-7"])
 print(soup.h1)
 # <h1>םולש</h1>
 print(soup.original_encoding)
 # WINDOWS-1255

Windows-1255 isn't 100% correct, but that encoding is a compatible
superset of ISO-8859-8, so it's close enough. (``exclude_encodings``
is a new feature in Beautiful Soup 4.4.0.)

In rare cases (usually when a UTF-8 document contains text written in
a completely different encoding), the only way to get Unicode may be
to replace some characters with the special Unicode character
"REPLACEMENT CHARACTER" (U+FFFD, �). If Unicode, Dammit needs to do
this, it will set the ``.contains_replacement_characters`` attribute
to ``True`` on the ``UnicodeDammit`` or ``BeautifulSoup`` object. This
lets you know that the Unicode representation is not an exact
representation of the original--some data was lost. If a document
contains �, but ``.contains_replacement_characters`` is ``False``,
you'll know that the � was there originally (as it is in this
paragraph) and doesn't stand in for missing data.

Output encoding
---------------

When you write out a document from Beautiful Soup, you get a UTF-8
document, even if the document wasn't in UTF-8 to begin with. Here's a
document written in the Latin-1 encoding::

 markup = b'''
  <html>
   <head>
    <meta content="text/html; charset=ISO-Latin-1" http-equiv="Content-type" />
   </head>
   <body>
    <p>Sacr\xe9 bleu!</p>
   </body>
  </html>
 '''

 soup = BeautifulSoup(markup, 'html.parser')
 print(soup.prettify())
 # <html>
 #  <head>
 #   <meta content="text/html; charset=utf-8" http-equiv="Content-type" />
 #  </head>
 #  <body>
 #   <p>
 #    Sacré bleu!
 #   </p>
 #  </body>
 # </html>

Note that the <meta> tag has been rewritten to reflect the fact that
the document is now in UTF-8.

If you don't want UTF-8, you can pass an encoding into ``prettify()``::

 print(soup.prettify("latin-1"))
 # <html>
 #  <head>
 #   <meta content="text/html; charset=latin-1" http-equiv="Content-type" />
 # ...

You can also call encode() on the ``BeautifulSoup`` object, or any
element in the soup, just as if it were a Python string::

 soup.p.encode("latin-1")
 # b'<p>Sacr\xe9 bleu!</p>'

 soup.p.encode("utf-8")
 # b'<p>Sacr\xc3\xa9 bleu!</p>'

Any characters that can't be represented in your chosen encoding will
be converted into numeric XML entity references. Here's a document
that includes the Unicode character SNOWMAN::

 markup = u"<b>\N{SNOWMAN}</b>"
 snowman_soup = BeautifulSoup(markup, 'html.parser')
 tag = snowman_soup.b

The SNOWMAN character can be part of a UTF-8 document (it looks like
☃), but there's no representation for that character in ISO-Latin-1 or
ASCII, so it's converted into "&#9731" for those encodings::

 print(tag.encode("utf-8"))
 # b'<b>\xe2\x98\x83</b>'

 print(tag.encode("latin-1"))
 # b'<b>&#9731;</b>'

 print(tag.encode("ascii"))
 # b'<b>&#9731;</b>'

Unicode, Dammit
---------------

You can use Unicode, Dammit without using Beautiful Soup. It's useful
whenever you have data in an unknown encoding and you just want it to
become Unicode::

 from bs4 import UnicodeDammit
 dammit = UnicodeDammit("Sacr\xc3\xa9 bleu!")
 print(dammit.unicode_markup)
 # Sacré bleu!
 dammit.original_encoding
 # 'utf-8'

Unicode, Dammit's guesses will get a lot more accurate if you install
one of these Python libraries: ``charset-normalizer``, ``chardet``, or
``cchardet``. The more data you give Unicode, Dammit, the more
accurately it will guess. If you have your own suspicions as to what
the encoding might be, you can pass them in as a list::

 dammit = UnicodeDammit("Sacr\xe9 bleu!", ["latin-1", "iso-8859-1"])
 print(dammit.unicode_markup)
 # Sacré bleu!
 dammit.original_encoding
 # 'latin-1'

Unicode, Dammit has two special features that Beautiful Soup doesn't
use.

Smart quotes
^^^^^^^^^^^^

You can use Unicode, Dammit to convert Microsoft smart quotes to HTML or XML
entities::

 markup = b"<p>I just \x93love\x94 Microsoft Word\x92s smart quotes</p>"

 UnicodeDammit(markup, ["windows-1252"], smart_quotes_to="html").unicode_markup
 # '<p>I just &ldquo;love&rdquo; Microsoft Word&rsquo;s smart quotes</p>'

 UnicodeDammit(markup, ["windows-1252"], smart_quotes_to="xml").unicode_markup
 # '<p>I just &#x201C;love&#x201D; Microsoft Word&#x2019;s smart quotes</p>'

You can also convert Microsoft smart quotes to ASCII quotes::

 UnicodeDammit(markup, ["windows-1252"], smart_quotes_to="ascii").unicode_markup
 # '<p>I just "love" Microsoft Word\'s smart quotes</p>'

Hopefully you'll find this feature useful, but Beautiful Soup doesn't
use it. Beautiful Soup prefers the default behavior, which is to
convert Microsoft smart quotes to Unicode characters along with
everything else::

 UnicodeDammit(markup, ["windows-1252"]).unicode_markup
 # '<p>I just “love” Microsoft Word’s smart quotes</p>'

Inconsistent encodings
^^^^^^^^^^^^^^^^^^^^^^

Sometimes a document is mostly in UTF-8, but contains Windows-1252
characters such as (again) Microsoft smart quotes. This can happen
when a website includes data from multiple sources. You can use
``UnicodeDammit.detwingle()`` to turn such a document into pure
UTF-8. Here's a simple example::

 snowmen = (u"\N{SNOWMAN}" * 3)
 quote = (u"\N{LEFT DOUBLE QUOTATION MARK}I like snowmen!\N{RIGHT DOUBLE QUOTATION MARK}")
 doc = snowmen.encode("utf8") + quote.encode("windows_1252")

This document is a mess. The snowmen are in UTF-8 and the quotes are
in Windows-1252. You can display the snowmen or the quotes, but not
both::

 print(doc)
 # ☃☃☃�I like snowmen!�

 print(doc.decode("windows-1252"))
 # â˜ƒâ˜ƒâ˜ƒ“I like snowmen!”

Decoding the document as UTF-8 raises a ``UnicodeDecodeError``, and
decoding it as Windows-1252 gives you gibberish. Fortunately,
``UnicodeDammit.detwingle()`` will convert the string to pure UTF-8,
allowing you to decode it to Unicode and display the snowmen and quote
marks simultaneously::

 new_doc = UnicodeDammit.detwingle(doc)
 print(new_doc.decode("utf8"))
 # ☃☃☃“I like snowmen!”

``UnicodeDammit.detwingle()`` only knows how to handle Windows-1252
embedded in UTF-8 (or vice versa, I suppose), but this is the most
common case.

Note that you must know to call ``UnicodeDammit.detwingle()`` on your
data before passing it into ``BeautifulSoup`` or the ``UnicodeDammit``
constructor. Beautiful Soup assumes that a document has a single
encoding, whatever it might be. If you pass it a document that
contains both UTF-8 and Windows-1252, it's likely to think the whole
document is Windows-1252, and the document will come out looking like
``â˜ƒâ˜ƒâ˜ƒ“I like snowmen!”``.

``UnicodeDammit.detwingle()`` is new in Beautiful Soup 4.1.0.

Line numbers
============

The ``html.parser`` and ``html5lib`` parsers can keep track of where in
the original document each Tag was found. You can access this
information as ``Tag.sourceline`` (line number) and ``Tag.sourcepos``
(position of the start tag within a line)::

 markup = "<p\n>Paragraph 1</p>\n    <p>Paragraph 2</p>"
 soup = BeautifulSoup(markup, 'html.parser')
 for tag in soup.find_all('p'):
     print(repr((tag.sourceline, tag.sourcepos, tag.string)))
 # (1, 0, 'Paragraph 1')
 # (3, 4, 'Paragraph 2')

Note that the two parsers mean slightly different things by
``sourceline`` and ``sourcepos``. For html.parser, these numbers
represent the position of the initial less-than sign. For html5lib,
these numbers represent the position of the final greater-than sign::
   
 soup = BeautifulSoup(markup, 'html5lib')
 for tag in soup.find_all('p'):
     print(repr((tag.sourceline, tag.sourcepos, tag.string)))
 # (2, 0, 'Paragraph 1')
 # (3, 6, 'Paragraph 2')

You can shut off this feature by passing ``store_line_numbers=False`
into the ``BeautifulSoup`` constructor::

 markup = "<p\n>Paragraph 1</p>\n    <p>Paragraph 2</p>"
 soup = BeautifulSoup(markup, 'html.parser', store_line_numbers=False)
 print(soup.p.sourceline)
 # None
  
`This feature is new in 4.8.1, and the parsers based on lxml don't
support it.`

Comparing objects for equality
==============================

Beautiful Soup says that two ``NavigableString`` or ``Tag`` objects
are equal when they represent the same HTML or XML markup. In this
example, the two <b> tags are treated as equal, even though they live
in different parts of the object tree, because they both look like
"<b>pizza</b>"::

 markup = "<p>I want <b>pizza</b> and more <b>pizza</b>!</p>"
 soup = BeautifulSoup(markup, 'html.parser')
 first_b, second_b = soup.find_all('b')
 print(first_b == second_b)
 # True

 print(first_b.previous_element == second_b.previous_element)
 # False

If you want to see whether two variables refer to exactly the same
object, use `is`::

 print(first_b is second_b)
 # False

Copying Beautiful Soup objects
==============================

You can use ``copy.copy()`` to create a copy of any ``Tag`` or
``NavigableString``::

 import copy
 p_copy = copy.copy(soup.p)
 print(p_copy)
 # <p>I want <b>pizza</b> and more <b>pizza</b>!</p>

The copy is considered equal to the original, since it represents the
same markup as the original, but it's not the same object::

 print(soup.p == p_copy)
 # True

 print(soup.p is p_copy)
 # False

The only real difference is that the copy is completely detached from
the original Beautiful Soup object tree, just as if ``extract()`` had
been called on it::

 print(p_copy.parent)
 # None

This is because two different ``Tag`` objects can't occupy the same
space at the same time.

Advanced parser customization
=============================

Beautiful Soup offers a number of ways to customize how the parser
treats incoming HTML and XML. This section covers the most commonly
used customization techniques.

Parsing only part of a document
-------------------------------

Let's say you want to use Beautiful Soup look at a document's <a>
tags. It's a waste of time and memory to parse the entire document and
then go over it again looking for <a> tags. It would be much faster to
ignore everything that wasn't an <a> tag in the first place. The
``SoupStrainer`` class allows you to choose which parts of an incoming
document are parsed. You just create a ``SoupStrainer`` and pass it in
to the ``BeautifulSoup`` constructor as the ``parse_only`` argument.

(Note that *this feature won't work if you're using the html5lib parser*.
If you use html5lib, the whole document will be parsed, no
matter what. This is because html5lib constantly rearranges the parse
tree as it works, and if some part of the document didn't actually
make it into the parse tree, it'll crash. To avoid confusion, in the
examples below I'll be forcing Beautiful Soup to use Python's
built-in parser.)

``SoupStrainer``
^^^^^^^^^^^^^^^^

The ``SoupStrainer`` class takes the same arguments as a typical
method from `Searching the tree`_: :ref:`name <name>`, :ref:`attrs
<attrs>`, :ref:`string <string>`, and :ref:`**kwargs <kwargs>`. Here are
three ``SoupStrainer`` objects::

 from bs4 import SoupStrainer

 only_a_tags = SoupStrainer("a")

 only_tags_with_id_link2 = SoupStrainer(id="link2")

 def is_short_string(string):
     return string is not None and len(string) < 10

 only_short_strings = SoupStrainer(string=is_short_string)

I'm going to bring back the "three sisters" document one more time,
and we'll see what the document looks like when it's parsed with these
three ``SoupStrainer`` objects::

 html_doc = """<html><head><title>The Dormouse's story</title></head>
 <body>
 <p class="title"><b>The Dormouse's story</b></p>

 <p class="story">Once upon a time there were three little sisters; and their names were
 <a href="http://example.com/elsie" class="sister" id="link1">Elsie</a>,
 <a href="http://example.com/lacie" class="sister" id="link2">Lacie</a> and
 <a href="http://example.com/tillie" class="sister" id="link3">Tillie</a>;
 and they lived at the bottom of a well.</p>

 <p class="story">...</p>
 """

 print(BeautifulSoup(html_doc, "html.parser", parse_only=only_a_tags).prettify())
 # <a class="sister" href="http://example.com/elsie" id="link1">
 #  Elsie
 # </a>
 # <a class="sister" href="http://example.com/lacie" id="link2">
 #  Lacie
 # </a>
 # <a class="sister" href="http://example.com/tillie" id="link3">
 #  Tillie
 # </a>

 print(BeautifulSoup(html_doc, "html.parser", parse_only=only_tags_with_id_link2).prettify())
 # <a class="sister" href="http://example.com/lacie" id="link2">
 #  Lacie
 # </a>

 print(BeautifulSoup(html_doc, "html.parser", parse_only=only_short_strings).prettify())
 # Elsie
 # ,
 # Lacie
 # and
 # Tillie
 # ...
 #

You can also pass a ``SoupStrainer`` into any of the methods covered
in `Searching the tree`_. This probably isn't terribly useful, but I
thought I'd mention it::

 soup = BeautifulSoup(html_doc, 'html.parser')
 soup.find_all(only_short_strings)
 # ['\n\n', '\n\n', 'Elsie', ',\n', 'Lacie', ' and\n', 'Tillie',
 #  '\n\n', '...', '\n']

Customizing multi-valued attributes
-----------------------------------

In an HTML document, an attribute like ``class`` is given a list of
values, and an attribute like ``id`` is given a single value, because
the HTML specification treats those attributes differently::

 markup = '<a class="cls1 cls2" id="id1 id2">'
 soup = BeautifulSoup(markup, 'html.parser')
 soup.a['class']
 # ['cls1', 'cls2']
 soup.a['id']
 # 'id1 id2'

You can turn this off by passing in
``multi_valued_attributes=None``. Than all attributes will be given a
single value::

 soup = BeautifulSoup(markup, 'html.parser', multi_valued_attributes=None)
 soup.a['class']
 # 'cls1 cls2'
 soup.a['id']
 # 'id1 id2'

You can customize this behavior quite a bit by passing in a
dictionary for ``multi_valued_attributes``. If you need this, look at
``HTMLTreeBuilder.DEFAULT_CDATA_LIST_ATTRIBUTES`` to see the
configuration Beautiful Soup uses by default, which is based on the
HTML specification.

`(This is a new feature in Beautiful Soup 4.8.0.)`

Handling duplicate attributes
-----------------------------

When using the ``html.parser`` parser, you can use the
``on_duplicate_attribute`` constructor argument to customize what
Beautiful Soup does when it encounters a tag that defines the same
attribute more than once::

 markup = '<a href="http://url1/" href="http://url2/">'

The default behavior is to use the last value found for the tag::

 soup = BeautifulSoup(markup, 'html.parser')
 soup.a['href']
 # http://url2/

 soup = BeautifulSoup(markup, 'html.parser', on_duplicate_attribute='replace')
 soup.a['href']
 # http://url2/
  
With ``on_duplicate_attribute='ignore'`` you can tell Beautiful Soup
to use the `first` value found and ignore the rest::

 soup = BeautifulSoup(markup, 'html.parser', on_duplicate_attribute='ignore')
 soup.a['href']
 # http://url1/

(lxml and html5lib always do it this way; their behavior can't be
configured from within Beautiful Soup.)

If you need more, you can pass in a function that's called on each duplicate value::

 def accumulate(attributes_so_far, key, value):
     if not isinstance(attributes_so_far[key], list):
         attributes_so_far[key] = [attributes_so_far[key]]
     attributes_so_far[key].append(value)

 soup = BeautifulSoup(markup, 'html.parser', on_duplicate_attribute=accumulate)
 soup.a['href']
 # ["http://url1/", "http://url2/"]

`(This is a new feature in Beautiful Soup 4.9.1.)`

Instantiating custom subclasses
-------------------------------

When a parser tells Beautiful Soup about a tag or a string, Beautiful
Soup will instantiate a ``Tag`` or ``NavigableString`` object to
contain that information. Instead of that default behavior, you can
tell Beautiful Soup to instantiate `subclasses` of ``Tag`` or
``NavigableString``, subclasses you define with custom behavior::

 from bs4 import Tag, NavigableString
 class MyTag(Tag):
     pass


 class MyString(NavigableString):
     pass


 markup = "<div>some text</div>"
 soup = BeautifulSoup(markup, 'html.parser')
 isinstance(soup.div, MyTag)
 # False
 isinstance(soup.div.string, MyString)
 # False 

 my_classes = { Tag: MyTag, NavigableString: MyString }
 soup = BeautifulSoup(markup, 'html.parser', element_classes=my_classes)
 isinstance(soup.div, MyTag)
 # True
 isinstance(soup.div.string, MyString)
 # True  

This can be useful when incorporating Beautiful Soup into a test
framework.

`(This is a new feature in Beautiful Soup 4.8.1.)`

Αντιμετώπιση προβλημάτων
===============

.. _diagnose:

``diagnose()``
--------------

Αν δυσκολεύεστε να κατανοήσετε τι κάνει η Beautiful Soup σε κάποιο
έγγραφο, περάστε το έγγραφο στη συνάρτηση ``diagnose()``. (Νέα στην
Beautiful Soup 4.2.0.) Η Beautiful Soup θα εμφανίσει μια αναφορά που θα δείχνει
πως χειρίζονται το έγγραφο διαφορετικοί αναλυτές και σας λέει αν σας
λείπει κάποιος αναλυτής που θα μπορούσε να χρησιμοποιεί η Beautiful Soup::

 from bs4.diagnose import diagnose
 with open("bad.html") as fp:
     data = fp.read()

 diagnose(data)

 # Diagnostic running on Beautiful Soup 4.2.0
 # Python version 2.7.3 (default, Aug  1 2012, 05:16:07)
 # I noticed that html5lib is not installed. Installing it may help.
 # Found lxml version 2.3.2.0
 #
 # Trying to parse your data with html.parser
 # Here's what html.parser did with the document:
 # ...

Απλά κοιτάζοντας την έξοδο της diagnose() μπορεί να σας δείξει πώς να λύσετε το πρόβλημα. 
Ακόμα και αν όχι, μπορείτε να παρουσιάσετε την έξοδο της ``diagnose()`` όταν
ζητάτε για βοήθεια.

Σφάλματα κατά την ανάλυση εγγράφου
------------------------------


Υπάρχουν δύο διαφορετικά είδη σφαλμάτων ανάλυσης. Υπάρχουν κρασαρίσματα (crashes),
όταν τροφοδοτείτε ένα έγγραφο στην Beautiful Soup και εγείρει μια
εξαίρεση, συνήθως ένα ``HTMLParser.HTMLParseError``. Και επίσης υπάρχουν
απροσδόκητες συμπεριφορές, όταν ένα Beautiful Soup δέντρο ανάλυσης φαίνεται πολύ
διαφορετικό από το έγγραφο που χρησιμοποιήθηκε για τη δημιουργία του.

Σχεδόν κανένα από αυτά τα προβλήματα δεν καταλήγει να είναι προβλήματα με την Beautiful Soup.
Αυτό δεν συμβαίνει επειδή η Beautiful Soup είναι ένα εκπληκτικά καλογραμμένο
κομμάτι λογισμικού. Είναι επειδή η Beautiful Soup δεν περιλαμβάνει κανένα
κώδικα ανάλυσης. Αντιθέτως, βασίζεται σε εξωτερικούς αναλυτές. Αν ένας αναλυτής
δεν λειτουργεί σε ένα συγκεκριμένο έγγραφο, η καλύτερη λύση είναι να δοκιμάσετε έναν
διαφορετικό αναλυτή. Δείτε `Εγκατάσταση αναλυτή`_ για λεπτομέρειες και συγκρίσεις αναλυτών.

Τα πιο κοινά σφάλματα ανάλυσης είναι τα ``HTMLParser.HTMLParseError:
malformed start tag`` και ``HTMLParser.HTMLParseError: bad end
tag``. Και τα δύο γεννιούνται απο την εσωτερική βιβλιοθήκη ανάλυσης HTML της Python,
και η λύση είναι να :ref:`εγκατάσταση του lxml ή html5lib. <parser-installation>`

Ο πιο κοινός τύπος απροσδόκητης συμπεριφοράς είναι όταν δε μπορείτε να βρείτε κάποιο
tag το οποίο ξέρετε πως βρίσκεται στο έγγραφο. Το είδατε να "εισέρχεται", αλλά η
``find_all()`` επιστρέφει ``[]`` ή η ``find()`` επιστρέφει ``None``. Αυτό είναι
ένα άλλο συχνό πρόβλημα με την εσωτερική βιβλιοθήκη ανάλυσης HTML της Python, η οποία
κάποιες φορές παραλείπει tags τα οποία δεν καταλαβαίνει.  Και πάλι, η καλύτερη λύση είναι η
:ref:`εγκατάσταση του lxml ή html5lib. <parser-installation>`

Προβλήματα αναντιστοιχίας εκδόσεων
-------------------------

* ``SyntaxError: Invalid syntax`` (on the line ``ROOT_TAG_NAME =
  '[document]'``): - Προκαλείται από την εκτέλεση μιας παλιάς έκδοσης Python 2 της
  Beautiful Soup σε Python 3, χωρίς να μετατρέψετε τον κώδικα.

* ``ImportError: No module named HTMLParser`` - Προκαλείται από την εκτέλεση μιας παλιάς έκδοσης Python 2 της
  Beautiful Soup σε Python 3.

* ``ImportError: No module named html.parser`` - Προκαλείται από την εκτέλεση της έκδοσης Python 3 της
  Beautiful Soup σε Python 2.

* ``ImportError: No module named BeautifulSoup`` - Προκαλείται από την εκτέλεση
  κώδικα Beautiful Soup 3 σε ένα σύστημα που δεν έχει την bs3
  εγκατεστημένη. Ή, απο την συγγραφή κώδικα Beautiful Soup 4 χωρίς να γνωρίζεται οτι
  το όνομα του πακέτου έχει μετονομαστεί σε ``bs4``.

* ``ImportError: No module named bs4`` - Προκαλείται από την εκτέλεση κώδικα Beautiful
  Soup 4 σε ένα σύστημα που δεν έχει την BS4 εγκατεστημένη.

.. _parsing-xml:

Αναλύοντας XML
-----------

Από προεπιλογή, η Beautiful Soup αναλύει τα έγγραφα ως HTML. Για ανάλυση ενός
εγγράφου ως XML, δώστε την λέξη "xml" ως το δεύτερο όρισμα στον
κατασκευαστή ``BeautifulSoup``::

 soup = BeautifulSoup(markup, "xml")

Θα πρέπει να :ref:`έχετε τον lxml εγκατεστημένο <parser-installation>`.

Άλλα προβήματα αναλυτών
---------------------

* Εάν το script σας λειτουργεί σε έναν υπολογιστή αλλά όχι σε άλλον ή σε έναν
  εικονικό περιβάλλον αλλά όχι άλλο, ή έξω από το εικονικό
  περιβάλλον αλλά όχι μέσα σε αυτό, είναι πιθανώς επειδή τα δύο
  περιβάλλοντα διαθέτουν διαφορετικές βιβλιοθήκες ανάλυσης διαθέσιμες. Για παράδειγμα,
  μπορεί να έχετε αναπτύξει το script σε υπολογιστή που έχει τον lxml
  εγκατεστημένο, και στη συνέχεια, να προσπαθήσατε να το εκτελέσετε σε έναν υπολογιστή που έχει μόνο
  τον html5lib εγκατεστημένο. Δείτε το `Διαφορές μεταξύ των αναλυτών`_ για το γιατί αυτό
  έχει σημασία και διορθώστε το πρόβλημα αναφέροντας μια συγκεκριμένη βιβλιοθήκη αναλυτών
  στον κατασκευαστή ``BeautifulSoup``.

* Επειδή οι `ετικέτες HTML και οι ιδιότητες δεν κάνουν διάκριση πεζών-κεφαλαίων
  <http://www.w3.org/TR/html5/syntax.html#syntax>`_, και οι τρείς 
  αναλυτές HTML μετατρέπουν ονόματα ετικετών και ιδιοτήτων σε πεζά. Για παράδειγμα
  η σήμανση <TAG></TAG> μετατρέπεται σε <tag></tag>. Αν θέλετε να διατηρήσετε την μίξη πεζών-κεφαλαίων
  ή την κεφαλαιοποίηση των ιδιοτήτων, θα πρέπει να :ref:`αναλύστε το έγγραφο ως XML. <parsing-xml>`

.. _misc:

Διάφορα
-------------

* ``UnicodeEncodeError: 'charmap' codec can't encode character
  '\xfoo' in position bar`` (ή οποιοδήποτε άλλο
  ``UnicodeEncodeError``) - Αυτό το πρόβλημα εμφανίζεται σε δυο
  περιπτώσεις. Αρχικά, όταν προσπαθείτε να εμφανίσετε έναν Unicode χαρακτήρα τον οποίο
  δε γνωρίζει πως να εμφανίσει η κονσόλα. (Δείτε `αυτή τη σελίδα στο
  wiki της Python <http://wiki.python.org/moin/PrintFails>`_ για βοήθεια.)
  Δεύτερον, όταν γράφετε σε ένα αρχείο και του περνάτε έναν Unicode
  χαρακτήρα ο οποίος δεν υποστηρίζεται απο την προεπιλεγμένη κωδικοποίηση. Σε αυτή την
  περίπτωση, η απλούστερη λύση είναι να κωδικοποιήσετε ρητά την Unicode ακολουθία
  χαρακτήρων σε UTF-8 με την ``u.encode("utf8")``.

* ``KeyError: [attr]`` - Προκαλείται όταν προσπαθείτε να επιλέξετε την ``tag['attr']`` και το
  συγκεκριμένο tag δεν ορίζει την ιδιότητα ``attr``. Τα πιο
  συνηθισμένα σφάλματα είναι τα ``KeyError: 'href'`` και ``KeyError: 'class'``.
  Χρησιμοποιήστε την ``tag.get('attr')`` αν δεν είστε σίγουροι αν η ``attr`` είναι
  ορισμένη, jόπως ακριβώς θα κάνατε με ένα λεξικό της Python.

* ``AttributeError: 'ResultSet' object has no attribute 'foo'`` - Αυτό
  συνήθως συμβαίνει επειδή περιμένατε απο την ``find_all()`` να επιστρέψει ένα
  μοναδικό tag ή συμβολοσειρά. Όμως η ``find_all()`` επιστρέφει μια _λίστα_ απο tags
  και συμβολοσειρές--ένα αντικείμενο ``ResultSet``. Πρέπει να προσπελάσετε την
  λίστα και να κοιτάξετε στήν ``.foo`` κάθε ενός. Διαφορετικά, αν πραγματικά 
  χρειάζεστε μόνο ένα αποτέλεσμα, πρέπει να χρησιμοποιήσετε την ``find()`` αντί της
  ``find_all()``.

* ``AttributeError: 'NoneType' object has no attribute 'foo'`` - Αυτό
  συνήθως συμβαίνει επειδή καλέσατε την ``find()`` και έπειτα προσπαθήσατε να
  επιλέξετε την ιδιότητα `.foo`` του αποτελέσματος. Όμως στη περίπτωση σας, η
  ``find()`` δεν βρήκε τίποτα, οπότε επέστρεψε ``None``, αντί να επιστρέψει
  ένα tag ή μια συμβολοσειρά. Πρέπει να ανακαλύψετε γιατί η κλήση της
  ``find()`` δεν επιστρέφει κάτι.

* ``AttributeError: 'NavigableString' object has no attribute
  'foo'`` - Αυτό συνήθως συμβαίνει επειδή χειρίζεστε μια συμβολοσειρά σαν
  να ήταν ένα tag. Μπορεί να προσπελαύνεται μια λίστα, περιμένοντας οτι
  περιέχει μόνο tags και τίποτα άλλο, αλλά στην πραγματικότητα περιέχει και tags και
  συμβολοσειρές.


Βελτιώνοντας την Επίδοση
---------------------

Η Beautiful Soup δεν θα είναι ποτέ τόσο γρήγορη όσο οι αναλυτές τους οποίους
χρησιμοποιεί. Εάν ο χρόνος απόκρισης είναι κρίσιμος, εάν πληρώνετε τον χρόνο χρήσης του υπολογιστή
με την ώρα ή αν υπάρχει οποιοσδήποτε άλλος λόγος για τον οποίο ο χρόνος χρήσης του υπολογιστή είναι πιο
πολύτιμος από τον χρόνο του προγραμματιστή, θα πρέπει να ξεχάσετε την Beautiful Soup
και να εργαστείτε απευθείας πάνω στον `lxml <http://lxml.de/>`_.

Παρόλα αυτά, υπάρχουν πράγματα που μπορείτε να κάνετε για να επιταχύνετε την Beautiful Soup. Αν
δεν χρησιμοποιείτε τον lxml ως τον υποκείμενο αναλυτή, η συμβουλή είναι να το
:ref:`κάνετε <parser-installation>`. Η Beautiful Soup αναλύει έγγραφα
πολύ πιο γρήγορα χρησιμοποιώντας τον lxml από ό,τι χρησιμοποιώντας τον html.parser ή html5lib.

Μπορείτε να επιταχύνετε σημαντικά τον εντοπισμό κωδικοποίησης εγκαθιστώντας την
`cchardet <http://pypi.python.org/pypi/cchardet/>`_ βιβλιοθήκη.

Η `ανάλυση μόνο μέρους ενός εγγράφου`_ δεν θα σας εξοικονομήσει πολύ χρόνο από την ανάλυση
του εγγράφου, αλλά μπορεί να εξοικονομήσει πολλή μνήμη και θα κάνει την
`αναζήτηση` του εγγράφου πολύ πιο γρήγορη.


Μεταφράζοντας αυτές τις οδηγίες
==============================

Νέες μεταφράσεις των οδηγιών της Beautiful Soup είναι ιδιαίτερα
ευπρόσδεκτες. Οι μεταφράσεις πρέπει να είναι αδειοδοτημένες υπο την άδεια MIT,
ακριβώς όπως και η Beautiful Soup αλλά και οι οδηγίες στα Αγγλικά.

Υπάρχουν δυο τρόποι για να περάσει η μετάφρασή σας στην κύρια βάση κώδικα
και στην ιστοσελίδα της Beautiful Soup:

1. Δημιουργήστε ένα branch στο αποθετήριο της Beautiful Soup, προσθέστε την
   μετάφρασή σας, και προτείνετε μια συγχώνευση με το κύριο branch, με τον ίδιο τρόπο
   που θα προτείνατε μια αλλαγή στον πηγαίο κώδικα. 
2. Στείλτε ένα μήνυμα στην ομάδα συζητήσεων της Beautiful Soup με ένα λίνκ στην
   μετάφρασή σας, ή επισυνάψτε την μετάφραση στο μήνυμά σας.

Χρησιμοποιήστε την Κινέζικη ή την Πορτογαλική-Βραζιλίας μετάφραση σαν μοντέλο.
Συγκεκριμένα, παρακαλώ μεταφράστε το πηγαίο αρχείο ``doc/source/index.rst``,
αντί για την HTML έκδοση των οδηγιών. Αυτό επιτρέπει την δημοσίευση
των οδηγιών σε μια πληθώρα μορφών, όχι απλά σε HTML.

Beautiful Soup 3
================

Η Beautiful Soup 3 είναι η προηγούμενη σειρά κυκλοφορίας, και δεν βρίσκεται
πλέον σε ενεργή ανάπτυξη. Πρός το παρόν περιέχεται σε όλες τις κύριες διανομές
Linux:

:kbd:`$ apt-get install python-beautifulsoup`

Επίσης δημοσιεύεται μέσω του PyPi ως ``BeautifulSoup``.:

:kbd:`$ easy_install BeautifulSoup`

:kbd:`$ pip install BeautifulSoup`

Μπορείτε επιπλέον να `κατεβάσετε ένα tarball της Beautiful Soup 3.2.0
<http://www.crummy.com/software/BeautifulSoup/bs3/download/3.x/BeautifulSoup-3.2.0.tar.gz>`_.

Αν εκτελέσατε τις ``easy_install beautifulsoup`` ή ``easy_install
BeautifulSoup``, αλλά ο κώδικάς σας δεν λειτουργεί, εγκαταστήσατε την Beautiful
Soup 3 κατά λάθος. Πρέπει να εκτελέσετε την ``easy_install beautifulsoup4``.

`Οι οδηγίες χρήσης της Beautiful Soup 3 είναι αρχειοθετημένη διαδικτυακά
<http://www.crummy.com/software/BeautifulSoup/bs3/documentation.html>`_.

Μετατροπή κώδικα σε BS4
-------------------

Ο περισσότερος κώδικας ποτ γράφτηκε για Beautiful Soup 3 θα δουλεύει στην Beautiful
Soup 4 μια απλή αλλαγή. Το μόνο που πρέπει να κάνετε είναι να αλλάξετε το
όνομα του πακέτου απο ``BeautifulSoup`` σε ``bs4``. Οπότε αυτό::

 from BeautifulSoup import BeautifulSoup

γίνεται έτσι::

 from bs4 import BeautifulSoup

* Αν αντιμετωπίσετε το ``ImportError`` "No module named BeautifulSoup", το 
  πρόβλημά σας είναι οτι προσπαθείτε να εκτελέσετε κώδικα Beautiful Soup 3, αλλά έχετε
  μόνο την Beautiful Soup 4 εγκατεστημένη.

* Αν αντιμετωπίσετε το ``ImportError`` "No module named bs4", yτο 
  πρόβλημά σας είναι οτι προσπαθείτε να εκτελέσετε κώδικα Beautiful Soup 4, αλλά έχετε
  μόνο την Beautiful Soup 3 εγκατεστημένη.

Παρόλο που η BS4 είναι κατά κύριο λόγο οπισθόδρομα συμβατή με την BS3, οι περισσότερες απο τις
μεθόδους της έχουν παρωχηθεί και τους έχουν δωθεί ονόματα συμβατά με τον οδηγό `PEP 8
<http://www.python.org/dev/peps/pep-0008/>`_. Υπάρχει μια πληθώρα άλλων μετονομασιών και
αλλαγών, και μόνο μερικές απο αυτές χαλάνε την οπισθόδρομη συμβατότητα.

Παρακάτω είναι οτιδήποτε χρειάζεστε για να μετατρέψετε τον κώδικα και τις "συνήθειες" της BS3 σε BS4:

Χρειάζεστε αναλυτή
^^^^^^^^^^^^^^^^^

Η Beautiful Soup 3 χρησιμοποιούσε τον αναλυτή ``SGMLParser`` της Python, ένα module το οποίο ήταν
παρωχημένο και αφαιρέθηκε στην Python 3.0. Η Beautiful Soup 4 χρησιμοποιεί τον
``html.parser`` απο προεπιλογή, αλλά μπορείτε να προσθέσετε τον lxml ή html5lib και να
χρησιμοποιήσετε αυτούς. Δείτε το `Εγκατάσταση αναλυτή`_ για μια σύγκριση.

Εφόσων ο ``html.parser`` δεν είναι ο ίδιος αναλυτής με τον ``SGMLParser``, μπορεί
να παρατηρήσετε οτι η Beautiful Soup 4 δίνει διαφορετικό δέντρο ανάλυσης απο την
Beautiful Soup 3 για την ίδια σήμανση (markup). Αν αλλάξετε τον ``html.parser``
για τον lxml ή html5lib, υπάρχει η πιθανότητα το δέντρο ανάλυσης να είναι πάλι διαφορετικό.
Αν συμβεί αυτό, χρειάζεται να αναβαθμίσετε τον κώδικα scraping για να δουλεύει με το νέο δέντρο.

Ονόματα μεθόδων
^^^^^^^^^^^^

* ``renderContents`` -> ``encode_contents``
* ``replaceWith`` -> ``replace_with``
* ``replaceWithChildren`` -> ``unwrap``
* ``findAll`` -> ``find_all``
* ``findAllNext`` -> ``find_all_next``
* ``findAllPrevious`` -> ``find_all_previous``
* ``findNext`` -> ``find_next``
* ``findNextSibling`` -> ``find_next_sibling``
* ``findNextSiblings`` -> ``find_next_siblings``
* ``findParent`` -> ``find_parent``
* ``findParents`` -> ``find_parents``
* ``findPrevious`` -> ``find_previous``
* ``findPreviousSibling`` -> ``find_previous_sibling``
* ``findPreviousSiblings`` -> ``find_previous_siblings``
* ``getText`` -> ``get_text``
* ``nextSibling`` -> ``next_sibling``
* ``previousSibling`` -> ``previous_sibling``

Κάποιες παράμετροι στον κατασκευαστή Beautiful Soup μετονομάστηκαν για τους
ίδιους λόγους:

* ``BeautifulSoup(parseOnlyThese=...)`` -> ``BeautifulSoup(parse_only=...)``
* ``BeautifulSoup(fromEncoding=...)`` -> ``BeautifulSoup(from_encoding=...)``

Μια μέθοδος μετονομάστηκε για συμβατότητα με την Python 3:

* ``Tag.has_key()`` -> ``Tag.has_attr()``

Μια ιδιότητα μετονομάστηκε χάρην πιο ακριβής ορολογίας:

* ``Tag.isSelfClosing`` -> ``Tag.is_empty_element``

Τρείς ιδιότητες μετονομάστηκαν για να αποφευχθεί η χρήση λέξεων που έχουν ειδική
σημασία στην Python. Σε αντίθεση με τις άλλες, αυτές οι αλλαγές *δεν είναι οπισθόδρομα
συμβατές.* Αν έχετε χρησιμοποιήσει αυτές τις ιδιότητες στην BS3, ο κώδικάς σας θα χαλάσει
στην BS4 μέχρι να τις αλλάξετε.

* ``UnicodeDammit.unicode`` -> ``UnicodeDammit.unicode_markup``
* ``Tag.next`` -> ``Tag.next_element``
* ``Tag.previous`` -> ``Tag.previous_element``

Αυτές οι μέθοδοι έχουν μείνει απο το Beautiful Soup 2 API. Είναι
παρωχημένες απο το 2006, και δε πρέπει να χρησιμοποιούνται καθόλου:

* ``Tag.fetchNextSiblings``
* ``Tag.fetchPreviousSiblings``
* ``Tag.fetchPrevious``
* ``Tag.fetchPreviousSiblings``
* ``Tag.fetchParents``
* ``Tag.findChild``
* ``Tag.findChildren``


Γεννήτριες
^^^^^^^^^^

Οι γεννήτριες έχουν ονόματα συμβατά με τον οδηγό στύλ PEP 8, και έχουν μετατραπεί σε ιδιότητες:

* ``childGenerator()`` -> ``children``
* ``nextGenerator()`` -> ``next_elements``
* ``nextSiblingGenerator()`` -> ``next_siblings``
* ``previousGenerator()`` -> ``previous_elements``
* ``previousSiblingGenerator()`` -> ``previous_siblings``
* ``recursiveChildGenerator()`` -> ``descendants``
* ``parentGenerator()`` -> ``parents``

Έτσι αντί γι' αυτό::

 for parent in tag.parentGenerator():
     ...

Μπορείτε να γράψετε αυτό::

 for parent in tag.parents:
     ...

(Αλλά ο παλιός κώδικας λειτουργεί ακόμα.)

Κάποιες γεννήτριες συνήθιζαν να παράγουν την τιμή ``None`` όταν είχαν τελειώσει, και
έπειτα να σταματάνε. Αυτό ήταν σφάλμα. Πλέον οι γεννήτριες απλά σταματάνε.

Υπάρχουν δυο νέες γεννήτριες, :ref:`.strings και
.stripped_strings <string-generators>`. ``.strings`` παράγει αντικείμενα 
NavigableString, και ``.stripped_strings`` αποδίδει Python
strings, "ξεγυμνωμένα" απο χαρακτήρες κενού (whitespace).

XML
^^^

Πλέον δεν υπάρχει η κλάση ``BeautifulStoneSoup`` για ανάλυση XML. Για
να αναλύσετε XML περνάτε την λέξη "xml" σαν δεύτερη παράμετρη στον κατασκευαστή
``BeautifulSoup``. Για τον ίδιο λόγο, ο κατασκευαστής
``BeautifulSoup`` δεν αναγνωρίζει πλέον την παράμετρο ``isHTML``.

Ο τρόπος διαχείρησης της Beautiful Soup των άδειων-απο-στοιχεία XML tags έχει
βελτιωθεί. Προηγουμένως όταν αναλύατε XML έπρεπε να αναφέρετε ρητά
ποιά tags θεωρούντουσαν άδεια-απο-στοιχεία tags. Η παράμετρος ``selfClosingTags``
στον κατασκευαστή δεν αναγνωρίζεται πλέον. Αντιθέτως, η
Beautiful Soup θεωρεί οποιοδήποτε άδειο tag ως άδειο-απο-στοιχεία tag. Αν
προσθέσετε ένα παιδί σε κάποιο άδειο-απο-στοιχεία tag, σταματάει να είναι άδειο-απο-στοιχεία tag.

Οντότητες
^^^^^^^^

Μια εισερχόμενη HTML ή XML οντότηα μετατρέπεται πάντα στον
αντίστοιχο Unicode χαρακτήρα. Η Beautiful Soup 3 είχε έναν αριθμό
απο επικαλυπτόμενους τρόπους διαχείρησης οντοτήτων, οι οποίοι έχουν
αφαιρεθεί. Ο κατασκευαστής ``BeautifulSoup`` πλέον δεν αναγνωρίζει τις
παραμέτρους ``smartQuotesTo`` ή ``convertEntities``. (`Unicode,
Dammit`_ ακόμα έχει την ``smart_quotes_to``, αλλά η προεπιλογή πλέον έιναι
να μετατραπούν τα έξυπνα εισαγωγικά (smart quotes) σε Unicode). Οι σταθερές ``HTML_ENTITIES``,
``XML_ENTITIES``, και ``XHTML_ENTITIES`` έχουν αφαιρεθεί, αφού
ρυθμίζουν ένα χαρακτηριστικό (μετατροπή κάποιων αλλά οχι όλων των οντοτήτων
σε Unicode χαρακτήρες) το οποίο πλέον δεν υπάρχει.

Άν θέλετε να μετατρέψετε χαρακτήρες Unicode πίσω σε HTML οντότητες στην
έξοδο, αντί να τις μετατρέψετε σε UTF-8 χαρακτήρες, πρέπει να χρησιμοποιήσετε
έναν :ref:`μορφοποιητή εξόδου <output_formatters>`.

Διάφορα
^^^^^^^^^^^^^

Το :ref:`Tag.string <.string>` πλέον δρά αναδρομικά. Αν ένα tag A
περιέχει ένα μοναδικό tag B και τίποτα άλλο, τότε το A.string είναι το ίδιο με το
B.string. (Προηγουμένος, ήταν None.)

`Ιδιότητες πολλαπλών τιμών`_ όπως η ``class`` έχουν λίστες απο συμβολοσειρές
ως τιμές τους, όχι σκέτες συμβολοσειρές. Αυτό μπορεί να να επηρρεάσει τον τρόπο που
αναζητάτε βάση κλάσεων CSS.

Τα αντικείμενα ``Tag`` πλέον υλοποιούν την μέθοδο ``__hash__``, έτσι ώστε δύο
αντικείμενα ``Tag`` θεωρούνται ίδια αν δημιουργούν την ίδια σήμανση (markup).
Αυτό μπορεί να αλλάξει την συμπεριφορά του Script σας αν βάλετε αντικείμενα ``Tag``
σε ένα λεξιλόγιο (Python dictionary) ή ένα σέτ (Python set).

Αν δώσετε σε κάποια απο τις μεθόδους ``find*`` ένα :ref:`string <string>` `και`
μια σχετική-με-tags παράμετρο όπως το :ref:`name <name>`, η Beautiful Soup θα
αναζητήσει για tags τα οποία ταιριάζουν με το κριτήριο της σχετική-με-το-tag παραμέτρου και των οποίων
το :ref:`Tag.string <.string>` ταιριάζει με την τιμή της παραμέτρου :ref:`string
<string>`. `Δεν` θα βρεί τις συμβολοσειρές καθαυτές. Προηγουμένως,
η Beautiful Soup αγνοούσε τις σχετικές-με-το-tag παραμέτρους και έψαχνε για τις συμβολοσειρές.

Ο κατασκευαστής ``BeautifulSoup`` πλέον δεν αναγνωρίζει την παράμετρο
`markupMassage`. Είναι πλέον δουλειά του αναλυτή να διαχειριστεί
την σήμανση (markup) με σωστό τρόπο.

Οι σπανίως χρησιμοποιούμενοι αναλυτές όπως οι
``ICantBelieveItsBeautifulSoup`` και ``BeautifulSOAP`` έχουν
αφαιρεθεί. Είναι πλέον δουλειά του αναλυτή να διαχειριστεί 
οποιαδήποτε διφορούμενη σήμανση (markup) με σωστό τρόπο.

Η μέθοδος ``prettify()`` πλέον επιστρέφει μια Unicode συμβολοσειρά (String), όχι ένα bytestring.
