Sieć
-------

Możemy połączyć urządzenia w celu wymiany wiadomości między
nimi. Nazywa się to siecią. Sieć połączonych sieci nazywana
jest internetem. Natomiast Internet łączy w sobie wszystkie pozostałe
internety.

Praca w sieci (networking) jest trudna. Można to zaobserwować na
przykładzie opisanego niżej programu. Jednak niewątpliwą zaletą tego projektu
jest to, że zawiera on wszystkie rozpowszechnione aspekty programowania sieciowego
o których powinieneś wiedzieć. W dodatku jest niezwykle prosty i przyjemny w obsłudze.

Ale zacznijmy od początku...

Połączenie
++++++++++

Wyobraźmy sobie sieć jako szereg warstw. Na samym dole mamy najbardziej
fundamentalny aspekt komunikacji: musi być jakiś sposób, żeby przekazać sygnał
z jednego urządzenia do drugiego. Czasem jest to wykonywane poprzez połączenie radiowe,
ale w tym przykładzie zwyczajnie użyjemy dwóch przewodów.

.. image:: network.png

To właśnie na tym fundamencie możemy układać wszystkie pozostałe warstwy
naszej *sieci*.

Jak widzimy na rysunku, niebieskie urządzenie micro:bits jest połączone z czerwonym
za pośrednictwem przewodów krokodylkowych. W obydwu przypadkach wtyk 1 jest wykorzystywany do
sygnału wyjściowego, natomiast wtyk 2 — wejściowego. Wyjście jednego urządzenia jest
połączone z wejściem drugiego. Przypomina to słuchawkę telefonu: na jednym jej końcu
umiejscowiony jest mikrofon (wyjście), na drugim — głośniczek (wejście). Głos zapisany
za pośrednictwem mikrofonu jest odtwarzany przez głośniczek drugiego urządzenia. Gdybyśmy
trzymali słuchawkę odwrotnie, mogłoby z tego wyjść coś dziwnego!

W tym przypadku jest tak samo: konieczne jest poprawne podpięcie przewodów!

Sygnał
++++++

Następną warstwą w naszej *sieci* jest sygnał. Często zależy on od
rodzaju połączenia. W naszym przypadku jest to zwykły sygnał wlączenia i
wyłącznia przekazywany za pośrednictwem wtyków w naszych przewodach.

Jak już wiemy, wtyki wyjścia/wejścia można wykorzystać w następujący sposób::

    pin1.write_digital(1)  # włącz sygnał
    pin1.write_digital(0)  # wyłącz sygnał
    input = pin2.read_digital()  # odczytaj wartość sygnału (1 lub 0)

 Następnym krokiem będzie opisanie sposobu użycia i kontrolowania sygnału. W tym celu będzie
 nam potrzebny...

Protokół
++++++++

Gdybyś kiedyś spotkał się z Królową, musiałbyś sprostać pewnym oczekiwaniom.
Na przykład, po jej przyjeździe musiałbyś się ukłonić, a gdyby podała ci rękę,
należałoby kulturalnie ją uścisnąć. Zwracać się do Królowej przyjęto
per "wasza wysokość", następnie per "proszę pani" itd. Ten zestaw zasad nazywany jest
Królewskim Protokołem. Protokół wyjaśnia jak zachować się w odpowiedniej sytuacji
(np. na spotkaniu z Królową). Protokół jest z góry określony w celu zagwarantowania
jasności sytuacji zanim miałaby ona wystąpić.

.. image:: queen.jpg

Definiujemy i wykorzystujemy protokoły po to, żeby móc przekazywać wiadomości
za pośrednictwem sieci komputerowych. Komputery muszą z góry uzgodnić, jak
wysyłać i przyjmować wiadomości. Prawdopodobnie najbardziej znanym protokołem
jest protokół transmisji hipertekstu (HTTP), wykorzystywany przez sieć www.

