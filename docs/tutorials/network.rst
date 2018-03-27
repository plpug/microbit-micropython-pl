Sieć
----

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
za pośrednictwem krokodylków. W obydwu przypadkach wtyk 1 jest wykorzystywany do
sygnału wyjściowego, natomiast wtyk 2 — wejściowego. Wyjście jednego urządzenia jest
połączone z wejściem drugiego. Przypomina to słuchawkę telefonu: na jednym jej końcu
umiejscowiony jest mikrofon (wyjście), na drugim — głośniczek (wejście). Głos zapisany
za pośrednictwem mikrofonu jest odtwarzany przez głośniczek drugiego urządzenia. Gdybyśmy
trzymali słuchawkę odwrotnie, mogłoby z tego wyjść coś dziwnego!

W tym przypadku jest tak samo: konieczne jest poprawne podpięcie przewodów!

Sygnał
++++++

Następną warstwą w naszej *sieci* jest sygnał. Często zależy on od
rodzaju połączenia. W naszym przypadku jest to zwykły sygnał włączenia i
wyłączenia przekazywany za pośrednictwem wtyków w naszych przewodach.

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
+++++++++

Wreszcie dotarliśmy do chwili, gdy możemy stworzyć wiadomość, która
ma dla ludzi określone znaczenie. Jest to najwyższa warstwa w
naszej *sieci*.

Wykorzystując zdefiniowany wyżej protokół, mogę wysłać za pośrednictwem
przewodu następującą sekwencję sygnałów do innego urządzenia micro:bit::

    ...././.-../.-../---/.--/---/.-./.-../-..

Czy możesz odczytać te wiadomość?

Aplikacja
+++++++++

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
+++++++++++++

Poniżej zamieszczony jest program w całej swojej krasie wraz z dużą ilością
komentarzy, żebyś mógł zobaczyć co się dzieje::

    from microbit import *
    import music


    # tabela kodów alfabetu Morse'a i odpowiednio do każdego z nich przypisanego znaku.
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
        # Próba pobrania kodu Morse'a z tabeli znaków. Jeżeli
        # go tam nie ma, po prostu zwróć kropkę.
        return MORSE_CODE_LOOKUP.get(buffer, '.')


    # Jak wyświetlić pojedynczą kropkę.
    DOT = Image("00000:"
                "00000:"
                "00900:"
                "00000:"
                "00000:")


    # Jak wyświetlić pojedynczą kreskę.
    DASH = Image("00000:"
                 "00000:"
                 "09990:"
                 "00000:"
                 "00000:")


    # Aby utworzyć KROPKĘ musisz przytrzymać przycisk krócej niż 250 milisekund.
    DOT_THRESHOLD = 250
    # Aby utworzyć KRESKĘ musisz przytrzymać przycisk krócej niż 500 milisekund.
    DASH_THRESHOLD = 500


    # Trzyma przychodzący sygnał Morse'a.
    buffer = ''
    # Trzyma przetłumaczony sygnał Morse'a na znaki.
    message = ''
    # Czas od kiedy urządzenie czeka na naciśnięcie następnego klawisza.
    started_to_wait = running_time()


    # Ustaw urządzenie w pętli, aby czekało i zareagowało na naciśnięcie.
    while True:
        # Oblicz jak długo urządzenie czekało na naciśnięcie.
        waiting = running_time() - started_to_wait
        # Zresetuj czas dla key_down_time.
        key_down_time = None
        # Jeżeli button_a jest przytrzymany, to wtedy...
        while button_a.is_pressed():
            # Zabrzęcz - to jest alfabet Morse'a, rozumiesz ;-)
            music.pitch(880, 10)
            # Ustaw pin1 (wyjście) na "on" (ang. włączony)
            pin1.write_digital(1)
            # ... i jeżeli key_down_time jest puste, to wtedy ustaw go na teraz!
            if not key_down_time:
                key_down_time = running_time()
        # Alternatywnie, jeżeli pin2 (wejście) jest otrzymuje sygnał, zasymuluj
        # naciśnięcie klawisza button_a...
        while pin2.read_digital():
            if not key_down_time:
                key_down_time = running_time()
        # Pobierz aktualny czas i nazwij go key_up_time.
        key_up_time = running_time()
        # Ustaw pin1 (wyjście) na "off" (ang. wyłączony)
        pin1.write_digital(0)
        # Jeżeli key_down_time jest ustawiona (utworzona gdy button_a został
        # naciśnięty pierwszy raz).
        if key_down_time:
            # ... potem oblicz jak długo był wciśnięty.
            duration = key_up_time - key_down_time
            # Jeżeli duration (ang. czas trwania) jest mniejszy niż maksymalna długość
            # dla naciśnięcia "dot"... (ang. kropka)
            if duration < DOT_THRESHOLD:
                # ... wtedy dodaj kropkę do buffer (ang. bufor) zawierającego
                # przychodzący kod Morse'a i wyświetl kropkę na wyświetlaczu.
                buffer += '.'
                display.show(DOT)
            # W przeciwnym wypadku, ale jeżeli czas trwania jest mniejszy niż maksymalna
            # długość dla naciśnięcia "dash"... (ang. kreska) (ale dłuższy niż dla
            # KROPKI ~ określonej powyżej)
            elif duration < DASH_THRESHOLD:
                # ... wtedy dodaj kreskę do bufora i wyświetl kreskę.
                buffer += '-'
                display.show(DASH)
            # W pozostałych przypadkach, każda inna długość naciśnięcia jest ignorowana
            # (to nie jest konieczne, ale zostało dodane dla "zrozumiałości").
            else:
                pass
            # Obsługa przycisku została zakończona, więc zresetuj czas od którego
            # urządzenie rozpoczyna czekać na naciśnięcie przycisku.
            started_to_wait = running_time()
        # W przeciwnym razie button_a nie został naciśnięty podczas tego cyklu pętli,
        # więc sprawdź czy to nie przerwa wskazująca na koniec przychodzącego kodu
        # Morse'a dla znaku. Przerwa musi być dłuższa niż czas dla KRESKI (DASH)
        elif len(buffer) > 0 and waiting > DASH_THRESHOLD:
            # Mamy bufor i przetwarzanie dobiegło końca tak więc...
            # Rozkoduj zawartość bufora.
            character = decode(buffer)
            # Opróżnij bufor do czysta.
            buffer = ''
            # Pokaż odkodowane znaki.
            display.show(character)
            # Dodaj znaki do wiadomości.
            message += character
        # Ostatecznie, jeżeli button_b został naciśnięty podczas trwania powyższego...
        if button_b.was_pressed():
            # ... wyświetl wiadomość,
            display.scroll(message)
            # potem opróżnij do czysta (gotowy na nową wiadomość).
            message = ''

Jak chciałbyś ulepszyć to? Czy możesz zmienić definicje kropki i kreski tak by
biegły użytkownik alfabetu Morse'a mógł użyć tego? Co się stanie gdy oba urządzenia
będą wysyłać w tym samym czasie? Co możesz zrobić, aby poradzić sobie z tą sytuacją.

.. footer:: Obraz Królowej Elżbiety II został użyty zgodnie z licencją określoną na stronie: https://commons.wikimedia.org/wiki/File:Queen_Elizabeth_II_March_2015.jpg
