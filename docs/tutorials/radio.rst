Radio
-----

Komunikacja na odległość jest jak magia. 

Magia może być użyteczna jeśli jesteś elfem, czarodziejem lub jednorożcem, ale oni istnieją tylko w powieściach.

Jednakże jest coś znacznie lepszego od magii: fizyka!

Bezprzewodowa interakcja to sama fizyka: fale radiowe (rodzaj promieniowania elektromagnetycznego, 
podobnego do światła widzialnego) mają pewną właściwość (tak jak ich amplituda, faza lub długość)
modulowaną przez nadajnik w taki sposób, że można w niej zakodować wiadomość, i dzięki temu ja wysłać. 
Kiedy fale radiowe natrafią na przewodnik elektryczny (np. antenę) wzbudzają w niej prąd zmienny, 
z którego można wyciągnąć informacje zawarte w falach radiowych i przekształcić je do oryginalnej postaci. 

Warstwy na warstwach
++++++++++++++++++++

Jak pamiętasz, sieci są tworzone w warstwach. 

Podstawowym wymaganiem dla sieci jest taki rodzaj połączenia, który pozwoli by sygnał dotarł
z jednego urządzenia do drugiego. W naszym samouczku dotyczącym sieci używaliśmy przewodów 
podłączonych do pinów wejście/wyjście (I/O). Przy pomocy radia możemy pracować bez przewodów, 
wykorzystując opisane powyżej zjawisko do niewidzialnego połączenia urządzeń. 

Następna warstwa na stosie sieciowym jest również inna od przykładu z wprowadzenia do sieci. 
W przykładzie z przewodami stosowaliśmy cyfrowe włącz i wyłącz do wysłania i odebrania sygnału z pinów. 
Z radiem wbudowanym w micro:bit najmniejsza użyteczna cząstka sygnału to bajt.

Bajty
+++++

Bajt jest jednostką informacji, która składa się (najczęściej) z ośmiu bitów. 
Bit jest najmniejszą możliwą jednostką informacji ponieważ może przyjmować 
tylko dwa stany włączony (1) i wyłączony (0).

Bajty zachowują się jak liczydło: każda pozycja w bajcie jest jak kolumna liczydła -- razem 
reprezentują one przypisany numer. W liczydle są to najczęściej tysiące, setki, dziesiątki i jednostki. 
W bajtach są to 128, 64, 32, 16, 8, 4, 2 oraz 1. Jako bity (sygnały włączony/wyłączony)
przesyłane są one w powietrzu i składane ponownie w bajty w odbiorniku. 

Czy zauważyłeś jakąś prawidłowość (podpowiedź: podstawa 2.)

Dodając numery związane z pozycją w bajcie, które są w pozycji "włączony" możemy wyrazić liczby od 0 do 255. 
Poniższy obrazek pokazuje jak to się odbywa dla pierwszych pięciu bitów licząc od 0 do 32:

.. image:: binary_count.gif

Jeżeli przyjmiemy co reprezentuje każdy z 255 numerów (zakodowanych w bajcie) - jak np. literę lub znak - wtedy możemy zacząć przesyłać tekst bajt po bajcie. 
	
Co ciekawe, ktoś już o `tym pomyślał <https://en.wikipedia.org/wiki/ASCII>`. Wykorzystywanie
bajtów do kodowania informacji jest obecnie powszechne. Przypomina to trochę 
porozumiewanie się alfabetem Morse'a o, którym pisaliśmy w dziale sieć.

Świetnym wytłumaczeniem przyjaznym dla uczniów (i nauczycieli) jest seria na stronie `CS unplugged <http://csunplugged.org/binary-numbers/>`

Adresowanie
+++++++++++

Problemem radia jest to, że nie możesz transmitować bezpośrednio do jednej osoby. 
Każdy posiadający odpowiednią antenę może odebrać twoją wiadomość. W związku z tym 
trzeba rozróżniać to, kto powinien odbierać transmisję. 

Sposób, w jaki radio wbudowane w micro:bit, rozwiązuje ten problem jest całkiem prosty:

* Radio można dostroić na różne kanały. Działa to dokładnie tak jak w krótkofalówkach (walkie-talkie): wszyscy ustawiają jeden wspólny kanał i wszyscy słyszą wiadomości innych nadających na tym kanale. Tak jak w walkie-talkie używanie sąsiadujących kanałów może powodować zakłócenia. 

* Moduł radia pozwala na sprecyzowanie dwóch informacji: adresu i grupy. Adres jest jak adres pocztowy a grupa jest jak nazwisko adresata w pod danym adresem. Trzeba pamiętać, że radio odfiltruje z otrzymanych wiadomości te które nie "pasują" do Twojego adresu i grupy. Dlatego trzeba pamiętać o ustawieniu odpowiedniego adresu w aplikacji którą chcesz używać. 