Innym znanym protokołem wysyłania wiadomości (który istniał jeszcze przed komputerami)
jest alfabet Morse'a. Określa on sposób wysyłania wiadomości złożonych z liter poprzez
sygnały o krótkim lub długim czasie trwanie. Często takie sygnały są odtwarzane jako
dźwięki. Sygnały o długim czasie trwania nazywane są kreskami ("-"), podczas gdy
te krótkie określane są kropkami ("."). Poprzez połączenie kresek z kropkami Morse
umożliwia wysyłanie liter. Poniżej zamieszczony jest standardowy alfabet Morsa::

    .-    A     ---   J     ...   S     .----  1      ----.  9
    -...  B     -.-   K     -     T     ..---  2      -----  0
    -.-.  C     .-..  L     ..-   U     ...--  3
    -..   D     --    M     ...-  V     ....-  4
    .     E     -.    N     .--   W     .....  5
    ..-.  F     ---   O     -..-  X     -....  6
    --.   G     .--.  P     -.--  Y     --...  7
    ....  H     --.-  Q     --..  Z     ---..  8
    ..    I     .-.   R

Jak wynika z powyższego wykresu, żeby wysłać literę "H", należy czterokrotnie
wysłać krótki sygnał, który odpowiada czterem kropkom ("...."). W przypadku
litery "L" sygnał również jest wysyłany cztery razy, jednak drugie włączenie
trwa dłużej (".-..").

Oczywiście czas trwania sygnału ma znaczenie: musimy odróżnić kropkę od kreski.
Na tym polega kolejny cel protokołu — żeby uzgodnić wspólne dla wszystkich zasady
wdrażania protokołu.
W tym przypadku powiemy tylko, że:

* Sygnał trwający mniej niż 250 milisekund jest kropką.
* Sygnał trwający od 250 do 499 milisekund jest kreską.
* Każdy sygnał o innym czasie trwania będzie ignorowany.
* Przerwa / odstęp między sygnałami, który przekracza 500 milisekund określa koniec litery.

W ten sposób, wysłanie litery "H" określane jest jako cztery sygnały trwające
nie więcej niż 250 milisekund każdy, po których następuje przerwa dłuższa
niż 500 milisekund (określająca koniec litery).

Wiadomość
+++++++

Wreszcie dotarliśmy do chwili, gdy możemy stworzyć wiadomość, która
ma dla ludzi określone znaczenie. Jest to najwyższa warstwa w
naszej *sieci*.

Wykorzystując zdefiniowany wyżej protokół, mogę wysłać za pośrednictwem
przewodu następującą sekwencję sygnałów do innego urządzenia micro:bit::

    ...././.-../.-../---/.--/---/.-./.-../-..

Czy możesz odczytać te wiadomość?

Aplikacja
+++++++++++

Posiadanie sieci jest dobre, jednak potrzebujemy również sposobu, żeby wejść
z nią w interakcję. Można to zrobić z pomocą aplikacji, która wysyła i przyjmuje
wiadomości. HTTP jest ciekawe, jednak *większość* ludzi nie wie o nim, pozostawiając
jego obsługę przeglądarce - sieć leżąca u podstaw sieci ogólnoświatowej
jest ukryta (tak jak być powinno).

A więc, jakiego rodzaju aplikację powinniśmy napisać dla urządzenia BBC micro:bit?
Jak powinna ona działać, patrząc z perspektywy użytkownika?

Żeby wysłać wiadomość, powinniśmy mieć możliwość wprowadzenia kropek i kresek
(w tym celu możemy wykorzystać przycisk A). Żeby zobaczyć wiadomość, którą wysłaliśmy
lub otrzymaliśmy, powinniśmy mieć możliwość odtworzenia jej poprzez przewijanie na
wyświetlaczu (możemy w tym celu wykorzystać przycisk B). Co więcej, jako że jest
to alfabet Morse'a, jeśli mamy podłączony głośniczek, powinniśmy mieć możliwość
odtworzenia sygnałów dźwiękowych w chwili gdy użytkownik wprowadza swoją wiadomość.

Efekt końcowy
++++++++++++++

