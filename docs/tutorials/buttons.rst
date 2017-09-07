Buttons
-------

Jak dotychczas utworzyliśmy kod umożliwiający urządzeniu pokazywanie czegoś.
Nazywa się to *wyjście*. Musimy też jednak umożliwić urządzeniu reagowanie na
zdarzenia. Nazywa się to *wejście*.

Łatwo zapamiętać: wyjście dotyczy wszystkiego co wychodzi z urządzenia,
natomiast wejście to wszystko co przychodzi do urządzenia w celu
przetworzenia.

Najbardziej oczywiste jest to, że wejściem będą dwa przyciski na mikrobicie,
które oznaczone są jako 'A' i 'B'. Potrzebujemy aby MicroPython jakoś
zareagował na naciśnięcie przycisku.

To jest fenomenalnie proste:

    from microbit import *

    sleep(10000)
    display.scroll(str(button_a.get_presses()))

Cały skrypt jest uśpiony przez 10 000 milisekund (czyli 10 sekund) a po tym
czasie na wyświetlaczu przewinie się liczba ilości wciśnięć przycisku 'A'.
To wszystko.

Chociaż ten skrypt jest bezużyteczny, to jednak pokazuje name kilka ciekawych
pomysłów:

#. *Funkcja* ``sleep`` usypia micro:bit na pewną ilość milisekund. Jeżeli
   chcesz zatrzymać program, to jest sposób w jaki możesz to zrobić.
   *Funkcja* jest podobna do *metody* ale nie jest przyporządkowana do 
   *obiektu* za pomocą kropki.

#. Istnieje obiekt nazwany ``button_a`` i on pozwala ci pobrać liczbę
   określającą ilości naciśnięć za pomocą *metody* ``get_presses``.

*Metoda* ``get_presses`` daje nam tylko wartość, a ``display.scroll`` tylko
wyświetla znaki, więc potrzebujemy jeszcze przetworzyć wartość numeryczną na
znaki. Robimy to za pomocą funkcji ``str`` (skrót od ang "string" ~ przetwarza
wszystko na tekst).

Trzeci wiersz jest jak cebula. Jeżeli przyjmiesz nawiasy za warstwy cebuli, to
zauważysz, że ``display.scroll`` zawiera ``str``, który zawiera 
``button_a.get_presses``. Python próbuje rozpracować najbardziej wewnętrzne
odpowiedzi najpierw, zanim przejdzie do następnej warstwy. To nazywa się
*zagnieżdżenie* - programistyczny odpowiednik rosyjskiej lalki - matrioszka.

.. image:: matrioshka.jpg

Przyjmijmy, że nacisnąłeś przycisk 10 razy. Oto jak działa Python, to dzieje
się w trzeciej linii:

Python widzi pełną linię i dostaje wartość ``get_presses``::

    display.scroll(str(button_a.get_presses()))

Teraz, gdy Python wie już ile razy został wciśnięty przycisk, to przetworzy
wartość numeryczną na tekst::

    display.scroll(str(10))

No i w końcu Python wie co wyświetlić przewijając na ekranie::

    display.scroll("10")

Może ci się wydawać, że to jest mnóstwo pracy ale MicroPython robi to 
niezwykle szybko.

Pętle zdarzeń
+++++++++++

Często potrzebujesz aby program poczekał na coś, co się wydarzy. Aby uzyskać
to musisz zrobić pętlę dla jakiegoś kawałka kodu, który będzie miał 
zdefiniowane w jaki sposób zareagować na oczekiwaną reakcję taką, jak
naciśnięcie przycisku.

Aby utworzyć pętle w Pythonie użyj słowa kluczowego ``while``. To sprawdza czy
coś jest ``True``. Jeżeli tak jest, to uruchamia *blok kodu* zwany *zawartością*
pętli. Jeżeli jednak nie jest, to przerywa wykonywanie pętli (ignorując zawartość),
a wtedy reszta część programu może wykonać się.

