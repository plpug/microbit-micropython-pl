MicroPython dla BBC micro:bit
=================================

Oto kod źródłowy MicroPythona działającego na BBC micro:bit!

By nawiązać kontakt ze społecznością, zasubskryguj listę mailową
microbit@python.org (https://mail.python.org/mailman/listinfo/microbit). Musisz
być jej członkiem, by móc wysyłać wiadomości.

W niniejszym repozytorium znajdują się różne rzeczy, w tym:
- kod źródowy w katalogach source/ oraz inc/,
- przykładowe skrypty Pythona w katalogu examples/,
- narzędzia w katalogu tools/.

Kod źródłowy jest aplikacją yotta i należy go skompilować przy użyciu
yotta oraz kompilatora ARM (np. arm-none-eabi-gcc i jego przyjaciół).

Użytkownicy Ubuntu mogą uzyskać wszytkie potrzebne pakiety poprzez:
```
sudo add-apt-repository -y ppa:team-gcc-arm-embedded
sudo add-apt-repository -y ppa:pmiller-opensource/ppa
sudo apt-get update
sudo apt-get install cmake ninja-build gcc-arm-none-eabi srecord libssl-dev
pip3 install yotta
```

Once all packages are installed, use yotta to build.  You will need an ARM
mbed account to complete the first command, and will be prompted to create one
as a part of the process.

- Use target bbc-microbit-classic-gcc-nosd:

  ```
  yt target bbc-microbit-classic-gcc-nosd
  ```

- Run yotta update to fetch remote assets:

  ```
  yt up
  ```

- Start the build:

  ```
  yt build
  ```

The resulting microbit-micropython.hex file to flash onto the device can be
found in the build/bbc-microbit-classic-gcc-nosd/source from the root of the
repository.

There is a Makefile provided that does some extra preprocessing of the source,
which is needed only if you add new interned strings to qstrdefsport.h.  The
Makefile also puts the resulting firmware at build/firmware.hex, and includes
some convenience targets.

How to use
==========

Upon reset you will have a REPL on the USB CDC serial port, with baudrate
115200 (eg picocom /dev/ttyACM0 -b 115200).

Then try:

    >>> import microbit
    >>> microbit.display.scroll('hello!')
    >>> microbit.button_a.is_pressed()
    >>> dir(microbit)

Tab completion works and is very useful!

Read our documentation here:

https://microbit-micropython.readthedocs.io/en/latest/

You can also use the tools/pyboard.py script to run Python scripts directly
from your PC, eg:

    $ ./tools/pyboard.py /dev/ttyACM0 examples/conway.py

Be brave! Break things! Learn and have fun!
