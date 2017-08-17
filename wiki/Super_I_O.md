---
title: Super I/O
permalink: wiki/Super_I/O/
layout: wiki
---

The Super I/O board is a feature of [Development
Kits](/wiki/Development_Kits "wikilink"). The board is build around a SMSC
LPC47M157 chip
([Datasheet](https://drive.google.com/uc?export=download&id=0BxOesalXbGtOanoxenlqQUh6Y0k))

The board provides the following ports:

-   RS232 (used for [ Kernel debugging](/wiki/Kernel_Debug "wikilink"), not to
    be confused with [Xbox Debug
    Monitor](/wiki/Xbox_Debug_Monitor "wikilink"))

unpopulated ports or functions are:

-   PS/2 Mouse port
-   PS/2 Keyboard port
-   something MCPX (SMBus?)
-   Temp something (SMBus?)
-   Post code (SMBus?)
-   Flash-ROM / BIOS (like modchips, replaces onboard kernel?)

[Picture of the board](http://codeasm.com/xbox/images/dvt4/SL734874.JPG)

Schematic
---------

Its a four layer board, layers 2 and 3 are filled on the entire board,
probablu ground and power planes. north, or up in the next tables is up
as written the layer numbers and silkscreen common direction. The
folowing main parts are populated on the board:

| | total | Labels                                                               | Description                                         |
|---------|----------------------------------------------------------------------|-----------------------------------------------------|
| 1       | U1                                                                   | SMSC LPC47M157-NC (1996 )                           |
| 1       | U3                                                                   | MAX223EAI (0104, first week 2004?)                  |
| 1       | Y2                                                                   | CMX-309FB B (14.3181Mhz standard Clock Oscillator ) |
| 1       | J7                                                                   | AMP rs232 Male connector                            |
| 1       | J9                                                                   | 16 pins male header (LPC bus)                       |
| 5       | R1,R7,R10,R11,R12                                                    | 10Kohm smd resistor                                 |
| 7       | C10,C12,C13,C16,C17,C36,C37                                          | Bigger, probably NOT all the same caps              |
| 17      | C1,C2,C9,C15,C1?(8?),C21,C22,C23,C24,C25,C26,C28,C31,C32,C33,C34,C35 | Smaller, also, asuming not all the same             |

Maybe someday it will look better, but for now here are the listings of
wich pin goes where:

| | Pin      | to pin            | Note              |
|------------|-------------------|-------------------|
| U1 pin 18  | C17(up) and ?     | (not finished)    |
| U1 pin 6,7 | C17(down) and GND | Both U1 pins yes) |
||

| | Pin     | to pin                       | Note               |
|-----------|------------------------------|--------------------|
| J9 pin 1  |                              | (not finished)     |
| J9 pin 2  | GND                          |                    |
| J9 pin 3  |                              | (not finished)     |
| J9 pin 4  | NC                           | no pin             |
| J9 pin 5  |                              | RST(not finished)  |
| J9 pin 6  | C12(e),C13(S),C15(S),U3 p11. | 5V(not finished)   |
| J9 pin 7  |                              | LAD3(not finished) |
| J9 pin 8  |                              | LAD2(not finished) |
| J9 pin 9  | 3.3V                         | (not finished)     |
| J9 pin 10 |                              | LAD1(not finished) |
| J9 pin 11 |                              | LAD0(not finished) |
| J9 pin 12 | GND                          |                    |
| J9 pin 13 |                              | SCL(not finished)  |
| J9 pin 14 |                              | SDA(not finished)  |
| J9 pin 15 | 3.3V                         | (not finished)     |
| J9 pin 16 |                              | (not finished)     |

Related links
-------------

-   [Super I/O emulation in
    XQEMU](https://github.com/espes/xqemu/blob/xbox/hw/xbox/lpc47m157.c)

(SMBus?)

-   [Images (CC0 License) by Codeasm with detailed shots of the SuperIO
    board](http://imgur.com/a/vJi9E)

