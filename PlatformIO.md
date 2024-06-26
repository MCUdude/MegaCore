# PlatformIO

[PlatformIO](https://platformio.org) is an open-source ecosystem for embedded development. 
It has a built-in library manager and is Arduino-compatible. It supports most operating systems; Windows, MacOS, Linux 32 and 64-bit, ARM, and X86.  
And best of all, MegaCore is supported!

* [What is PlatformIO?](http://docs.platformio.org/en/latest/what-is-platformio.html)
* [PlatformIO IDE](http://platformio.org/#!/platformio-ide)
* Getting started with [PlatformIO IDE](http://docs.platformio.org/en/latest/ide/atom.html#quick-start) or [PlatformIO command line interface](http://docs.platformio.org/en/latest/quickstart.html)
* [Advanced functionality](http://docs.platformio.org/en/latest/platforms/atmelavr.html) 
* [Project Examples](http://docs.platformio.org/en/latest/platforms/atmelavr.html#examples)


## MegaCore + PlatformIO 
MegaCore and PlatformIO go great together. You can do serial uploads and upload using a dedicated programmer, but you can also let PlatformIO calculate the fuses and load the correct bootloader file, just like Arduino IDE does!

PlatformIO uses the information provided in platformio.ini to calculate what fuse bits and what bootloader file to load.  
Provide enough information and run the following commands:  

```ini
; Only set fuses
pio run -t fuses -e fuses_bootloader
; Set fuses and burn bootloader
pio run -t bootloader -e fuses_bootloader
; (where "fuses_bootloader" can be replaced with a different environment to match your build configuration)
```

You can find a platformio.ini template you can use when creating a project for a MegaCore-compatible device below.  
The most common functionality is available in this template. As you can see, the template is divided into multiple environments.  

* The default build environment is defined under `[platformio]`.
* All parameters common for all environments are defined under `[env]`.
* Use the `[env:Upload_UART]` or `[env:Upload_ISP]` to upload to your target.
* Use `[env:fuses_bootloader]` to set the fuses or burn the bootloader.

More information on what each line means can be found further down on this page.


## platformio.ini template

``` ini
; PlatformIO template configuration file for MegaCore
; https://github.com/MCUdude/MegaCore/
;
;   Build options: build flags, source filter
;   Hardware options: oscillator type, BOD, UART number, EEPROM retain
;   Upload options: custom upload port, speed, and extra flags
;   Library options: dependencies, extra library storage
;   Advanced options: extra scripting
;
; Please visit the documentation for the other options
; https://github.com/MCUdude/MegaCore/blob/master/PlatformIO.md
; https://docs.platformio.org/page/projectconf.html
; https://docs.platformio.org/en/latest/platforms/atmelavr.html


[platformio]
default_envs = Upload_UART ; Default build target


; Common settings for all environments
[env]
platform = atmelavr
framework = arduino

; TARGET SETTINGS
; Chip in use
board = ATmega128
; Clock frequency in [Hz]
board_build.f_cpu = 16000000L

; BUILD OPTIONS
; Comment out to enable LTO (this line unflags it)
build_unflags = -flto
; Extra build flags
build_flags = 

; SERIAL MONITOR OPTIONS
; Serial monitor port defined in the Upload_UART environment
monitor_port = ${env:Upload_UART.upload_port}
; Serial monitor baud rate
monitor_speed = 9600


; Run the following command to upload with this environment
; pio run -e Upload_UART -t upload
[env:Upload_UART]
; Serial bootloader protocol
upload_protocol = urclock
; Serial upload port
upload_port = /dev/cu.usbserial*
; Set upload baudrate. Can be changed on the fly if using Urboot
board_upload.speed = ${env:fuses_bootloader.board_bootloader.speed}


; Run the following command to upload with this environment
; pio run -e Upload_ISP -t upload
[env:Upload_ISP]
; Custom upload procedure
upload_protocol = custom
; Avrdude upload flags
upload_flags =
  -C$PROJECT_PACKAGES_DIR/tool-avrdude/avrdude.conf
  -p$BOARD_MCU
  -PUSB
  -cusbasp
; Avrdude upload command
upload_command = avrdude $UPLOAD_FLAGS -U flash:w:$SOURCE:i


; Run the following command to set fuses
; pio run -e fuses_bootloader -t fuses
; Run the following command to set fuses + burn bootloader
; pio run -e fuses_bootloader -t bootloader
[env:fuses_bootloader]
board_hardware.oscillator = external ; Oscillator type
board_bootloader.type = urboot       ; urboot, optiboot or no_bootloader
board_bootloader.speed = 115200      ; Bootloader baud rate
board_hardware.uart = uart0          ; Set UART to use for serial upload
board_hardware.bod = 2.7v            ; Set brown-out detection
board_hardware.eesave = yes          ; Preserve EEPROM when uploading using programmer
upload_protocol = usbasp             ; Use the USBasp as programmer
upload_flags =                       ; Select USB as upload port and divide the SPI clock by 8
  -PUSB
  -B8
```


### `board`
PlatformIO requires the `board` parameter to be present.
The table below shows what board name should be used for each target

| Target                                         | Board name   |
|------------------------------------------------|--------------|
| ATmega6490P/PA                                 | `ATmega6490P`|
| ATmega6490/A                                   | `ATmega6490` |
| ATmega6450P                                    | `ATmega6450P`|
| ATmega6450/A                                   | `ATmega6450` |
| ATmega3290P/PA                                 | `ATmega3290P`|
| ATmega3290/A                                   | `ATmega3290` |
| ATmega3250P                                    | `ATmega3250P`|
| ATmega3250/A                                   | `ATmega3250` |
| ATmega2561/V                                   | `ATmega2561` |
| ATmega2560/V                                   | `ATmega2560` |
| ATmega1281/V                                   | `ATmega1281` |
| ATmega1280/V                                   | `ATmega1280` |
| ATmega649P/PA                                  | `ATmega649P` |
| ATmega649/A                                    | `ATmega649`  |
| ATmega645P                                     | `ATmega645P` |
| ATmega645/A                                    | `ATmega645`  |
| ATmega640/V                                    | `ATmega640`  |
| ATmega329P/PA                                  | `ATmega329P` |
| ATmega329/A                                    | `ATmega329`  |
| ATmega325P                                     | `ATmega325P` |
| ATmega325/A                                    | `ATmega325`  |
| ATmega169P/PA                                  | `ATmega169P` |
| ATmega169/A                                    | `ATmega169A` |
| ATmega165P                                     | `ATmega165P` |
| ATmega165/A                                    | `ATmega165`  |
| ATmega128/A                                    | `ATmega128`  |
| ATmega64/A                                     | `ATmega64`   |
| AT90CAN128                                     | `AT90CAN128` |
| AT90CAN64                                      | `AT90CAN64`  |
| AT90CAN32                                      | `AT90CAN32`  |


### `board_build.f_cpu`
Specifies the clock frequency in [Hz]. 
Used to determine what oscillator option to choose. A capital L has to be added to the end of the frequency number.
Below is a table with supported clocks. Defaults to 16 MHz if not specified.

| Clock speed | Oscillator | board_build.f_cpu         |
|-------------|------------|---------------------------|
| 20 MHz      | External   | `20000000L`               |
| 18.432 MHz  | External   | `18432000L`               |
| 16 MHz      | External   | `16000000L` (default)     |
| 14.7456 MHz | External   | `14745600L`               |
| 12 MHz      | External   | `12000000L`               |
| 11.0592 MHz | External   | `11059200L`               |
| 9.216 MHz   | External   | `9216000L`                |
| 8 MHz       | External   | `8000000L`                |
| 7.3728  MHz | External   | `7372800L`                |
| 6 MHz       | External   | `6000000L`                |
| 4 MHz       | External   | `4000000L`                |
| 3.6864  MHz | External   | `3686400L`                |
| 2 MHz       | External   | `2000000L`                |
| 1.8432  MHz | External   | `1843200L`                |
| 1 MHz       | External   | `1000000L`                |
| 8 MHz       | Internal   | `8000000L`                |
| 1 MHz       | Internal   | `1000000L`                |


### `board_hardware.oscillator`
Specifies to use the internal or an external oscillator.  
Internal oscillator only works with `board_build.f_cpu` values `8000000L` and `1000000L`.

| Oscillator option    |
|----------------------|
| `external` (default) |
| `internal`           |


### `board_bootloader.type`
Specifies which bootloader type to burn.

| Bootloader type         |
|-------------------------|
| `urboot` (default)      |
| `optiboot` (deprecated) |
| `no_bootlaoder`         |


### `board_hardware.uart`
Specifies the hardware UART port used for serial upload. Use `no_bootloader` if you’re using a dedicated programmer, i.e. not using a bootloader for serial upload.

| Upload serial port option |
|---------------------------|
| `no_bootloader`           |
| `uart0` (default)         |
| `uart1`                   |
| `uart2`                   |
| `uart3`                   |


### `board_hardware.bod`
Specifies the hardware brown-out detection. Use `disabled` to disable.

| ATmega640/1280/1281/2560/2561 | ATmega64/128     | AT90CAN32/64/128 |
|-------------------------------|------------------|------------------|
| `4.3v`                        | `4.0v`           | `4.1v`           |
| `2.7v` (default)              | `2.7v` (default) | `4.0v`           |
| `1.8v`                        |                  | `3.9v`           |
| `disabled`                    | `disabled`       | `3.8v`           |
|                               |                  | `2.7v` (default) |
|                               |                  | `2.6v`           |
|                               |                  | `2.5v`           |
|                               |                  | `disabled`       |


### `board_hardware.eesave`
Specifies if the EEPROM memory should be retained when uploading using a programmer. Use `no` to disable.

| EEPROM retain   |
|-----------------|
| `yes` (default) |
| `no`            |


### `board_hardware.ckout`
Enable clock output on targets that support it.  
Pin PE7 on ATmega1280/1281/2560/2561, ATmega165/169/325/329/645/649 and ATmega3250/3290/6450/6490
Pin PC7 on AT90CAN32/64/128  

| Clock output enable |
|---------------------|
| `yes`               |
| `no` (default)      |


### `board_hardware.jtagen`
Enable the JTAG port.

| JTAG enable     |
|-----------------|
| `yes`           |
| `no` (default)  |


### `board_upload.speed` / `board_bootloader.speed`
Specifies the upload baud rate. Valid baud rates are shown in the table below. The Urboot bootloader has auto baud support, so the upload baud rate can be changed without re-flashing the bootloader. The only exception to this rule is UART2 and UART3 on the ATmega640/1280/2560, which does *not* support auto baud. Optiboot on the other hand is compiled to support a specific baud rate, and a different bootloader binary needs to be flashed to support a different baud rate.
  
**Note that if you're using a programmer that communicates with Avrdude with a serial port (Arduino as ISP, STK500, etc.) the `board_upload.speed` field will interfere with the programmer's baud rate.  
In this case, use `board_bootloader.speed` to set the bootloader baud rate, and `board_upload.speed` to set the baud rate for the programmer.**  

Suggested baud rates for a particular clock speed are in **bold text**.

|             | 1000000 | 500000 | 460800 | 250000 | 230400 | 115200 | 57600  | 38400  | 19200 | 9600   |
|-------------|---------|--------|--------|--------|--------|--------|--------|--------|-------|--------|
| `20000000L` |         |  X     |        |  X     |        |  **X** |        |        |  X    |        |
| `18432000L` |         |        |  X     |        |  X     |  **X** |  X     |  X     |  X    |  X     |
| `16000000L` |  X      |  X     |        |  X     |        |  **X** |        |  X     |  X    |  X     |
| `14745600L` |         |        |  X     |        |  X     |  **X** |  X     |  X     |  X    |  X     |
| `12000000L` |         |  X     |        |  X     |        |        |  **X** |        |  X    |  X     |
| `11059200L` |         |        |  X     |        |  X     |  X     |  **X** |  X     |  X    |  X     |
| `9216000L`  |         |        |        |        |  X     |  **X** |  X     |  X     |  X    |  X     |
| `8000000L`  |  X      |  X     |        |  X     |        |  X     |  X     |  **X** |  X    |  X     |
| `7372800L`  |         |        |  X     |        |  X     |  **X** |  X     |  X     |  X    |  X     |
| `6000000L`  |         |        |        |  X     |        |        |  **X** |  X     |  X    |  X     |
| `4000000L`  |         |  X     |        |        |        |        |        |        |  X    |  **X** |
| `3686400L`  |         |        |  X     |        |  X     |  **X** |  X     |  X     |  X    |  X     |
| `2000000L`  |         |        |        |  X     |        |        |        |        |  X    |  **X** |
| `1843200L`  |         |        |        |        |  X     |  **X** |  X     |  X     |  X    |  X     |
| `1000000L`  |         |        |        |        |        |        |        |        |       |  **X** |



### `board_build.variant`
Holds the current pinout in use. The alternative pinout is only available for ATmega640/1280/2560.
See [pinout pics](https://github.com/MCUdude/MegaCore#pinout) for more info.

| Target                                                                                                        | Default pinout         | Alternative pinout |
|---------------------------------------------------------------------------------------------------------------|------------------------|--------------------|
| ATmega2560/V <br/>ATmega1280/V <br/>ATmega640/V                                                               | `100-pin-arduino-mega` | `100-pin-avr`      |
| ATmega2561/V <br/>ATmega1281/V <br/>ATmega128/A <br/>ATmega64/A <br/>AT90CAN128 <br/>AT90CAN64 <br/>AT90CAN32 | `64-pin-avr`           |                    |


### `build_unflags`
This parameter is used to unflag. Since LTO is enabled by default in PlatformIO we may disable it by unflagging `-flto`.

### `build_flags`
This parameter is used to set compiler flags. This is useful if you want to, for instance, charge the serial RX or TX buffer. Here's a list of the currently available core file flags:

| Flag                        | Default size | Description                                               |
|-----------------------------|--------------|-----------------------------------------------------------|
| -lprintf_flt                |              | Lets you print floats with printf (occupies ~1.5 kB)      |
| -Wall -Wextra               |              | Show all compiler warnings                                |
| -DSERIAL_RX_BUFFER_SIZE=128 | 64 bytes     | Sets the serial RX buffer to 128 bytes                    |
| -DSERIAL_TX_BUFFER_SIZE=128 | 64 bytes     | Sets the serial TX buffer to 128 bytes                    |
| -DTWI_BUFFER_SIZE=64        | 32 bytes     | Sets the TWI (i2c) buffer to 64 bytes                     |
| -DWIRE_TIMEOUT              |              | Enable timeout for the Wire and Wire1 library             |

**Example:**
`build_flags = -DSERIAL_RX_BUFFER_SIZE=128 -DSERIAL_TX_BUFFER_SIZE=128`


### `upload_port`
Holds the serial port used for uploading. PlatformIO automatically detects the serial port. However, if you want to override this you can uncomment `upload_port`. Use `/dev/[port]` on Unix-compatible systems, and use `COMx` on Windows.


### `board_upload.speed`
Upload baud rate. See [board_build.f_cpu](#board_buildf_cpu) for more details.


### `upload_protocol`
Used when using a programmer rather than using a USB to serial adapter.  
Supports all Avrdude compatible programmers such as `usbasp`, `usbtiny` and `stk500v1`.


### `upload_flags`
Used to pass extra flags to Avrdude when uploading using a programmer.  
Typical parameters are `-PUSB`, `-B[clock divider]` and `-b[baudrate]`.  
**Note that every flag has to be on its own line, and they have to be indented with two spaces:**
```ini
upload_flags =
  -PUSB
  -B32
  -v
```

### `monitor_port`
PlatformIO detects serial ports automatically. However, if you want to override this you can uncomment `monitor_port`. Use `/dev/[port]` on Unix-compatible systems, and use `COMx` on Windows.


### `monitor_speed`
Sets the serial monitor baud rate. Defaults to 9600 if not defined.
