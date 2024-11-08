---
id: BMP_Project_Setup
aliases:
  - Project Setup
tags: []
---


# Project Setup

## Makefile

```makefile
:default:
	avr-gcc -Os -DF_CPU=16000000UL -mmcu=atmega328p -nostdinc -ffreestanding -isystem/usr/avr/include -c -o main.o main.c
	avr-gcc -o main.bin main.o
	avr-objcopy -O ihex -R .eeprom main.bin main.hex
	sudo avrdude -F -V -c arduino -p ATMEGA328P -P /dev/ttyUSB0 -b 115200 -U flash:w:main.hex
```

## LSP

Enable LSP by generating compiledb as follows:

```bash
compiledb make
```

## Resources

- [ Getting Started with Baremetal Arduino C Programming | No IDE Required [Linux SDK] ](https://youtu.be/j4xw8QomkXs?si=E9jAcrLB5qtrrXxL)
