Mowa
------

.. warning::

	UWAGA! TO JEST KOD ALFA!

	Zastrzegamy sobie prawo do zmiany tego interfejsu API w miarę rozwoju 	oprogramowania.

	Jakość mowy nie jest świetna, tylko "wystarczająco dobra". Ze względu na 	ograniczenia urządzenia, mogą wystąpić błędy pamięci 	i / lub nieoczekiwane 	dodatkowe dźwięki podczas odtwarzania. To są dopiero początki i cały czas 	poprawiamy kod syntezatora mowy. Zgłaszanie błędów będzie mile widziane.
Komputery i roboty, które mówią wydają się bardziej "ludzkie".

Często o tym co komputer właśnie robi dowiadujemy się poprzez graficzny interfejs użytkownika (ang. graphical user interface, GUI). W przypadku BBC micro:bit, GUI to matryca LED 5x5, która pozostawia wiele do życzenia.

Sprawienie, aby micro:bit mówił, jest jednym ze sposobów na wyrażenie informacji  w zabawny, skuteczny i użyteczny sposób. W tym celu zintegrowaliśmy prosty syntezator mowy oparty na odwrotnie zaprojektowanej wersji syntezatora z wczesnych lat '80. Brzmi naprawdę uroczo, w stylu "wszyscy ludzie muszą umrzeć".

Mając to na uwadze, użyjemy syntezatora do stworzenia...

Poezji DALEKów
++++++++++++

.. image:: dalek.jpg

Mało kto wie, że DALEKowie lubią poezję - szczególnie limeryki. Szaleją na punkcie wierszy anapestycznych o surowej budowie AABBA. Kto by pomyślał!

(Jak się niżej dowiemy, to wina Doktora, że, ku irytacji Davrosa, DALEKowie lubią limeryki.)

W każdym razie, zamierzamy stworzyć recital poezji DALEKowej na żądanie.

Powiedz coś
+++++++++++++

Zanim urządzenie będzie mogło mówić, musisz podłączyć głośniczek w taki sposób:

.. image:: ../speech.png

Najłatwiejszym sposobem na to żeby urządzenie zaczęło mówić jest zaimportowanie modułu ``speech`` (pol. ``mowa``) i użycie funkcji ``say`` (pol. ``powiedz``) w taki sposób:: 

    import speech

    speech.say("Hello, World")

Jest to urocze, jednak nie jest wystarczająco DALEKowate jak na nasz gust, dlatego musimy zmienić niektóre parametry używane przez syntezator do wygenerowania mowy. Nasz syntezator mowy jest pod tym względem dość potężny, ponieważ możemy zmienić cztery parametry:

* ``pitch`` (pol. ``ton``) - jak wysoko lub nisko brzmi głos (0 = wysoko, 255 = Barry White)
* ``speed`` (pol. ``szybkość``) - jak szybko urządzenie mówi (0 = niemożliwe, 255 = opowiadanie na dobranoc)
* ``mouth`` (pol. ``usta``) - czy mowa jest przez zaciśnięte zęby czy bardzo wyraźna (0 = smoczek brzuchomówcy, 255 = Foghorn Leghorn)
* ``throat`` (pol. ``gardło``) - jak spokojny lub napięty jest ton głosu (0 = załamujący się, 255 = totalnie zrelaksowany)

W sumie razem parametry te kontrolują jakość dźwięku - t.j barwa dźwięku. Szczerze mówiąc, najlepszą drogą do uzyskania porządanej barwy dźwięku jest eksperymentowanie, ocena i dostosowanie.

Aby dostosować ustawienia, przekazujesz je jako argumenty funkcji ``say``. Więcej szczegółów można znaleźć w dokumentacji interfejsu API ``speech``.

Po kilku przeprowadzonych eksperymentach, ten brzmi całkiem DALEKowato::

    speech.say("I am a DALEK - EXTERMINATE", speed=120, pitch=100, throat=100, mouth=200)

Poezja na Żądanie
++++++++++++++++

Będąc cyborgami, DALEKowie wykorzystują umiejętności robotów do tworzenia poezji
i okazuje się, że korzystają z algorytmów napisanych w Python, np::

	# Generator poezji DALEKowej Doktora
    import speech
    import random
    from microbit import sleep

	# Losowo wybierz fragmenty do wstawienia do szablonu.
    location = random.choice(["brent", "trent", "kent", "tashkent"])
    action = random.choice(["wrapped up", "covered", "sang to", "played games with"])
    obj = random.choice(["head", "hand", "dog", "foot"])
    prop = random.choice(["in a tent", "with cement", "with some scent",
                         "that was bent"])
    result = random.choice(["it ran off", "it glowed", "it blew up",
                           "it turned blue"])
    attitude = random.choice(["in the park", "like a shark", "for a lark",
                             "with a bark"])
    conclusion = random.choice(["where it went", "its intent", "why it went",
                               "what it meant"])

    # Szablon wiersza. Nawiasy {} będą zastąpione nazwanymi fragmentami. 
    poem = [
        "there was a young man from {}".format(location),
        "who {} his {} {}".format(action, obj, prop),
        "one night after dark",
        "{} {}".format(result, attitude),
        "and he never worked out {}".format(conclusion),
        "EXTERMINATE",
    ]

	# Wprowadź w pętlę każdą linię wiersza i użyj modułu
    for line in poem:
        speech.say(line, speed=120, pitch=100, throat=100, mouth=200)
        sleep(500)

