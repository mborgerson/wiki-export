---
title: Power Supply
permalink: wiki/Power_Supply/
layout: wiki
---

It's either a Foxlink or Delta Electronics power supply for most xboxes.
Some are rumoured to be shipped with a Minebea brand and 1.6 xboxes are
found with a “tuscany” made probably by Samsung.

On a 1.0 xbox supply, there seems to be 2 types for the US 120V and EU
240V market.

The standby voltage powers the [SMC](/wiki/SMC "wikilink") which handles
turning on and off the system. The Xbox supply needs a voltage (3V3?) on
it's power-on line to leave the standby mode. This results in the other
voltages being supplied.

### Delta Electronics

The marks are on the PCB with a check next to them:

-   Delta Electronics DPSN-96-AP-1 should be the EU 240V one
-   Delta Electronics DPSN-96-AP is the US 120V one

There is a link J11 under CR1 (right side of the coil under it) that
might make the difference between 120V and 240V. It also seems that the
Bridge rectifier is bigger on the US models. This could make sense as it
has to handle double the current. So, before experimenting with this it
might be worth to check if the installed type can handle 2.3 amps.

The rated output currents are also noted on the PCB

-   3V3 Standby 0.075A
-   3V3 4.8A
-   5V 13.2A
-   12V 1.2A

### Foxlink Technology LTD

On the lower heatsink, there is a sticker with the following markings
and versions:

-   MODEL: FTPS-0001 REV:B {citation needed} (120V)
-   MODEL: FTPS-0002 REV:B (from a 2002 PAL (240V))
-   MODEL: FTPS-0002 REV:H (from a 2003 PAL (240V))
-   MODEL: FTPS-0002 REV:G {citation needed} (240V)
-   MODEL: FTPS-0007 REV:B {citation needed} (120V) (are these after the
    powercord recall?)
-   MODEL: FTPS-0007 REV:D {citation needed} (120V) (are these after the
    powercord recall?)

The rated output currents are also noted on the sticker

-   96W Max output power
-   3V3 Standby 0.045A
-   3V3 4.8A
-   5V 13.2A
-   12V 1.2A

#### power cord recall

According to an older Xbox-scene article, Foxlink powersupplies tend to
have a bad powerplug connector on the back and where installed in 1.0
and 1.1 xboxes.{citation needed}{citation needed}. Microsoft tried to
resolve this by offering free replacement powercords and advising to
trow out the old ones. [More to the power cord replacement than meets
the
eye?](https://web.archive.org/web/20120722175134/http://www.xbox-scene.com/xbox1data/sep/EEpAEAylAluZlwSlOJ.php).
One would either recieve a thicker but very similiar powercord or an
actual [GFI](https://simple.wikipedia.org/wiki/GFCI) (Ground Fault
Circuit Interupter). a online form would then be used to determine wich
type of cable you recieve by means of serial number.

### Samsung “TUSCANy”

Found in 1.6 xboxes and seems to be made by Samsung (or atleast the main
transformer is, wich also has the detail label on it)

-   PSCD101301A

### Minebea

-   MS001A096EMJ

Connector pinout
----------------

### Xbox 1.0 and 1.1

TUSCANY

| Pin    | Usage       |
|--------|-------------|
| Pin 1  | +12V        |
| Pin 2  | +5V         |
| Pin 3  | +5V         |
| Pin 4  | +5V         |
| Pin 5  | +3.3V       |
| Pin 6  | +3V Standby |
| Pin 7  | GND         |
| Pin 8  | GND         |
| Pin 9  | GND         |
| Pin 10 | GND         |
| Pin 11 | POWON       |
| Pin 12 | POWOK       |

### Xbox 1.2 and later

| Pin    | Usage       |
|--------|-------------|
| Pin 1  | +5V         |
| Pin 2  | +5V         |
| Pin 3  | +5V         |
| Pin 4  | None        |
| Pin 5  | GND         |
| Pin 6  | None        |
| Pin 7  | +3.3V       |
| Pin 8  | None        |
| Pin 9  | GND         |
| Pin 10 | POWOK       |
| Pin 11 | +12V        |
| Pin 12 | None        |
| Pin 13 | +5V         |
| Pin 14 | GND         |
| Pin 15 | +3V Standby |
| Pin 16 | GND         |
| Pin 17 | None        |
| Pin 18 | +3.3V       |
| Pin 19 | GND         |
| Pin 20 | POWON       |

Related links
-------------

-   [Describes how to turn an ATX Power-Supply to Xbox
    PSU](http://brandonw.net/consoles/xbox/)

