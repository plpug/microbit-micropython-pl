Obrazy
------

MicroPython jest tak dobry jak to tylko możliwe, jeśli chodzi o sztukę, jeśli
jedyne czym dysponujesz to macierz 5x5 diód LED (ang. Light Emitting Diodes,
diody emitujące światło na przodzie urządzenia). MicroPython daje sporo kontroli
nad wyświetlaczem, zatem możesz uzyskać sporo ciekawych efektów.

MicroPython posiada wbudowany zestaw obrazów które może pokazać na wyświetlaczu.
Na przykład, żeby pokazać na ekranie szczęśliwą bużkę musimy napisać::

    from microbit import *
    display.show(Image.HAPPY)

Mam nadzieję, że pamiętasz co robi pierwsza linijka powyżej. Druga linia używa
obieku ``display`` i metody ``show`` (ang. pokaż) do wyświetlenia obrazka. Buźka
którą chcemy wyświetlić jest częścią obieku ``Image`` (ang. Obraz) i nazywa się
``HAPPY`` (ang. szczęśliwy). Mówimy metodzie ``show`` by użyła tego obrazka przez
podanie jego nazwy w nawiasach (``(`` i ``)``).

.. image:: happy.png

Poniżej znajdziesz listę wszystkichh wbudowanych obrazów:

    * ``Image.HEART``
    * ``Image.HEART_SMALL``
    * ``Image.HAPPY``
    * ``Image.SMILE``
    * ``Image.SAD``
    * ``Image.CONFUSED``
    * ``Image.ANGRY``
    * ``Image.ASLEEP``
    * ``Image.SURPRISED``
    * ``Image.SILLY``
    * ``Image.FABULOUS``
    * ``Image.MEH``
    * ``Image.YES``
    * ``Image.NO``
    * ``Image.CLOCK12``, ``Image.CLOCK11``, ``Image.CLOCK10``, ``Image.CLOCK9``,
      ``Image.CLOCK8``, ``Image.CLOCK7``, ``Image.CLOCK6``, ``Image.CLOCK5``,
      ``Image.CLOCK4``, ``Image.CLOCK3``, ``Image.CLOCK2``, ``Image.CLOCK1``
    * ``Image.ARROW_N``, ``Image.ARROW_NE``, ``Image.ARROW_E``,
      ``Image.ARROW_SE``, ``Image.ARROW_S``, ``Image.ARROW_SW``,
      ``Image.ARROW_W``, ``Image.ARROW_NW``
    * ``Image.TRIANGLE``
    * ``Image.TRIANGLE_LEFT``
    * ``Image.CHESSBOARD``
    * ``Image.DIAMOND``
    * ``Image.DIAMOND_SMALL``
    * ``Image.SQUARE``
    * ``Image.SQUARE_SMALL``
    * ``Image.RABBIT``
    * ``Image.COW``
    * ``Image.MUSIC_CROTCHET``
    * ``Image.MUSIC_QUAVER``
    * ``Image.MUSIC_QUAVERS``
    * ``Image.PITCHFORK``
    * ``Image.XMAS``
    * ``Image.PACMAN``
    * ``Image.TARGET``
    * ``Image.TSHIRT``
    * ``Image.ROLLERSKATE``
    * ``Image.DUCK``
    * ``Image.HOUSE``
    * ``Image.TORTOISE``
    * ``Image.BUTTERFLY``
    * ``Image.STICKFIGURE``
    * ``Image.GHOST``
    * ``Image.SWORD``
    * ``Image.GIRAFFE``
    * ``Image.SKULL``
    * ``Image.UMBRELLA``
    * ``Image.SNAKE``

Jest ich bardzo dużo! Może zmodyfikujesz kod powyżej żeby zobaczyć jak 
wyglądają pozostałe obrazy? (Wystarczy że zastąpisz ``Image.HAPPY`` jednym
z innych wbudowanych obrazów które są wypisane powyżej.)

Obrazy -- Zrób to sam
++++++++++

Naturalnie, na pewno chcesz stworzyć własny obrazek do wyświetlenia, prawda?

To proste.

Każda dioda LED (dalej nazywana pixelem) na wyjświetlaczu może przyjąć jedną 
z dziesięciu wartości. Jeśli pixel jest ustawiony na ``0`` (zero), to znaczy
że jest wyłączony. Po prostu jasność jest ustawiona na zero, dlatego nie świeci.
Jeśli jednak podamy wartość ``9`` to ustawiomy najwyższy poziom jasności.
Wartości od ``1`` do ``8`` reprezentują poziomy jasności między ``0`` (wyłączony)
do ``9`` (pełna jasność).

