---
title: Super I/O
permalink: wiki/Super_I/O/
layout: wiki
---

The Super I/O board is a feature of some [Development
Kits](/wiki/Development_Kits "wikilink"). The board is build around a SMSC
LPC47M157 chip
([Datasheet](https://drive.google.com/uc?export=download&id=0BxOesalXbGtOanoxenlqQUh6Y0k))
and interfaces with the Xbox via a ribbon cable connected to the [LPC
Debug Port](/wiki/LPC_Debug_Port "wikilink").

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
probably ground and power planes. north, or up in the next tables is up
as written the layer numbers and silkscreen common direction. The
folowing main parts are populated on the board:

| | total | Labels                                                               | Description                                         |
|---------|----------------------------------------------------------------------|-----------------------------------------------------|
| 1       | U1                                                                   | SMSC LPC47M157-NC (1996 )                           |
| 0       | U2                                                                   | unpopulated DIP24 MCU(?)                            |
| 1       | U3                                                                   | MAX223EAI (0104, first week 2004?)                  |
| 1       | Y2                                                                   | CMX-309FB B (14.3181Mhz standard Clock Oscillator ) |
| 1       | J7                                                                   | AMP rs232 Male connector                            |
| 1       | J9                                                                   | 16 pins male header (LPC bus)                       |
| 5       | R1,R7,R10,R11,R12                                                    | 10Kohm smd resistor                                 |
| 7       | C10,C12,C13,C16,C17,C36,C37                                          | Bigger, probably NOT all the same caps              |
| 17      | C1,C2,C9,C15,C1?(8?),C21,C22,C23,C24,C25,C26,C28,C31,C32,C33,C34,C35 | Smaller, also, asuming not all the same             |

The connections in the following tables are checked with the continuity
test on a VOM(Multimeter). but for now here are the listings of wich pin
goes where:

### U1 SMsC LPC ic

Still lots to fill out here, but all connections required for kernel
debugging via null-modem cable tied to TX/RX/GND have been accounted
for.

| | Pin | Name            | Type   | Description                   | Termination(s)            | Notes                                                                                                                           |
|-------|-----------------|--------|-------------------------------|---------------------------|---------------------------------------------------------------------------------------------------------------------------------|
| 6     | CLKI32          | Input  | 32.768kHz trickle clock input | GND (C17)                 | Disabled via GND                                                                                                                |
| 7     | VSS             | Power  | Ground                        | GND (C17)                 |                                                                                                                                 |
| 18    | VTR             | Power  | 3.3V standby voltage          | C17 bypass to 3.3V        | Connected to VCC because no wakeup functionality is required                                                                    |
| 19    | CLOCKI          | Input  | 14.318MHz clock input         | Y2 3                      |                                                                                                                                 |
| 20    | LAD0            | I/O    | LPC address/data bus          | J9 11                     | Multiplexed command, address and data bus.                                                                                      |
| 21    | LAD1            | I/O    | LPC address/data bus          | J9 10                     | Multiplexed command, address and data bus.                                                                                      |
| 22    | LAD2            | I/O    | LPC address/data bus          | J9 8                      | Multiplexed command, address and data bus.                                                                                      |
| 23    | LAD3            | I/O    | LPC address/data bus          | J9 7                      | Multiplexed command, address and data bus.                                                                                      |
| 24    | LFRAME\#        | Input  | Frame signal                  | J9 3                      | Indicates start of a new cycle and termination of broken cycle.                                                                 |
| 26    | PCI\_RESET\#    | Input  | PCI reset                     | J9 5                      | Used as LPC interface reset.                                                                                                    |
| 27    | LPCPD\#         | Input  | Power down signal             | R7 (10k) pull-up to 3.3V  | Indicates that it should prepare for power to be shut down on the LPC interface. Tied high relying on PCI\_RESET to do its job. |
| 29    | PCI\_CLK        | Input  | PCI clock                     | J9 1                      |                                                                                                                                 |
| 30    | SER\_IRQ        | Output | Serial interrupt requests     | J9 16                     | Provides a serial interrupt request line for mouse/keyboard implementations of the IO board.                                    |
| 31    | VSS             | Power  | Ground                        | GND                       |                                                                                                                                 |
| 32    | GP10/J1B1       |        |                               | Trace open?               |                                                                                                                                 |
| 33    | GP11/J1B2       |        |                               | Trace open?               |                                                                                                                                 |
| 34    | GP12/J2B1       |        |                               | Trace open?               |                                                                                                                                 |
| 37    | GP15/J1Y        |        |                               | Trace open?               |                                                                                                                                 |
| 38    | GP16/J2X        |        |                               | Trace open?               |                                                                                                                                 |
| 39    | GP17/J2Y        |        |                               | Trace open?               |                                                                                                                                 |
| 40    | AVSS            | Power  | Analog ground                 | GND                       |                                                                                                                                 |
| 42    | GP21/P16/nDS1   |        |                               | Trace open?               |                                                                                                                                 |
| 44    | VREF            | Power  | 3.3V reference voltage        | C2 bypass to 3.3V         | 5V or 3.3V used for the game port logic                                                                                         |
| 45    | GP24/SYSOPT     |        |                               | R1 (10k) pull-down to GND | Set the configuration base I/O address of 0x2E (R2 would be a pullup to 3.3V with a different configuration address)            |
| 47    | GP26/MIDI\_OUT  |        |                               | Trace open?               |                                                                                                                                 |
| 49    | GP61/LED2       |        |                               | Trace open?               |                                                                                                                                 |
| 51    | GP30/FAN\_TACH2 |        |                               | Trace open?               |                                                                                                                                 |
| 53    | VCC             | Power  | 3.3V supply voltage           | C1 bypass to 3.3V         |                                                                                                                                 |
| 55    | P33/FAN1        |        |                               | Trace open?               |                                                                                                                                 |
| 57    | KCLK            |        |                               | Trace open?               |                                                                                                                                 |
| 59    | MCLK            |        |                               | Trace open?               |                                                                                                                                 |
| 60    | VSS             | Power  | Ground                        | GND                       |                                                                                                                                 |
| 61    | IRRX2/GP34      |        |                               | Trace open?               |                                                                                                                                 |
| 65    | VCC             | Power  | 3.3V supply voltage           | C9 bypass to 3.3V         |                                                                                                                                 |
| 76    | VSS             | Power  | Ground                        | GND (C10)                 |                                                                                                                                 |
| 84    | RXD1            | Input  |                               | U3 8 U2 18                |                                                                                                                                 |
| 85    | TXD1            | Output |                               | U3 6 U2 17                |                                                                                                                                 |
| 86    | nDSR1           | Input  | Data set ready                | U3 ?                      | Optional flow control                                                                                                           |
| 87    | nRTS1           | Output | Request to send               | U3 ?                      | Optional flow control                                                                                                           |
| 93    | VCC             | Power  |                               | C21 bypass to 3.3V        |                                                                                                                                 |
| 95    | RXD2            | Input  |                               | Trace open?               |                                                                                                                                 |
| 96    | TXD2            | Output |                               | Trace hidden under IC?    |                                                                                                                                 |
| 97    | nDSR2           | Input  | Data set ready                | Trace open?               | Optional flow control                                                                                                           |
| 98    | nRTS2           | Output | Request to send               | Trace hidden under IC?    | Optional flow control                                                                                                           |
| 101   | HVSS            | Power  |                               | GND                       |                                                                                                                                 |
| 102   | HVCC            | Power  |                               | C24 bypass to 3.3V        |                                                                                                                                 |
| 103   | SDA             | I/O    |                               | J9 14                     |                                                                                                                                 |
| 104   | SCLK            | I/O    |                               | J9 13                     |                                                                                                                                 |
| 105   | A0/RESET\#      |        |                               | R10 (10k) pull-up to 3.3V |                                                                                                                                 |
| 111   | HVCC            | Power  |                               | C32 bypass to 3.3V        |                                                                                                                                 |
| 112   | HVSS            | Power  |                               | GND                       |                                                                                                                                 |
| 121   | HVCC            | Power  |                               | C33 bypass to 3.3V        |                                                                                                                                 |
| 122   | HVCC            | Power  |                               | C34 bypass to 3.3V        |                                                                                                                                 |
| 125   | HVSS            | Power  |                               | GND                       |                                                                                                                                 |
| 126   | HVSS            | Power  |                               | GND                       |                                                                                                                                 |
| 127   | HVSS            | Power  |                               | GND                       |                                                                                                                                 |
| 128   | HVSS            | Power  |                               | GND                       |                                                                                                                                 |

### J9 LPC header

NOTE: version 1.3+ motherboards are missing the LFRAME signal which will
need to be generated by an external CPLD
[1](https://www.reddit.com/r/originalxbox/comments/7uo3lq/got_an_xbox_for_free_dvd_tray_wont_stop_ejecting/dtmqmn3/)

NOTE: version 1.5 motherboards are missing pins 2 (GND) and 9 (VCC3),
and pins 12 (GND) and 15 (VCC3) haven't been confirmed to work correctly

| | Pin | Name        | Type   | Description               | Termination(s)    | Notes                                                                                        |
|-------|-------------|--------|---------------------------|-------------------|----------------------------------------------------------------------------------------------|
| 1     | LCLK        | Output | PCI clock                 | U1 29             |                                                                                              |
| 2     | VSS         | Power  | System ground             | (see other parts) |                                                                                              |
| 3     | LFRAME\#    | Output | Frame signal              | U1 24             | Indicates start of a new cycle and termination of broken cycle.                              |
| 4     | ---         | ---    | ---                       | ---               | Voided to ensure correct ribbon cable alignment.                                             |
| 5     | LRST\#      | Output | PCI reset                 | U1 26             | Used as LPC interface reset.                                                                 |
| 6     | VCC5        | Power  | 5V power supply           | (see other parts) | C12(e), C13(S), C15(S), U3 11                                                                |
| 7     | LAD3        | I/O    | LPC address/data bus      | U1 23             | Multiplexed command, address and data bus.                                                   |
| 8     | LAD2        | I/O    | LPC address/data bus      | U1 22             | Multiplexed command, address and data bus.                                                   |
| 9     | VCC3        | Power  | 3.3V power supply         | (see other parts) |                                                                                              |
| 10    | LAD1        | I/O    | LPC address/data bus      | U1 21             | Multiplexed command, address and data bus.                                                   |
| 11    | LAD0        | I/O    | LPC address/data bus      | U1 20             | Multiplexed command, address and data bus.                                                   |
| 12    | VSS         | Power  | System ground             | (see other parts) |                                                                                              |
| 13    | SCL         | I/O    | SMBus clock signal        | U1 104            |                                                                                              |
| 14    | SDA         | I/O    | SMBus data signal         | U1 103            |                                                                                              |
| 15    | VCC3        | Power  | 3.3V power supply         | (see other parts) | Intended to be used for external IO board                                                    |
| 16    | L\_SER\_IRQ | Input  | Serial interrupt requests | U1 30             | Provides a serial interrupt request line for mouse/keyboard implementations of the IO board. |

Related links
-------------

-   [Super I/O emulation in
    XQEMU](https://github.com/espes/xqemu/blob/xbox/hw/xbox/lpc47m157.c)

(SMBus?)

-   [Images (CC0 License) by Codeasm with detailed shots of the SuperIO
    board](https://imgur.com/a/ROMYa)

