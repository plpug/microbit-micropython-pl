Przyciski
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
znaki. Robimy to za pomocą funkcji ``str`` (skrót od angielskiego słowa
"string" ~ przetwarza wszystko na ciąg znaków).

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
wartość numeryczną na ciąg znaków::

    display.scroll(str(10))

No i w końcu Python wie, że ma wyświetlić przesuwający się tekst na ekranie::

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
zrobienia napisaną na kartce papieru. Pewnie będzie wyglądało to tak::

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

To oczywiste, że główne zadania są podzielone na podrzędne zadania, które
zaczynają się od *akapitu* (wcięcie) (ang. indent) poniżej głównego zadania, do którego
są powiązane. Tak więc ``Jaja``, ``Boczek`` i ``Pomidory`` są oczywiście
związane z zadaniem ``Zakupy``. Wcięcia ułatwiają nam sprawdzanie w jaki sposób
zadania są powiązane ze sobą.

To nazywane jest *zagnieżdżeniem*. Używamy zagnieżdżeń do definicji bloków
kodu takich jak::

    from microbit import *

    while running_time() < 10000:
        display.show(Image.ASLEEP)

    display.show(Image.SURPRISED)

Funkcja ``running_time`` zwraca liczbę milisekund od startu urządzenia.

Linia ``while running_time() < 10000:`` sprawdza czy czas pracy urządzenia
jest mniejszy od 10000 milisekund (czyli 10 sekund). Jeżeli tak, *i to jest
miejsce gdzie możemy zobaczyć skalę działania*, to zostanie wyświetlony
``Image.ASLEEP``. Zauważ jak to jest wcięte poniżej instrukcji ``while``
*tak jak na naszej liście zadań*.

Oczywiście, jeśli czas pracy jest równy lub większy niż 10000 milisekund,
wówczas na ekranie pojawi się ``Image.SURPRISED``. Dlaczego? Ponieważ warunek
``while`` będzie fałszywy (ang. False) (``running_time`` nie jest już ``< 10000``).
W takim przypadku pętla jest zakończona i program będzie kontynuowany po bloku
kodu pętli ``while``. Wygląda na to, że twoje urządzenie śpi przez 10 sekund
zanim obudzi się z zaskoczoną miną na swojej twarzy.

Wypróbuj to!

Ogsługa zdarzenia
+++++++++++++++++

Jeśli chcemy aby MicroPython reagował na zdarzenia naciśnięcia przycisku, to
powinniśmy umieścić go w nieskończonej pętli i sprawcać, czy przycisk
``is_pressed``.

Nieskończona pętla jest prosta::

    while True:
        # rób coś

(Pamiętaj, że ``while`` sprawdza czy coś jest ``True`` przed każdym wykonaniem
bloku kodu. Ponieważ ``True`` jest oczywiście ``True`` przez czały czas, to
otrzymujesz nieskończoną pętlę!)

Zróbmy bardzo prosty cyber-zwierzak. Jest on smutny, dopóki nie naciśniesz
przycisku ``A``. Ale jeżeli naciśniesz przycisk ``B`` on umrze. (Zdaję sobię
sprawę, że to nie jest zbyt przyjemna gra, więc może masz pomysł jak ją
ulepszyć.)::

    from microbit import *

    while True:
        if button_a.is_pressed():
            display.show(Image.HAPPY)
        elif button_b.is_pressed():
            break
        else:
            display.show(Image.SAD)

    display.clear()

Czy widzisz jak sprawdzamy, jakie przyciski są wciśnięte? Użyliśmy ``if``,
``elif`` (skrót od "else if") oraz ``else``. Są one nazywane *warunkami* i
działają tak::

    if coś jest True:
        # zrób pierwszą rzecz
    elif coś innego jest True:
        # zrób następną rzecz
    else:
        # zrób jeszcze coś.

To jest podobne do języka angielskiego!

Metoda ``is_pressed`` generuje tylko dwa rezultaty: ``True`` albo ``False``.
Jeżeli naciśniesz przycisk, to zwróci ``True``, w przeciwnym wypadku zwróci
``False``. Powyższy kod można przetłumaczyć jako "jeżeli i tylko wtedy gdy
przycisk A został naciśnięty, to pokaż szczęśliwą twarz, jeżeli przycisk B
został naciśnięty, to przerwij pętlę, a w przeciwnym wypadku pokaż smutną
minę." Przerywamy wykonywanie pętli (zatrzymujemy uruchomiony w nieskończoność
program) za pomocą instrukcji ``braek``.

Na samym końcu, kiedy cyber-zwierzak nie żyje, wyczyśćmy ekran metodą
``clear``.

Czy możesz pomyśleć o sposobie, aby ta gra była mniej tragiczna? Jak chciałbyś
sprawdzić, czy przyciski *both* zostały naciśnięte? (Podpowiedź: Python ma
logiczne operatory  ``and``, ``or`` i ``not``, które umożliwiają sprawdzenie
wielu wyrażeń warunkowych (rzeczy, które generują rezultaty ``True`` albo
``False``).

.. footer:: The image of Matrioshka dolls is licensed CC BY-SA 3.0, https://commons.wikimedia.org/w/index.php?curid=69402
