You need:

1. Core libraries (lib/arduino/core, lib/arduino/variants): https://github.com/arduino/ArduinoCore-avr/tree/master/cores/arduino
1. The AVR toolchain: https://www.microchip.com/mplab/avr-support/avr-and-arm-toolchains-c-compilers
1. avrdude utility for flashing: http://download.savannah.gnu.org/releases/avrdude/

So, there shall be two directories, for example:

1. "C:\tools\avr8-gnu-toolchain-3.7.0.1796-win32.any.x86_64"
1. "C:\tools\avrdude-6.4-mingw32"

Then use https://blog.jetbrains.com/clion/2020/08/arduino-from-hobby-to-prof-p1/ for configuring the toolchain and
CMake profile.