Jak pokazują komentarze, jest to bardzo prosty model:

* Nazwane fragmenty (`` location``, `` prop``, `` attitude`` itd.) są generowane losowo z predefiniowanych list możliwych wartości. Zwróć uwagę na użycie `` random.choice`` w celu wybrania pojedynczego elementu z listy.
* Szablon wiersza definiowany jest jako lista zwrotek z "dziurami" (oznaczonymi przez `` {} ``), do których za pomocą metody `` format`` wstawione zostaną nazwane fragmenty.
* Na koniec, Python wykonuje pętlę nad każdym elementem na liście wypełniaczy poezji i używa `` speech.say`` z ustawieniami głosu DALEKa do recytowania wiersza. Między poszczególnymi liniami wstawiana jest pauza 500 milisekund, ponieważ nawet DALEKowie muszą odetchnąć.

Co ciekawe, oryginalne wytyczne związane z poezją zostały napisane przez Davrosa  w języku `FORTRAN <https://en.wikipedia.org/wiki/Fortran>`_ (odpowiedni język DALEKów, gdyż pisany jest TYLKO WIELKIMI LITERAMI). Jednakże Doktor cofnął się w czasie dokładnie do punktu pomiędzy wprowadzeniem 'testów jednostkowych Davrosa <https://en.wikipedia.org/wiki/Unit_testing>`_ .  However, The
Doctor went back in time to precisely the point between Davros's
`unit tests <https://en.wikipedia.org/wiki/Unit_testing>`_
passing and the
`deployment pipeline <https://en.wikipedia.org/wiki/Continuous_delivery>`_
kicking in. At this instant he was able to insert a MicroPython interpreter
into the DALEK operating system and the code you see above into the DALEK
memory banks as a sort of long hidden Time-Lord
`Easter Egg <https://en.wikipedia.org/wiki/Easter_egg_(media)>`_ or
`Rickroll <https://www.youtube.com/watch?v=dQw4w9WgXcQ>`_.

Fonemy
++++++++

Zauważysz, że funkcja ``say`` nie tłumaczy dokładnie słów na odpowiednie dźwięki. Aby mieć lepszą kontrolę nad rezultatem, użyj fonemów: podstawowej jednostki struktury fonologicznej mowy.

Korzyść z używania fonemow jest taka, że nie musisz wiedzieć jak je przeliterować. Musisz raczej wiedzieć jak się dane słowo wymawia, żeby "przeliterować je" fonetycznie. W języku angielskim ma to duże znaczenie.

Pełna lista fonemów rozumianych przez syntezator mowy znajduje się w dokumentacji API dla mowy. Alternatywnie, zaoszczędź sobie dużo czasu wprowadzając angielskie słowa do funkcji ``translate`` (ang. przetłumacz). Zwróci ona pierwsze przybliżenie fonemów, które wykorzysta do wygenerowania audio. Otrzymany rezultat może zostać ręcznie wy-edytowany, żeby poprawić dokładność, fleksję i akcent (tak aby brzmiał bardziej naturalnie).

Funkcja ``pronounce`` (ang. wymowa) jest używana do wyprowadzania fonemu w następujący sposób::

    speech.pronounce("/HEH5EH4EH3EH2EH2EH3EH4EH5EHLP.”)

Jak udoskonaliłbyś kod Doktora, aby używał fonemów?

Zaśpiewaj piosenkę Micro:bit'a
++++++++++++++++++++++++

Poprzez zmianę ustawień ``pitch`` (ang. ton) i wywołanie funkcji ``sing`` (ang. śpiewaj), urządzenie może zacząć śpiewać (jednakże bez szans na wygranie Eurowizji, póki co).

Mapowanie z numerów tonów na nuty jest pokazane poniżej:

.. image:: ../speech-pitch.png

Funkcja ``sing`` musi przyjąć jako dane wejściowe fonemy i tonację::

    speech.sing("#115DOWWWW")

Zwróć uwagę na to w jaki sposób wysokość dźwięków jest podpięta do fonemu za pomocą hash'a (``#``). Tonacja pozostanie taka sama dla kolejnych fonemów do momentu wprowadzenia nowej tonacji.

Poniższy przykład pokazuje jak wszystkie trzy funkcje generujące (``say``,
``pronounce`` oraz ``sing``) mogą zostać wykorzystane do stworzenia czegoś przypominającego mowę.

.. include:: ../../examples/speech.py
    :code: python

.. footer:: Obraz DALEKa jest licencjonowany na zasadach przestawionych poniżej: https://commons.wikimedia.org/wiki/File:Dalek_(Dr_Who).jpg Obraz DAVROSa jest licencjonowany na zasadach przedstawionych poniżej: https://en.wikipedia.org/wiki/File:Davros_and_Daleks.jpg

