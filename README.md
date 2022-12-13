# ATMega328P-PU DIP-28

This is a living document authored by James Jeffery.

[Datasheet](Microchip-ATMEGA328P-PU-datasheet.pdf)

## Specs

* **AVR Microcontroller**
* **Supply voltage**: 1.8V - 5.5V
* **Core Size**: 8-bit
* **Speed:** up to 20MHz
* **I/O Pins**: 23
* **Program memory size**: 32Kb (16K x 16)
* **Program memory type**: Flash
* **EEPROM size**: 1K * 8
* **RAM size**: 2K * 8
* **Package**: DIP-28

## Pinout

![ATMega328P Pinout](https://github.com/CoderJayUK/atmega328P-PU/blob/main/atmega328P-pinout.png)

## Development Environment

Use Linux. Don't bother with Windows for embedded programming. You will waste
numerous hours. I use Lubuntu on my Lenovo X250.

    sudo apt update
    sudo apt upgrade
    sudo apt install gcc-avr binutils-avr avr-libc gdb-avr avrdude

## Programming the ATMega328P

I'm use my handy PICkit2 to program the ATMega328P. The PICkit2 is
a versatile tool that can even be used as a [logic analyser](https://sigrok.org/wiki/Microchip_PICkit2).

### ATMega328P PICkit2 Mapping

| Pin | PICkit2   | ATMega | Pin |
|-----|-----------|--------|-----|
| 1   | VPP       | Reset  | 1   |
| 2   | VDD       | VCC    | 7   |
| 3   | VSS       | Ground | 8   |
| 4   | ICSPDAT   | MISO   | 18  |
| 5   | ICSPCL    | SCK    | 19  |
| 6   | Auxillary | MOSI   | 17  |

### Using AVRDude

AVRDude is used to flash the ATMega329P using the PICkit2.

#### List Supported Devices

Using the following command you can check what hardware programmers are supported:

    avdude -c nope
    
By passing `nope` we force AVRDude to display a list of supported programmers.
Check that your installation supports `pickit2`.

#### List Supported Part Numbers

To check what part numbers are supported run the following command:

    avrdude -c pickit2
    
This will spit out a long list of supported AVR chips. We're interested in the
`m328p` part number which is for the ATMega328P.

#### Writing Data To Chip

    avrdude -c pickit -p m328p -U <memtype>:r|w|v:<filename>
    
* **memtype**: Either **flash**, **eeprom**, **hfuse** (high fuse), **lfuse**
  (low fuse) or **efuse** (extended fuse)
* **r|w|v**: Either **r** (read), **w** (write), **v** (verify)
* **<filename>**: The input or output file (reading or writing respectively)

    avrdude -c pickit -p m328p -U flash:w:prog.hex
