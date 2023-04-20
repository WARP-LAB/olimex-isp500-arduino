# Olimex ISP500 (AVR-ISP500-TINY) configs for Arduino

Notes for AVR-ISP500-TINY when used as ISP programmer via [Arduino IDE](https://www.arduino.cc/en/Main/Software). *Thus these notes are not for VisualGDB, Atmel Studio, straight AVRDude.*

For ATmega and ATtiny.

Tested on macOS 10.12 and MSW10, Arduino IDE 1.8.4.

# Programmer

### Firmware

Follow [manual](https://www.olimex.com/Products/AVR/Programmers/AVR-ISP500-TINY/), use MSW to burn in latest Olimex firmware (1.07). Already done on our units.

### Programmer definition

*programmers.txt*

```
olimexisp500tiny.name=Olimex AVR-ISP500-TINY
olimexisp500tiny.communication=serial
olimexisp500tiny.protocol=stk500v2
olimexisp500tiny.program.protocol=stk500v2
olimexisp500tiny.program.tool=avrdude
olimexisp500tiny.program.tool.default=avrdude
olimexisp500tiny.program.extra_params=-P{serial.port}
```

# Boards

## ATmega328p

### ATmega328p (8 Mhz internal)

Use hex found in *breadboard-1-6-x.zip* linked from [this Arduino tut](https://www.arduino.cc/en/Tutorial/ArduinoToBreadboard).

Update board definition inside `breadboard/avr/boards.txt`

*boards.txt*

```
##############################################################

atmega328bb.name=ATmega328p (8 MHz internal) 1.8.4

atmega328bb.upload.protocol=arduino
atmega328bb.upload.maximum_size=30720
atmega328bb.upload.speed=57600

atmega328bb.bootloader.low_fuses=0xE2
atmega328bb.bootloader.high_fuses=0xDA
atmega328bb.bootloader.extended_fuses=0xFD

atmega328bb.bootloader.file=atmega/ATmegaBOOT_168_atmega328_pro_8MHz.hex
# 1.8.4. warning You probably want to use 0xff instead of 0x3f (double check with your datasheet first).
atmega328bb.bootloader.unlock_bits=0x3F
# 1.8.4. warning You probably want to use 0xcf instead of 0x0f (double check with your datasheet first).
atmega328bb.bootloader.lock_bits=0x0F

atmega328bb.build.mcu=atmega328p
atmega328bb.build.board=AVR_ATMEGA328BB
atmega328bb.build.f_cpu=8000000L
atmega328bb.build.core=arduino:arduino
atmega328bb.build.variant=arduino:standard

atmega328bb.bootloader.tool=arduino:avrdude
atmega328bb.upload.tool=arduino:avrdude

##############################################################

atmega328bbo.name=ATmega328p (8 MHz internal) 1.6.x

atmega328bbo.upload.protocol=arduino
atmega328bbo.upload.maximum_size=30720
atmega328bbo.upload.speed=57600

atmega328bbo.bootloader.low_fuses=0xE2
atmega328bbo.bootloader.high_fuses=0xDA
atmega328bbo.bootloader.extended_fuses=0x05

atmega328bbo.bootloader.file=atmega/ATmegaBOOT_168_atmega328_pro_8MHz.hex
atmega328bbo.bootloader.unlock_bits=0x3F
atmega328bbo.bootloader.lock_bits=0x0F

atmega328bbo.build.mcu=atmega328p
atmega328bbo.build.board=AVR_ATMEGA328BBO
atmega328bbo.build.f_cpu=8000000L
atmega328bbo.build.core=arduino:arduino
atmega328bbo.build.variant=arduino:standard

atmega328bbo.bootloader.tool=arduino:avrdude
atmega328bbo.upload.tool=arduino:avrdude
```

### ATmega328p (other clock sources & speeds)

Just change low byte fuses - [see manual](http://www.atmel.com/images/Atmel-7810-Automotive-Microcontrollers-ATmega328P_Datasheet.pdf)

* 8.2 Clock Sources
* 8.3 Low Power Crystal Oscillator
* 8.5 Low Frequency Crystal Oscillator
* a.o.

[Here is a good summary by Martyn Currey](http://www.martyncurrey.com/arduino-atmega-328p-fuse-settings/).

And high byte fuses.

If you are making "Uno clone", then just use UNO definitions.


### ATmega328p notes

Consider dropping Arduino bootloader altogether ^_^

## ATtiny family

[ATTinyCore by Spence Konde & Co.](https://github.com/SpenceKonde/ATTinyCore/releases) works OOB with Olimex as ISP.

Do not use [ATtiny by David A. Mellis](https://github.com/damellis/attiny).

Avoid Arduino functions, work with registers, mkay ^_^
