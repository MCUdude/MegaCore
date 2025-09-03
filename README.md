# MegaCore
[![MegaCore forum thread](https://img.shields.io/badge/support-forum-blue.svg)](https://forum.arduino.cc/index.php?topic=386733.0)

An Arduino core for most 64 and 100-pin AVRs, all running the [Urboot](#write-to-own-flash) bootloader.
This core requires at least Arduino IDE v1.8, where v1.8.9 or newer is recommended. IDE 2.x should also work.

*From MegaCore version 3 and onwards, the Optiboot bootloader has been replaced by the superior [Urboot bootloader](https://github.com/stefanrueger/urboot/). It's smaller, faster, and has automatic baud rate detection, and can read and write to EEPROM. Other cool features the bootloader provides but are not utilized by MegaCore are user program metadata stored in flash (that can easily be viewed by Avrdude -xshowall) and chip-erase functionality.
If you already have Optiboot installed and don't want to replace it with Urboot, you can still upload programs without any compatibility issues. However, if you're burning a bootloader to a new chip, Urboot is the way to go.*


# Table of contents
* [Supported microcontrollers](#supported-microcontrollers)
* [Supported clock frequencies](#supported-clock-frequencies)
* [Bootloader option](#bootloader-option)
* [Baud rate option](#baud-rate-option)
* [BOD option](#bod-option)
* [EEPROM retain option](#eeprom-option)
* [Link time optimization / LTO](#link-time-optimization--lto)
* [Printf support](#printf-support)
* [Pin macros](#pin-macros)
* [Write to own flash](#write-to-own-flash)
* [PROGMEM with flash sizes greater than 64kB](#progmem-with-flash-sizes-greater-than-64kb)
* [Programmers](#programmers)
* **[How to install](#how-to-install)**
  - [Boards Manager Installation](#boards-manager-installation)
  - [Manual Installation](#manual-installation)
  - [Arduino CLI Installation](#arduino-cli-installation)
  - [PlatformIO](#platformio)
* **[Getting started with MegaCore](#getting-started-with-megacore)**
* [Wiring reference](#wiring-reference)
* **[Pinout](#pinout)**
* **[Minimal setup](#minimal-setup)**


## Supported microcontrollers:

* ATmega6490
* ATmega6450
* ATmega3290
* ATmega3250
* ATmega2561
* ATmega2560
* ATmega1281
* ATmega1280
* ATmega649
* ATmega645
* ATmega640
* ATmega329
* ATmega325
* ATmega169
* ATmega165
* ATmega128
* ATmega64
* AT90CAN128
* AT90CAN64
* AT90CAN32

(All variants - A, L, P, PA, PV, V)
<br/> <br/>
Can't decide what microcontroller to choose? Have a look at the specification table below:

|                  | Mega2560 | Mega1280 | Mega640 | Mega2561 | Mega1281 | Mega128<br/>CAN128 | Mega6490<br/>Mega6450 | Mega64<br/>CAN64<br>Mega649<br/>Mega645 | Mega6490<br/>Mega6450 | CAN32<br/>Mega329<br/>Mega325 | Mega169<br/>Mega165 |
|------------------|----------|----------|---------|----------|----------|--------------------|-----------------------|-----------------------------------------|-----------------------|-------------------------------|---------------------|
| **Flash**        | 256kB    | 128kB    | 64kB    | 256kB    | 128kB    | 128kB              | 64kB                  | 64kB                                    | 32kB                  | 32kB                          | 16kB                |
| **RAM**          | 8kB      | 8kB      | 8kB     | 8kB      | 8kB      | 4kB                | 4kB                   | 4kB                                     | 2kB                   | 2kB                           | 1 kB                |
| **EEPROM**       | 4kB      | 4kB      | 4kB     | 4kB      | 4kB      | 4kB                | 2kB                   | 2kB                                     | 1kB                   | 1kB                           | 512B                |
| **IO pins**      | 70/86 *  | 70/86 *  | 70/86 * | 54       | 54       | 53                 | 68                    | 53                                      | 68                    | 53                            | 53                  |
| **Serial ports** | 4        | 4        | 4       | 2        | 2        | 2                  | 1                     | 2 / 1 **                                | 1                     | 2 / 1 **                      | 1                   |
| **PWM pins**     | 15       | 15       | 15      | 8        | 8        | 7                  | 4                     | 7 / 4 **                                | 4                     | 7 / 4 **                      | 4                   |
| **LED pin**      | PB7      | PB7      | PB7     | PB5      | PB5      | PB5                | PB7                   | PB5                                     | PB7                   | PB5                           | PB5                 |

<b>*</b> Pin 70-85 is not broken out on the Arduino Mega. Make sure to check out the [*AVR style pinout*](#atmega64012802560) for a more straightforward pinout.

<b>**</b> ATmega165/169/325/329/645/649 has one hardware serial port and four PWM outputs.

## Supported clock frequencies

MegaCore supports a variety of different clock frequencies. Select the microcontroller in the boards menu, then select the clock frequency. *You will have to hit "Burn bootloader" in order to set the correct fuses and upload the correct bootloader. This also has to be done if you want to change any of the fuse settings (BOD and EEPROM settings) regardless if a bootloader is installed or not*.

Make sure you connect an ISP programmer, and select the correct one in the "Programmers" menu. For time-critical operations, an external crystal/oscillator is recommended. The Urboot bootloader has automatic baud rate detection (except when using UART2 and UART3 on ATmega640/1280/2560), so UART uploads should work fine even though the oscillator is a little too fast or too slow.

| Frequency   | Oscillator type             | Default upload speed <br/>(bootloader has auto-baud)  | Comment                                           |
|-------------|-----------------------------|-------------------------------------------------------|---------------------------------------------------|
| 16 MHz      | External crystal/oscillator | 115200                                                | Default clock on most AVR based Arduino boards    |
| 20 MHz      | External crystal/oscillator | 115200                                                |                                                   |
| 18.4320 MHz | External crystal/oscillator | 115200                                                | Great clock for UART communication with no error  |
| 14.7456 MHz | External crystal/oscillator | 115200                                                | Great clock for UART communication with no error  |
| 12 MHz      | External crystal/oscillator | 57600                                                 |                                                   |
| 11.0592 MHz | External crystal/oscillator | 115200                                                | Great clock for UART communication with no error  |
| 9.216 MHz   | External crystal/oscillator | 115200                                                | Great clock for UART communication with no error  |
| 8 MHz       | External crystal/oscillator | 57600                                                 | Common clock when working with 3.3V               |
| 7.3728 MHz  | External crystal/oscillator | 115200                                                | Great clock for UART communication with no error  |
| 6 MHz       | External crystal/oscillator | 57600                                                 |                                                   |
| 4 MHz       | External crystal/oscillator | 9600                                                  |                                                   |
| 3.6864 MHz  | External crystal/oscillator | 115200                                                | Great clock for UART communication with no error  |
| 2 MHz       | External crystal/oscillator | 9600                                                  |                                                   |
| 1.8432 MHz  | External crystal/oscillator | 115200                                                | Great clock for UART communication with no error  |
| 1 MHz       | External crystal/oscillator | 9600                                                  |                                                   |
| 8 MHz       | Internal oscillator         | 38400                                                 | Might cause UART upload issues. See comment above |
| 4 MHz       | Internal oscillator         | 9600                                                  | Derived from the 8 MHz internal oscillator        |
| 2 MHz       | Internal oscillator         | 9600                                                  | Derived from the 8 MHz internal oscillator        |
| 1 MHz       | Internal oscillator         | 9600                                                  | Derived from the 8 MHz internal oscillator        |


## Bootloader option
MegaCore lets you select which serial port you want to use for uploading. UART0 is the default port for all targets, but any hardware serial port may be used.
If your application doesn't need or require a bootloader for uploading you can also choose to disable it by selecting *No bootloader*.
This frees 384 bytes of flash memory on ATmega165/169/325/3250/329/3290 and 512 bytes on ATmega64/128/645/6450/649/6490/1281/2560 and AT90CAN32/64/128.

Note that you need to connect a programmer and hit **Burn bootloader** if you want to change any of the *Bootloader settings*.


# Baud rate option
Since Urboot has automatic baud rate detection, the upload baud rate can be changed without having to re-flash the bootloader. The default baud rate setting will pick a suited baud rate that also works with the legacy Optiboot bootloader used in earlier MegaCore versions.
The other baud rate options may or may not work, depending on the clock frequency and accuracy of the clock source. A rule of thumb is that "non-round" baud rates like 230400 works best with "non-round" clock speeds like 18.4320 MHz,
while "round" ones like 16 MHz work best with "round" baud rates like 250000.


## BOD option
Brown-out detection, or BOD for short lets the microcontroller sense the input voltage and shut down if the voltage goes below the brown-out setting. To change the BOD settings you'll have to connect an ISP programmer and hit "Burn bootloader". Below is a table that shows the available BOD options:
<br/>

| ATmega640/1280/2560 | ATmega1281/2561 | ATmega3290/6490<br/>ATmega3250/6450 | ATmega165/169<br/>ATmega325/329<br/>ATmega645/649 | ATmega64/128  | AT90CAN32/64/128 |
|---------------------|-----------------|-------------------------------------|---------------------------------------------------|---------------|------------------|
| 4.3V                | 4.3V            | 4.3V                                | 4.3V                                              | 4.0V          | 4.1V             |
| 2.7V                | 2.7V            | 2.7V                                | 2.7V                                              | 2.7V          | 4.0V             |
| 1.8V                | 1.8V            | 1.8V                                | 1.8V                                              |               | 3.9V             |
|                     |                 |                                     |                                                   |               | 3.8V             |
|                     |                 |                                     |                                                   |               | 2.7V             |
|                     |                 |                                     |                                                   |               | 2.6V             |
|                     |                 |                                     |                                                   |               | 2.5V             |
| Disabled            | Disabled        | Disabled                            | Disabled                                          | Disabled      | Disabled         |


## EEPROM option
If you want the EEPROM to be erased every time you burn the bootloader or upload using a programmer, you can turn off this option. You'll have to connect an ISP programmer and hit "Burn bootloader" to enable or disable EEPROM retain. Note that when uploading using a bootloader, the EEPROM will always be retained.

Note that if you're using an ISP programmer or have the Urboot bootloader installed, data specified in the user program using the `EEMEM` attribute will be uploaded to EEPROM when you upload your program in Arduino IDE. This feature is not available when using the older Optiboot bootloader.

```cpp
#include <avr/eeprom.h>

volatile const char ee_data EEMEM = {"Data that's loaded straight into EEPROM\n"};

void setup() {
}

void loop() {
}
```


## Link time optimization / LTO
Link time optimization (LTO for short) optimizes the code at link time, usually making the code significantly smaller without affecting performance. You don't need to hit "Burn Bootloader" in order to enable or disable LTO. Simply choose your preferred option in the "Tools" menu, and your code is ready for compilation. If you want to read more about LTO and GCC flags in general, head over to the [GNU GCC website](https://gcc.gnu.org/onlinedocs/gcc/Optimize-Options.html)!


## Printf support
Unlike the official Arduino cores, MegaCore has printf support out of the box. If you're not familiar with printf you should probably [read this first](https://www.tutorialspoint.com/c_standard_library/c_function_printf.htm). It's added to the Print class and will work with all libraries that inherit Print. Printf is a standard C function that lets you format text much easier than using Arduino's built-in print and println. Note that this implementation of printf will NOT print floats or doubles. This is disabled by default to save space but can be enabled using a build flag if using PlatformIO.

If you're using a serial port, simply use `Serial.printf("Milliseconds since start: %ld\n", millis());`. You can also use the `F()` macro if you need to store the string in flash. Other libraries that inherit the Print class (and thus support printf) are the LiquidCrystal LCD library and the U8G2 graphical LCD library.


## Pin macros
Note that you don't have to use the digital pin numbers to refer to the pins. You can also use some predefined macros that map "Arduino pins" to the port and port number:

```c++
// Use PIN_PE0 macro to refer to pin PE0 (Arduino pin 0)
digitalWrite(PIN_PE0, HIGH);

// Results in the exact same compiled code
digitalWrite(0, HIGH);

```


## Write to own flash
MegaCore uses the excellent Urboot bootloader, written by [Stefan Rueger](https://github.com/stefanrueger). Urboot supports flash writing within the running application, meaning that content from e.g. a sensor can be stored in the flash memory directly without needing external memory. Flash memory is much faster than EEPROM, and can handle at least 10,000 write cycles before wear becomes an issue.
For more information on how it works and how you can use this in your own application, check out the [Serial_read_write](https://github.com/MCUdude/MegaCore/blob/master/avr/libraries/Flash/examples/Serial_read_write/Serial_read_write.ino) for a simple proof-of-concept demo, and
[Flash_put_get](https://github.com/MCUdude/MegaCore/blob/master/avr/libraries/Flash/examples/Flash_get_put/Flash_get_put.ino) + [Flash_iterate](https://github.com/MCUdude/MegaCore/blob/master/avr/libraries/Flash/examples/Flash_iterate/Flash_iterate.ino) for useful examples on how you can store strings, structs, and variables to flash and retrieve then afterward.


## PROGMEM with flash sizes greater than 64kB
The  `PROGMEM` attribute stores constant data, such as string arrays, in flash and is great if you want to preserve the precious RAM. However, the usual PROGMEM library macros can access data only in the lower part of flash memory up to 64kiB.  If you store more data in the PROGMEM section, you will get a warning during the compilation process because some of the PROGMEM data will not be accessible in the usual way.

If you want to store and access data independently from the 64kiB limit, use `PROGMEM_FAR`  from the library `program_far.h` (installable using the Arduino library manager). Data with that attribute is stored at the far end of flash memory. Accessing this data is not as straightforward as with `PROGMEM`, but it's still doable:

```cpp
#include <progmem_far.h>

const char far_away[] PROGMEM_FAR = "Hello from far away!\n"; 

void print_progmem()
{
  char c;

  // Print out far_away
  for(uint8_t i = 0; i < sizeof(far_away); i++)
  {
    c = pgm_read_byte_far(pgm_get_far_address(far_away) + i);
    Serial.write(c);
  }
}

```

## Programmers

Select your microcontroller in the boards menu, then select the clock frequency. You'll have to hit "Burn bootloader" in order to set the correct fuses and upload the correct bootloader. <br/>
Make sure you connect an ISP programmer, and select the correct one in the "Programmers" menu. For time-critical operations, an external oscillator is recommended.


## How to install
#### Boards Manager Installation
This installation method requires Arduino IDE version 1.8.0 or greater.
* Open the Arduino IDE.
* Open the **File > Preferences** menu item.
* Enter the following URL in **Additional Boards Manager URLs**: `https://mcudude.github.io/MegaCore/package_MCUdude_MegaCore_index.json`
* Open the **Tools > Board > Boards Manager...** menu item.
* Wait for the platform indexes to finish downloading.
* Scroll down until you see the **MegaCore** entry and click on it.
* Click **Install**.
* After installation is complete close the **Boards Manager** window.

#### Manual Installation
Click on the "Download ZIP" button in the upper right corner. Extract the ZIP file, and move the extracted folder to the location "**~/Documents/Arduino/hardware**". Create the "hardware" folder if it doesn't exist.
Open Arduino IDE, and a new category in the boards menu called "MegaCore" will show up.

#### Arduino CLI Installation
Run the following command in a terminal:

```
arduino-cli core install MegaCore:avr --additional-urls https://mcudude.github.io/MegaCore/package_MCUdude_MegaCore_index.json
```

#### PlatformIO
[PlatformIO](http://platformio.org) is an open-source ecosystem for IoT and embedded systems, and supports MegaCore.

**See [PlatformIO.md](https://github.com/MCUdude/MegaCore/blob/master/PlatformIO.md) for more information.**


## Getting started with MegaCore
* Hook up your microcontroller as shown in the [pinout diagram](#pinout).
  - If you're not planning to use the bootloader (uploading code using a USB to serial adapter), the FTDI header and the 100 nF capacitor on the reset pin can be omitted.
* Open the **Tools > Board** menu item, select **MegaCore** and select your preferred target.
* If the *BOD option* is presented, you can select at what voltage the microcontroller will shut down at. Read more about BOD [here](#bod-option).
* Select your preferred clock frequency. **16 MHz** is standard on most Arduino boards.
* Select what kind of programmer you're using under the **Programmers** menu.
* Hit **Burn Bootloader**. The LED pin will *not* toggle after the bootloader has been loaded.
* Disconnect the ISP programmer, and connect a USB to serial adapter to the target microcontroller shown in the [pinout diagram](#pinout). Select the correct serial port under the **Tools** menu, and click the **Upload** button. If you're getting a timeout error, it may be because the RX and TX pins are swapped, or the auto-reset circuit isn't working properly (the 100 nF capacitor and a 10k resistor on the reset line).

Your code should now be running on the microcontroller!


## Wiring reference
To extend this core's functionality a bit further, I've added a few missing Wiring functions to this hardware package. As many of you know Arduino is based on Wiring, but that doesn't mean the Wiring development isn't active. These functions are used as "regular" Arduino functions, and there's no need to include an external library.<br/>
I hope you find this useful because they really are!

### Function list
* portMode()
* portRead()
* portWrite()
* sleepMode()
* sleep()
* noSleep()
* enablePower()
* disablePower()

### For further information please view the [Wiring reference page](https://github.com/MCUdude/MegaCore/blob/master/Wiring_reference.md)


## Pinout

### 64-pin chips
MegaCore provides a standard, logical pinout for the 64-pin chips. The standard LED pin is assigned to digital pin 13/PIN_PB5. <br/>
<b>Click to enlarge:</b> <br/>

| ATmega64/128/1281/2561<br/>AT90CAN32/CAN64/CAN128      | ATmega165/169/325/329/645/649                           |
|--------------------------------------------------------|---------------------------------------------------------|
|<img src="https://i.imgur.com/sweRJs3.jpg" width="280"> | <img src="https://i.imgur.com/YJ3ojm1.png" width="280"> |

### 100-pin chips
MegaCore provides a standard, logical pinout for the 64-pin chips. The standard LED pin is assigned to digital pin 13/PIN_PB5.
MegaCore includes the original Arduino Mega pinout for the ATmega640/1280/2560, but also a straightforward, "standard" pinout for _all_ 100-pin chips.
The standard LED pin is assigned to digital pin 13 on the Arduino MEGA pinout and digital pin 22 on the standard pinout (PIN_PB7 in both cases). <br/>
<b>Click to enlarge:</b> <br/>

| ATmega640/1280/2560<br/>Arduino MEGA pinout             | ATmega640/1280/2560<br/>"AVR" pinout                   | ATmega3250/3290/6450/6490                               |
|---------------------------------------------------------|--------------------------------------------------------|---------------------------------------------------------|
| <img src="https://i.imgur.com/O7WtWAj.jpg" width="240"> | <img src="http://i.imgur.com/DfR7arD.jpg" width="240"> | <img src="https://i.imgur.com/F80M5u1.jpg" width="240"> |


## Minimal setup
Here are some simple schematics showing a minimal setup using an external crystal. Omit the crystal and the two 22pF capacitors if you're using the internal oscillator. <br/>
<b>Click to enlarge:</b> <br/>

| ATmega64/128/1281/2561<br/>AT90CAN32/CAN64/CAN128      | ATmega165/325/645<br/>ATmega169/329/649                 | ATmega640/1280/2560                                    | ATmega3250/6450<br/>ATmega3290/6490                    |
|--------------------------------------------------------|---------------------------------------------------------|--------------------------------------------------------|--------------------------------------------------------|
|<img src="https://i.imgur.com/UlGwP45.png" width="200"> | <img src="https://i.imgur.com/d5Otnpj.png" width="200"> | <img src="http://i.imgur.com/gQS1ORv.png" width="200"> |<img src="https://i.imgur.com/jdPptok.png" width="200"> |

