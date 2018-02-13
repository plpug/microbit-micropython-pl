Kierunek
---------

Na urządzeniu BBC micro:bit znajduje się kompas. Jeżeli kiedykolwiek zrobisz
stację pogodową, możesz użyć micro:bita do sprawdzania kierunku wiatru.

Kompas
+++++++

Urządzenie może również wskazać kierunek północny::

    from microbit import *

    compass.calibrate()

    while True:
        needle = ((15 - compass.heading()) // 30) % 12
        display.show(Image.ALL_CLOCKS[needle])

.. notatka:: 

    **Przed dokonaniem pomiaru należy skalibrować urządzenie.** W innym wypadku
    rezultaty będą niepoprawne. Aby urządzenie zorientowało się w swoim ustawieniu
    względem pola magnetycznego Ziemi, metoda ``calibration`` uruchamia małą,
    śmieszną grę. 

    Aby skalibrować kompas, obracaj micro:bitem, dopóki krańcowe piksele
    wyświetlacza nie utworzą koła.

Program używa ``compass.heading`` i, wraz z prostą, acz piękną matematyką,
`floor division <https://en.wikipedia.org/wiki/Floor_and_ceiling_functions>`_ ``//`` oraz `modulo <https://en.wikipedia.org/wiki/Modulo_operation>`_ ``%``, wyprowadza pozycję wskazówki zegara, po czym wyświetla ją tak,
by pokazywała mniej więcej północ.
