
# PI1551 HAT

This is a minimalistic HAT design for RaspberryPI 3A/3B/3B+ for a low-cost 1551 floppy emulation solution for your Commodore C16/C116 or Plus4.

This hat doesn't just support 1551 emulation. It can also playback TAP files.

**Warning: This is for [tcbm2sd](https://github.com/ytmytm/plus4-tcbm2sd) ONLY. DO NOT connect PI1551 HAT to 1551 paddle. This circuit is not 5V-tolerant - it will damage your RaspberryPi.**

<img src="media/01.pcb.png" width=640 alt="PI1551 HAT PCB">


## Origin

This has been cloned off [PI1541 Hat](https://github.com/tebl/C64-Pi1541-module) with changes to support 1551 emulation and tape playback.

## Software

The RaspberryPI firmware comes from [PI1551 project](https://github.com/ytmytm/Pi1551) (branch 'pi1551').

## Hardware

The KiCad files are provided in the repository.

- [Schematic PDF](plots/RPi_Hat.pdf)
- [Gerbers](plots/)

<img src="media/03.assembled.jpg" width=640 alt="PI1551 HAT fully assembled">

### Required parts

The only required parts for 1551 emulation are:

- 40x2 female socket for RaspberryPI GPIO connector
- 8x2 male connector for a ribbon cable leading to [tcbm2sd](https://github.com/ytmytm/plus4-tcbm2sd)
- D2 (BAT54 or BAS40 or BAT43 or 1n5819 Schottky diode) to protect RaspberryPi
- R2 (10K) (companion to D2)

Everything else is optional.

You also need a straight 16-wire ribbon cable. The connections are 1:1, so as long as both ends are done in exactly the same way it doesn't really matter on which side the red stripe is and towards which side (notch or without a notch) the ribbon cable goes out of the connector. However I recommend crimping the cable in the same way as on the image above, with both connectors done in the same way. It makes cable routing much easier on both ends.

You can order a PCB with all the SMD parts already populated:

<a href="https://www.pcbway.com/project/shareproject/PI1551_HAT_a7817d89.html"><img src="https://www.pcbway.com/project/img/images/frompcbway-1220.png" alt="PCB from PCBWay" /></a>

### Display

- OLED display: SSD1306 128x64 or 128x32 or SH1106 128x64 (that one is large, so you can't use rotary encoder)
- OLED display with GND/VCC/SCL/SCK (default) or VCC/GND/SCL/SCK pinout, controlled with solder jumpers

### Drive LED indicator

- 3mm or 5mm LED
- R1 (220R)s

### Audio

- 3V buzzer with generator

### Controls

- 5 buttons (6x6mm tact switch) to control Pi1551
- or two buttons and rotary encoder (ALPS EC11E)
- or two buttons and rotary encoder module KY-040 (with the same rotary encoder part)

### TAP playback

- MiniDIN 7-pin socket for tape emulation
- or a header to solder wires if you only have one MiniDin 7-pin plug

The only transistor/resistor section required is on the `TAP_READ` block (Q2, R5, R8) and you need a MiniDIN socket or use connector on `J5` to solder the cable directly.

The other parts of TAP section are optional, but I recommend installing all of them anyway.

For example, you may choose not to check the `MOTOR` line and wire jumper `JP2` so that (for Pi1551) the motor line is always enabled.

In the opposite direction, you might decide not to solder the `TAP_SENSE` block (Q1, R3, R4) and wire `JP1` so that the `SENSE` line is always active and the computer sees the tape PLAY button as always pressed.

All of this is also controlled by the [PI1551](https://github.com/ytmytm/Pi1551) configuration file, so you might just as well solder all the parts and decide later.

*Note that if your computer had its original CPU replaced by a 6510, then it's no longer capable of controlling the `MOTOR` line (see [here](https://hackjunk.com/2017/06/23/commodore-16-plus-4-8501-to-6510-cpu-conversion/))*

The cable must have at least two wires: `TAP_READ` and `GND` connected. At least one end of the cable must have a Mini-DIN-7 male plug.

At the time of writing this, `TAP_WRITE` is provided for future compatibility, when PI1551 will support writing to tape.

## PCB

The board is a mixture of THT and SMD parts.

Don't be afraid of SMD parts, I have used larger footprints that are more friendly for hand soldering. You need tweezers, a steady hand, iron with a fine tip and a 0.5mm solder. Use the flux generously.

<img src="media/02.pcb-smd.jpg" width=640 alt="PI1551 HAT PCB with SMD parts assembled">

Note the D2 diode orientation - the stripe is on the same side as the closed silkscreen, towards the TCBM connector. 

For convenience, the majority of resistors (R2-R10) are all the same. Use any value in the 1K-10K range you have at hand.

You might find that soldering those four SMD transistors, ten resistors and one diode is faster than their THT counterparts.

## BOM

| Reference | Item                                   | Count |
| --------- | -------------------------------------- | ----- |
| J1        | 2x20 pin long female header            |     1 |
| J2        | 2x8 pin IDC 16P socket                 |     1 |
| J3        | KY-040 rotary encoder module           |    (1)|
| J4        | MiniDIN-7 female socket                |    (1)|
| J5        | 1x7 pin long male header               |    (1)|
| BZ1       | Buzzer (5mm / 7mm pin spacing), 3V     |    (1)|
| IC1       | SSD1306 OLED-display 128x64 (0.96")    |    (1)|
| IC2       | SSD1306 OLED-display 128x32 (0.91")    |    (1)|
| SW1-SW5   | Momentary push button, 6x6mm           |    (5)|
| SW6       | ALPS EC11E rotary encoder or similar   |    (1)|
| D1        | 3mm / 5mm LED, red for authenticity    |     1 |
| D2        | Schottky diode: BAT54, 1n5819, BAT43, etc. |    1 |
| R1        | 100-220 Ohm resistor                   |     1 |
| R2-R10    | 1K-10K Ohm resistor                    |     1+(9) |
| Q1-Q4     | MMBT3904 / 2N3904 transistors          |    (4) |
