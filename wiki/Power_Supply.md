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

-   MODEL: FTPS-0001 REV:B (120V)
-   MODEL: FTPS-0002 REV:B (from a 2002 PAL (240V))
-   MODEL: FTPS-0002 REV:H (from a 2003 PAL (240V))
-   MODEL: FTPS-0002 REV:G (240V)
-   MODEL: FTPS-0007 REV:B (120V) (are these after the powercord
    recall?)
-   MODEL: FTPS-0007 REV:D (120V) (are these after the powercord
    recall?)

The rated output currents are also noted on the sticker

-   96W Max output power
-   3V3 Standby 0.045A
-   3V3 4.8A
-   5V 13.2A
-   12V 1.2A

#### Powercord recall

Microsoft started an powercord recall, or more like a replacement
program
[xbox.com](https://web.archive.org/web/20050301093947/http://www.xbox.com:80/en-US/news/0502/powercordannouncement.htm)for
what apeared to be Foxlink powersupplies having a bad powerplug
connector on the back of the console and where installed in 1.0 and 1.1
Xboxes.. Microsoft tried to resolve this by offering free replacement
powercords and advising to trow out the old ones. [More to the power
cord replacement than meets the
eye?](https://web.archive.org/web/20120722175134/http://www.xbox-scene.com/xbox1data/sep/EEpAEAylAluZlwSlOJ.php).

From the Microsoft powercord replacement FAQ:

  
**Does my console require a replacement cord?**

If it was manufactured before October 23, 2003, your console requires a
replacement cord (except for consoles purchased in Continental Europe,
where consoles manufactured prior to January 13, 2004 require a
replacement cord). Consoles manufactured after October 23, 2003 (after
January 13, 2004 for consoles purchased in Continental Europe) do not
require replacement cords because design improvements to the cord and
console already protect against the problems that are addressed by the
replacement
cords.[1](https://web.archive.org/web/20050223041403/http://replacements.webprogram.com:80/en-us/faqs.asp#Q-7)

The old page describing how to order has been archived here:
[2](https://web.archive.org/web/20050223060900/http://replacements.webprogram.com:80/en-us/programoverview.asp)
One would either recieve a thicker but very similiar powercord or an
actual [GFI](https://simple.wikipedia.org/wiki/GFCI) (Ground Fault
Circuit Interupter). a online form would then be used to determine wich
type of cable you recieve by means of serial number.

##### AFCI

A video of whats inside an european (UK) “AFCI”
[3](https://www.youtube.com/watch?v=C0wQikAO-yA)

An european AFCI ordered from MS for free had the following on the
bottom label.

|             |                          |
|-------------|--------------------------|
| Part number | X800925-100              |
|             | 200-240V~,50/60HZ, 610mA |
| MFG Code    | DN0855                   |

The cable that is wired to the AFCI has the following markings:

| top    | bottom  |
|--------|---------|
| JI-HAW | JHT-031 |

##### Old and New cords

The replacement cable has the following markings:

| top     | bottom |
|---------|--------|
| JHT-031 |        |

The older cable that needed replacement was:

| top      | bottom    |
|----------|-----------|
| NITTO SS | W41-27854 |
| JHT-013  | N15905    |
| JI-HAW   |           |
| E147422  |           |

Presumely this one doesnt come with tighter tolerances.

### Samsung “TUSCANY”

Found in 1.6 xboxes and seems to be made by Samsung (or atleast the main
transformer is, wich also has the detail label on it)

-   PSCD101301A

### Minebea

-   MS001A096EMJ

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
-   [Powercord recall blog with exacter cable
    numbers](https://web.archive.org/web/20060421203325/https://msmvps.com/blogs/matthewsoft/archive/2005/02/17/36238.aspx)

