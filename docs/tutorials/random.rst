Losowość
------

Czasem chcesz pozostawić rzeczy szczęściu, lub odrobinę je pomieszać. Słowem: chcesz, 
by udządzenie działało losowo.

MicroPython zawiera moduł ``random``, wprowadzający element losowy oraz odrobinę
chaosu do twojego kodu. Oto przykładowy kod przewijający na wyświetlaczu losowe imię::

    from microbit import *
    import random

    names = ["Mary", "Yolanda", "Damien", "Alia", "Kushal", "Mei Xiu", "Zoltan" ]

    display.scroll(random.choice(names))

Lista (``names``) zawiera siedem imion zdefiniowanych jako łańcuchy znaków.
Ostatnia linia jest *zagnieżdżona* (the "onion" effect introduced eariler):
metoda ``random.choice`` przyjmuje jako argument listę ``names`` oraz zwraca
jej losowy element. Element ten (lowoso wybrane imię) jest agrumentem dla
``display.scroll``.

Czy możesz zmodyfikować listę tak, by zawierała wybrane przez ciebie imiona?

Losowe Liczby
++++++++++++++

Losowe liczby są bardzo użyteczne. Często używane są w grach. Po cóż innego
mamy kostki?

MicroPython zawiera wiele użtecznych metod powiązanych z losowymi liczbami.
Oto przykład prostej kości do gry::

    from microbit import *
    import random

    display.show(str(random.randint(1, 6)))

Po każdym restarcie urządzenia wyświetla ono numer z zakresu 1-6. Zaczynasz
zapoznawać się z *zagnieżdżaniem*. Warto więc tylko zauważyć, że ``random.randint``
zwraca liczbę całkowitą z przedziału domkniętego pomiędzy dwoma argumentami
(liczba całkowita nazywana jest integer, stąd nazwa metody). Zauważ, że skoro
``display.show`` oczekuje znaku alfabetycznego, używamy funkcji ``str`` do zamiany
numeru na znak (na przykład: ``6`` w ``"6"``).

W wypadku, gdy zawsze potrzebujesz liczny z przedziału pomiędzy ``0`` a ``N``,
używaj metody ``random.randrange``. Gdy podasz jej pojedynczy argument, zwróci
losową liczbę całkowitą z przedziału zamkniętego od ``0`` do ``N`` (w
przeciwieństwie do funkcji ``random.randint``).

Czasami potrzebujesz liczby z miejscem po przecinku. Nazywamy je liczbami
*zmiennoprzecinkowymi* i można je uzyskać używając metody ``random.radom``.
Zwraca ona wartość z przedziału domkniętego od ``0.0`` do ``1.0``. Jeżeli
potrzebujesz dużej liczby zmiennoprzecinkowej dodaj ``random.randrange`` do
``random.random``, jak poniżej::

    from microbit import *
    import random

    answer = random.randrange(100) + random.random()
    display.scroll(str(answer))

Pula Chaosu
++++++++++++++

Losowe liczby generowane przez komputery nie są naprawdę losowe. Zwracają
jedynie pseudolosowe rezultaty zaczynając od wartości początkowej. Wartość
początkowa generowana jest na podstawie pseudolosowej wartości, takiej jak
obecna godzina i/lub odczyty z sensorów sprzętowych.

Czasem chcesz osiągnąć powtarzalne, pseudolosowe zachowanie: odtwarzalne
źródło losowości. To jakby oczekiwać tych samych, losowych wyników pięciu
kolejnych rzutów kością.

Można to łatwo osiągnąć ustalając z góry wartość źródłową. Dla danej wartości
źródłowej generator liczb losowych będzie tworzył tę samą pulę liczb losowych.
Źródło jest ustawiane poprzez ``random.seed`` jako dowolna liczba całkowita.
Poniższa wersja programu do rzutu kością poda zawsze ten sam rezultat::


    from microbit import *
    import random

    random.seed(1337)
    while True:
        if button_a.was_pressed():
            display.show(str(random.randint(1, 6)))

Czy dostrzegasz, czemu powyższy program oczekuje wciśnięcia klawisza A, w
przeciwieństwie do restartu urządzenia z poprzedniego przykładu..?
