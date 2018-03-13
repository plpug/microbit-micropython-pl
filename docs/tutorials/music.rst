Muzyka
-----
MicroPython na BBC micro:bit zawiera bogaty w możliwości moduł muzyka i dźwięk. Dzięki niemu można w prosty sposób zaprogramować BBC micro:bit do wygenerowania 
dźwięków o wysokim lub niskim tonie. Oczywiście, aby cos usłyszeć trzeba *podłączyć głośniki* - za pomocą krokodylków podłącz pin 0 oraz GND do wejść głośnika. Nie ma znaczenia który zacisk do którego wejścia zostanie podłączony.

.. image:: pin0-gnd.png

.. note::

     Do tego celu nie używaj przetwornika pizoelektrycznego, który może generować
    tylko pojedynczy ton.

Zagrajmy coś!::
    import music

    music.play(music.NYAN)

Zauważ, że importujemy moduł ``music``. Zawiera on polecenia wykorzystywane do
wytwarzania i zarządzania dźwiękiem.

MicroPython ma dość dużo wbudowanych melodii. Oto ich kompletna lista:

* ``music.DADADADUM``
* ``music.ENTERTAINER``
* ``music.PRELUDE``
* ``music.ODE``
* ``music.NYAN``
* ``music.RINGTONE``
* ``music.FUNK``
* ``music.BLUES``
* ``music.BIRTHDAY``
* ``music.WEDDING``
* ``music.FUNERAL``
* ``music.PUNCHLINE``
* ``music.PYTHON``
* ``music.BADDY``
* ``music.CHASE``
* ``music.BA_DING``
* ``music.WAWAWAWAA``
* ``music.JUMP_UP``
* ``music.JUMP_DOWN``
* ``music.POWER_UP``
* ``music.POWER_DOWN``

Pobierz któryś z przykładowych kodów i odtwórz melodię. Która podoba Ci się najbardziej? Masz pomysł na ich wykorzystanie jako sygnały?

Wolfgang Amadeusz Microbit
+++++++++++++++++++++++++

Zaprogramowanie swojej własnej melodii jest bardzo łatwe!

Każda nuta ma nazwę (np. ``C#`` lub ``F``), oktawę (określającą jak wysoko lub nisko dźwięk ma być grany) oraz długość (przez jaki czas dźwięk ma być grany). Oktawy są oznaczone liczbami ~ 0 oznacza najniższą oktawę, oktawa 4 zawiera środkowe C, a 8 jest tak wysoko, że wyższej już nie będziesz potrzebować, chyba że zechcesz tworzyć muzykę dla psów. Długość dźwięku jest również wyrażana jako liczby. Im większa liczba tym dłużej nuta będzie odtwarzana. Te wielkości są od siebie zależne, np. po wprowadzeniu wartości ``4``, nuta będzie odtwarzana 2 razy dłużej niż po wprowadzeniu ``2`` itd. W miejsce nazwy nuty możesz wpisać ``R``. Wtedy MicroPython nie będzie odtwarzał nut przez ustalony czas.

Każda nuta opisywana jest jako ciąg znaków::

    NOTE[octave][:duration]
czyli 
	NUTA[oktawa][:długość]
	
Na przykład, ``"A1:4"`` opisuje nutę ``A`` w oktawie numer ``1`` o długości
``4``.

Utwórz listę nut, które stworzą melodię. Poniżej znajduje się zapis, dzięki 
któremu MicroPython zagra początek melodii "Panie Janie"::

    import music

    tune = ["C4:4", "D4:4", "E4:4", "C4:4", "C4:4", "D4:4", "E4:4", "C4:4",
            "E4:4", "F4:4", "G4:8", "E4:4", "F4:4", "G4:8"]
    music.play(tune)

.. note::

    MicroPython umożliwia zapisywanie takich melodii w uproszczony sposób. Zapamięta oktawę, długość nuty i będzie je wykorzystywał aż do 	wprowadzenia nowych wartości. Dzięki temu powyższy przykład może zostać zapisany tak:
		
        import music

        tune = ["C4:4", "D", "E", "C", "C", "D", "E", "C", "E", "F", "G:8",
 
               "E:4", "F", "G:8"]
        music.play(tune)

    Zauważ że wartości określające oktawę i czas trwania są wpisywane tylko wtedy, kiedy sa zmieniane. Dzięki temu kod jest łatwiejszy zarówno do napisania jak i do odczytania.

Efekty dźwiękowe
+++++++++++++

MiroPython umożliwia tworzenie melodii i dźwięków zapisanych inaczej niż nutami. 
Na przykład dzięki poniższemu kodowi możemy uzyskać efekt syreny policyjnej::

    import music

    while True:
        for freq in range(880, 1760, 16):
            music.pitch(freq, 6)
        for freq in range(1760, 880, -16):
            music.pitch(freq, 6)


Zwróć uwagę na to jak polecenie ``music.pitch`` jest w tym przypadku użyte.
Wymaga podania częstotliwości. Np. częstotliwość o wartości ``440`` jest podobna do częstotliwości jakiegoś koncertu, natomiast ``A`` to częstotliwość odpowiadająca orkiestrze symfonicznej.

W powyższym przykładzie funkcja ``range`` (ang. zakres) jest wykorzystana do  ustalenia zakresu wartości liczbowych. W powyższym przykładzie liczby te definiują wysokość tonu, po kolei: wartość początkową oraz wartość końcową. Jednak interesuje nas tylko co któraś liczba. O tym informuje ostatnia wartość. Tak więc pierwsza funkcja mówi: "dany zbiór ma zawierać co 16-tą liczbę w zakresie od 880 do 1760". Drugi zapisa mówi: "zbiór ma zawierać liczby od 1760 do 880 zmniejszające się o 16". W ten sposób uzyskamy zakres częstotliwości, które stopniowo zwiększając się i zmniejszając tworzą dźwięk przypominający syrenę.

Ponieważ dźwięk syreny powinien trwać nieskończenie długo, jest on wpisany
w niekończącą się pętlę ``while``.

Co ważne, wprowadziliśmy nowy rodzaj pętli wewnątrz pętli "while" (dopóki): pętlę "for" (dla). Opisując je powiedzielibyśmy: "dla każdego elementu w pewnym zbiorze, zrób z nią coś określonego". W kontekście poprzedniego przykładu: " dla każdej częstotliwości w ustalonym zakresie częstotliwości, graj ton o tej częstotliwości przez 6 milisekund". Zauważ wcięcia przy opisie każdej z czynności, które wykonujemy dla danego elementu (mówiliśmy o tym wcześniej). Dzięku temu Python dokładnie wie jaki fragment kodu uruchomić przy danych elementach.