Poniżej zamieszczony jest program w całej swojej krasie wraz z dużą ilością
komentarzy, żebyś mógł zobaczyć co się dzieje::

    from microbit import *
    import music


    # A lookup table of morse codes and associated characters.
    MORSE_CODE_LOOKUP = {
        ".-": "A",
        "-...": "B",
        "-.-.": "C",
        "-..": "D",
        ".": "E",
        "..-.": "F",
        "--.": "G",
        "....": "H",
        "..": "I",
        ".---": "J",
        "-.-": "K",
        ".-..": "L",
        "--": "M",
        "-.": "N",
        "---": "O",
        ".--.": "P",
        "--.-": "Q",
        ".-.": "R",
        "...": "S",
        "-": "T",
        "..-": "U",
        "...-": "V",
        ".--": "W",
        "-..-": "X",
        "-.--": "Y",
        "--..": "Z",
        ".----": "1",
        "..---": "2",
        "...--": "3",
        "....-": "4",
        ".....": "5",
        "-....": "6",
        "--...": "7",
        "---..": "8",
        "----.": "9",
        "-----": "0"
    }


    def decode(buffer):
        # Attempts to get the buffer of Morse code data from the lookup table. If
        # it's not there, just return a full stop.
        return MORSE_CODE_LOOKUP.get(buffer, '.')


    # How to display a single dot.
    DOT = Image("00000:"
                "00000:"
                "00900:"
                "00000:"
                "00000:")


    # How to display a single dash.
    DASH = Image("00000:"
                 "00000:"
                 "09990:"
                 "00000:"
                 "00000:")


    # To create a DOT you need to hold the button for less than 250ms.
    DOT_THRESHOLD = 250
    # To create a DASH you need to hold the button for less than 500ms.
    DASH_THRESHOLD = 500


    # Holds the incoming Morse signals.
    buffer = ''
    # Holds the translated Morse as characters.
    message = ''
    # The time from which the device has been waiting for the next keypress.
    started_to_wait = running_time()


    # Put the device in a loop to wait for and react to key presses.
    while True:
        # Work out how long the device has been waiting for a keypress.
        waiting = running_time() - started_to_wait
        # Reset the timestamp for the key_down_time.
        key_down_time = None
        # If button_a is held down, then...
        while button_a.is_pressed():
            # Play a beep - this is Morse code y'know ;-)
            music.pitch(880, 10)
            # Set pin1 (output) to "on"
            pin1.write_digital(1)
            # ...and if there's not a key_down_time then set it to now!
            if not key_down_time:
                key_down_time = running_time()
        # Alternatively, if pin2 (input) is getting a signal, pretend it's a
        # button_a key press...
        while pin2.read_digital():
            if not key_down_time:
                key_down_time = running_time()
        # Get the current time and call it key_up_time.
        key_up_time = running_time()
        # Set pin1 (output) to "off"
        pin1.write_digital(0)
        # If there's a key_down_time (created when button_a was first pressed
        # down).
        if key_down_time:
            # ... then work out for how long it was pressed.
            duration = key_up_time - key_down_time
            # If the duration is less than the max length for a "dot" press...
            if duration < DOT_THRESHOLD:
                # ... then add a dot to the buffer containing incoming Morse codes
                # and display a dot on the display.
                buffer += '.'
                display.show(DOT)
            # Else, if the duration is less than the max length for a "dash"
            # press... (but longer than that for a DOT ~ handled above)
            elif duration < DASH_THRESHOLD:
                # ... then add a dash to the buffer and display a dash.
                buffer += '-'
                display.show(DASH)
            # Otherwise, any other sort of keypress duration is ignored (this isn't
            # needed, but added for "understandability").
            else:
                pass
            # The button press has been handled, so reset the time from which the
            # device is starting to wait for a  button press.
            started_to_wait = running_time()
        # Otherwise, there hasn't been a button_a press during this cycle of the
        # loop, so check there's not been a pause to indicate an end of the
        # incoming Morse code character. The pause must be longer than a DASH
        # code's duration.
        elif len(buffer) > 0 and waiting > DASH_THRESHOLD:
            # There is a buffer and it's reached the end of a code so...
            # Decode the incoming buffer.
            character = decode(buffer)
            # Reset the buffer to empty.
            buffer = ''
            # Show the decoded character.
            display.show(character)
            # Add the character to the message.
            message += character
        # Finally, if button_b was pressed while all the above was going on...
        if button_b.was_pressed():
            # ... display the message,
            display.scroll(message)
            # then reset it to empty (ready for a new message).
            message = ''

How would you improve it? Can you change the definition of a dot and a dash so
speedy Morse code users can use it? What happens if both devices are sending at
the same time? What might you do to handle this situation?

.. footer:: The image of Queen Elizabeth II is licensed as per the details here: https://commons.wikimedia.org/wiki/File:Queen_Elizabeth_II_March_2015.jpg
