---
title: Power Supply
permalink: wiki/Power_Supply/
layout: wiki
---

It's either a delta electronics power supply or foxlink

On a 1.0 xbox supply, there seems to be 2 types for the US 120V and EU
240V market.

The marks are on the PCB with a check next to them:

-   DPSN-96-AP-1 should be the EU 240V one
-   DPSN-96-AP is the US 120V one

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

The standby voltage powers the [SMC](/wiki/SMC "wikilink") which handles
turning on and off the system. The Xbox supply needs a voltage (3V3?) on
it's power-on line to leave the standby mode. This results in the other
voltages being supplied.

Connector pinout
----------------

### Xbox 1.0 and 1.1

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