W rzeczywistości micro:bit wciąż odbiera transmisję do innych adresów/grup. Ważną rzeczą jest to,
że nie musisz zajmować się samodzielnie filtrowaniem wiadomości. Niemniej, gdyby ktoś był 
odpowiednio sprytny, mógłby podsłuchiwać wszystkie transmisje, niezależnie od tego do jakiego 
adresu/grupy były przeznaczone. Z tego powodu ważne jest by korzystać z szyfrowanych komunikacji,
w taki sposób by tylko pożądany odbiorca mógł odczytać przesłaną wiadomość. Kryptografia jest 
fascynującym tematem, ale niestety, poza zakresem naszej instrukcji. 
	

Świetliki
+++++++++

Oto świetlik: 

.. image:: firefly.gif

Jest to rodzaj owada, który wykorzystuje bioluminescencję do komunikacji (bez przewodów)
ze swoimi przyjaciółmi. Oto jak wyglądają kiedy nadają do siebie nawzajem:

.. image:: fireflies.gif

Na stronie BBC dostępne jest też piękne wideo świetlików <http://www.bbc.com/earth/story/20160224-worlds-largest-gathering-of-synchronised-fireflies>.

Wykorzystamy moduł radia do czegoś na kształt roju świetlików nadających do siebie.

Najpierw wykonaj komendę ``import radio`` by udostępnić funkcje w Twoim programie. 
Następnie wywołaj funkcję ``radio.on()`` by włączyć radio. W związku z tym, że radio
pobiera prąd i zajmuje pamięć, zrobiliśmy tak byś Ty mógł decydować kiedy 
jest włączone (oczywiście istnieje też funkcja ``radio.off()``).

W tym momencie moduł radia jest skonfigurowany do sensownych ustawień domyślnych,
które sprawiają, że jest kompatybilny z innymi platformami współpracującymi z BBC micro:bit.
Jest możliwe kontrolowanie wielu parametrów wymienionych wcześniej (takich jak kanał i adres),
jak również moc wykorzystywaną do nadawania i ilość pamięci RAM jaką zajmie kolejka wiadomości
przychodzących. Dokumentacja API zawiera wszystkie informacje potrzebne do skonfigurowania radia
tak, by spełniało Twoje potrzeby. 


Zakładając, że jesteśmy zadowoleni z ustawień domyślnych, najprościej można wysłać wiadomość tak:

	radio.send("wiadomość")
		
Ten przykład korzysta z funkcji ``send`` by zwyczajnie nadać ciąg znaków "wiadomość". Odbieranie wiadomości jest jeszcze prostsze:

	new_message = radio.receive()
	
Odebrane wiadomości są przechowywane w kolejce wiadomości. Funkcja ``receive`` 
zwraca najstarsze wiadomości z kolejki jako ciąg znaków, robiąc miejsce 
na nowe wiadomości przychodzące. Jeżeli kolejka się zapełni, wtedy kolejne 
wiadomości przychodzące będą ignorowane. 

To tak naprawdę wszystko! (Chociaż moduł radia jest wystarczająco zaawansowany aby przesyłać dowolny typ danych, nie tylko ciągi znaków. Sprawdź w dokumentacji API jak to działa.)

	
Będąc wyposażonym w tę wiedzę, łatwo jest stworzyć micro:bit'owe świetliki:

.. include:: ../../examples/radio.py
    :code: python


Import odbywa się w pętli zdarzeń. Najpierw, sprawdza czy przycisk A został wciśnięty,
jeśli tak, to wysyła wiadomość "flash" ("błyskaj"). Następnie urządznie czyta
każdą wiadomość z kolejki przy pomocy ``radio.receive()``. Jeżeli w kolejce
jest wiadomość, następuje uśpienie na krótki losowy okres (aby uczynić pokaz bardziej 
interesującym) i używa metody ``display.show()`` do animowania błysków świetlików. Ostatecznie, aby było 
jeszcze bardziej ekscytująco, wybiera losowy numer tak, że ma jedną na dziesięć 
szans na przesłanie wiadomości "flash" dalej do pozostałych (jest to sposób na
kontynuowanie pokazu świetlików wśród kilku urządzeń). Jeżeli urządzenie 
zdecyduje się przesłać wiadomość dalej, czeka pół sekundy (by początkowa
wiadomości wygasła) przed ponownym wysłaniem wiadomości "flash". 
Ponieważ ten kod znajduje się w bloku ``while True``, rozpoczyna pętlę 
od początku i powtarza ją w nieskończoność.


Końcowy wynik (korzystając z grupy micro:bitów) powinien wyglądać mniej więcej tak:

.. image:: mb-firefly.gif

.. footer:: Animacja schematu przeliczania na liczby binarne jest udostępniona na warunkach licencji 
dostępnej pod adresem: https://en.wikipedia.org/wiki/File:Binary_counter.gif