Python ułatwia definiowanie bloków kodu. Powiedz, że mam listę rzeczy do
zrobienia napisaną na kartcie papieru. Pewnie będzie wyglądało to tak::

    Zakupy
    Napraw uszkodzoną rynnę
    Skoś trawnik

Gdybym chciał nieco rozbić moją listę rzeczy do zrobienia, to mógłbym napisać
coś takiego::

    Zakupy:
        Jaja
        Boczek
        Pomidory
    Napraw uszkodzoną rynnę:
        Wyjmij drabinę z sąsiednich drzwi
        Znajdź młotek i gwoździe
        Obróć grabinę
    Skoś trawnik:
        Sprawdź trawnik wokół sadzawki dla żab
        Sprawdź poziom paliwa w kosiarce

It's obvious that the main tasks are broken down into sub-tasks that are
*indented* underneath the main task to which they are related. So ``Eggs``,
``Bacon`` and ``Tomatoes`` are obviously related to ``Shopping``. By indenting
things we make it easy to see, at a glance, how the tasks relate to each other.

This is called *nesting*. We use nesting to define blocks of code like this::

    from microbit import *

    while running_time() < 10000:
        display.show(Image.ASLEEP)

    display.show(Image.SURPRISED)

The ``running_time`` function returns the number of milliseconds since the
device started.

The ``while running_time() < 10000:`` line checks if the running time is less
than 10000 milliseconds (i.e. 10 seconds). If it is, *and this is where we can
see scoping in action*, then it'll display ``Image.ASLEEP``. Notice how this is
indented underneath the ``while`` statement *just like in our to-do list*.

Obviously, if the running time is equal to or greater than 10000 milliseconds
then the display will show ``Image.SURPRISED``. Why? Because the ``while``
condition will be False (``running_time`` is no longer ``< 10000``). In that
case the loop is finished and the program will continue after the ``while``
loop's block of code. It'll look like your device is asleep for 10
seconds before waking up with a surprised look on its face.

Try it!

Handling an Event
+++++++++++++++++

If we want MicroPython to react to button press events we should put it into
an infinite loop and check if the button ``is_pressed``.

An infinite loop is easy::

    while True:
        # Do stuff

(Remember, ``while`` checks if something is ``True`` to work out if it should
run its block of code. Since ``True`` is obviously ``True`` for all time, you
get an infinite loop!)

Let's make a very simple cyber-pet. It's always sad unless you're pressing
button ``A``. If you press button ``B`` it dies. (I realise this isn't a very
pleasant game, so perhaps you can figure out how to improve it.)::

    from microbit import *

    while True:
        if button_a.is_pressed():
            display.show(Image.HAPPY)
        elif button_b.is_pressed():
            break
        else:
            display.show(Image.SAD)

    display.clear()

Can you see how we check what buttons are pressed? We used ``if``,
``elif`` (short for "else if") and ``else``. These are called *conditionals*
and work like this::

    if something is True:
        # do one thing
    elif some other thing is True:
        # do another thing
    else:
        # do yet another thing.

This is remarkably similar to English!

The ``is_pressed`` method only produces two results: ``True`` or ``False``.
If you're pressing the button it returns ``True``, otherwise it returns
``False``. The code above is saying, in English, "for ever and ever, if
button A is pressed then show a happy face, else if button B is pressed break
out of the loop, otherwise display a sad face." We break out of the loop (stop
the program running for ever and ever) with the ``break`` statement.

At the very end, when the cyber-pet is dead, we ``clear`` the display.

Can you think of ways to make this game less tragic? How would you check if
*both* buttons are pressed? (Hint: Python has ``and``, ``or`` and ``not``
logical operators to help check multiple truth statements (things that
produce either ``True`` or ``False`` results).

.. footer:: The image of Matrioshka dolls is licensed CC BY-SA 3.0, https://commons.wikimedia.org/w/index.php?curid=69402