Mając powyższe na uwadze, możemy stworzyć własny obrazek w ten sposób:


    from microbit import *

    boat = Image("05050:"
                 "05050:"
                 "05050:"
                 "99999:"
                 "09990")

    display.show(boat)

(Kiedy uruchomisz urządzenie, na wyświetlaczu pokaże się łódka z dwoma masztami,
które będą nieco ciemniejsze od kadłubu.)

Wiesz już jak tworzyć obrazki? Widzisz jak każda linia wyświetlacza jest
reprezentowana przez ciąg znaków kończący się ``:`` (dwukropkiem) i zamknięty
w ``"`` (cudzysłów)? Każda liczba określa jasność. Mamy pięć linii po pięć
liczb, zatem możemy osobno podać jasność każdemu pixelowi na urządzeniu. Tak
właśnie tworzy się własne obrazy.

Proste? Proste!

W zasadzie nie musisz podawach tych wartości w kilku liniach. Jeśli będzie to dla
ciebie czytelne, to możesz wszystkie wartości podać w jednej linii::

    boat = Image("05050:05050:05050:99999:09990")

Animation
+++++++++

Static images are fun, but it's even more fun to make them move. This is also
amazingly simple to do with MicroPython ~ just use a list of images!

Here is a shopping list::

    Eggs
    Bacon
    Tomatoes

Here's how you'd represent this list in Python::

    shopping = ["Eggs", "Bacon", "Tomatoes" ]

I've simply created a list called ``shopping`` and it contains three items.
Python knows it's a list because it's enclosed in square brackets (``[`` and
``]``). Items in the list are separated by a comma (``,``) and in this instance
the items are three strings of characters: ``"Eggs"``, ``"Bacon"`` and
``"Tomatoes"``. We know they are strings of characters because they're enclosed
in quotation marks ``"``.

You can store anything in a list with Python. Here's a list of numbers::

    primes = [2, 3, 5, 7, 11, 13, 17, 19]


.. note::

    Numbers don't need to be quoted since they represent a value (rather than a
    string of characters). It's the difference between ``2`` (the numeric value
    2) and ``"2"`` (the character/digit representing the number 2). Don't worry
    if this doesn't make sense right now. You'll soon get used to it.

It's even possible to store different sorts of things in the same list::

    mixed_up_list = ["hello!", 1.234, Image.HAPPY]

Notice that last item? It was an image!

We can tell MicroPython to animate a list of images. Luckily we have a
couple of lists of images already built in. They're called ``Image.ALL_CLOCKS``
and ``Image.ALL_ARROWS``::

    from microbit import *

    display.show(Image.ALL_CLOCKS, loop=True, delay=100)

As with a single image, we use ``display.show`` to show it on the
device's display. However, we tell MicroPython to use ``Image.ALL_CLOCKS`` and
it understands that it needs to show each image in the list, one after the
other. We also tell MicroPython to keep looping over the list of images (so
the animation lasts forever) by saying ``loop=True``. Furthermore, we tell it
that we want the delay between each image to be only 100 milliseconds (a tenth
of a second) with the argument ``delay=100``.

Can you work out how to animate over the ``Image.ALL_ARROWS`` list? How do you
avoid looping forever (hint: the opposite of ``True`` is ``False`` although
the default value for ``loop`` is ``False``)? Can you change the speed of the
animation?

Finally, here's how to create your own animation. In my example I'm going to
make my boat sink into the bottom of the display::

    from microbit import *

    boat1 = Image("05050:"
                  "05050:"
                  "05050:"
                  "99999:"
                  "09990")

    boat2 = Image("00000:"
                  "05050:"
                  "05050:"
                  "05050:"
                  "99999")

    boat3 = Image("00000:"
                  "00000:"
                  "05050:"
                  "05050:"
                  "05050")

    boat4 = Image("00000:"
                  "00000:"
                  "00000:"
                  "05050:"
                  "05050")

    boat5 = Image("00000:"
                  "00000:"
                  "00000:"
                  "00000:"
                  "05050")

    boat6 = Image("00000:"
                  "00000:"
                  "00000:"
                  "00000:"
                  "00000")

    all_boats = [boat1, boat2, boat3, boat4, boat5, boat6]
    display.show(all_boats, delay=200)

Here's how the code works:

* I create six ``boat`` images in exactly the same way I described above.
* Then, I put them all into a list that I call ``all_boats``.
* Finally, I ask ``display.show`` to animate the list with a delay of 200 milliseconds.
* Since I've not set ``loop=True`` the boat will only sink once (thus making my animation scientifically accurate). :-)

What would you animate? Can you animate special effects? How would you make an
image fade out and then fade in again?
