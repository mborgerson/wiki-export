---
title: Manufacturing Process
permalink: wiki/Manufacturing_Process/
layout: wiki
---

![Sárvár, Hungary
factory](Manufacturing26.jpg "Sárvár, Hungary factory")

The Xbox was manufactured in four locations throughout its lifetime:
Guadalajara, Mexico; Sárvár, Hungary; Doumen, China; and Taiwan.

Serial Number
-------------

![An example of a serial number
sticker](Serial-sticker.jpg "An example of a serial number sticker")

Every Xbox has a sticker on the bottom with a manufacturing date and a
12-digit serial number. The serial number encodes information about
where and when the hardware was manufactured.

Using the pictured serial number as an example:

| Line | Number | Year | Week | Factory |
|------|--------|------|------|---------|
| 1    | 166356 | 2    | 09   | 03      |

The first field is the factory production line number. The second field
is the Xbox's number for the week (for the whole factory, not just the
one line). The third field is the last digit of the year. The fourth
field is the week of the year. Finally, the fifth field is the factory
number. Mexico is 02, Hungary is 03, China is 05, and Taiwan is 06.

So this Xbox was the 166,356<sup>th</sup> to be manufactured in week 9
of the year 2002 on production line 1 in Sárvár, Hungary.

Timeline
--------

|           | Mexico                                                                                              | Hungary                                                                            | China                                                                                                              |
|-----------|-----------------------------------------------------------------------------------------------------|------------------------------------------------------------------------------------|--------------------------------------------------------------------------------------------------------------------|
| 2001      |
| October   | Production of 110V 1.0 Xboxes with Thomson DVD drives begins for USA/Canada.                        |                                                                                    |
| 2002      |
| January   | Production begins for Japan.                                                                        | Production switches to 220V 1.0 models for Europe/Australia.                       |                                                                                                                    |
| April     | Production lines 1 and 6 are moved to China. 50% of new units now produced with Philips DVD drives. |                                                                                    |                                                                                                                    |
| May       |                                                                                                     | Production stops after ~3 million units. All four production lines moved to China. |                                                                                                                    |
| August    |                                                                                                     |                                                                                    | Production of the 220V 1.1 Xbox for Europe/Australia begins, with mostly Philips and sometimes Thomson DVD drives. |
| September |                                                                                                     |                                                                                    | Most units now use Samsung DVD drives.                                                                             |
| October   | Remaining four lines switched to version 1.1.                                                       |                                                                                    |                                                                                                                    |
| November  |                                                                                                     |                                                                                    | Production extended to 110V USA/Canada/Japan models.                                                               |
| December  | Production ends after ~7 million units.                                                             |                                                                                    | First line changes to version 1.2.                                                                                 |
| 2003      |
| February  |                                                                                                     |                                                                                    | Last line changes to version 1.2.                                                                                  |
| March     |                                                                                                     |                                                                                    | First line changes to version 1.3.                                                                                 |
| April     |                                                                                                     |                                                                                    | Last line changes to version 1.3.                                                                                  |
| July      |                                                                                                     |                                                                                    | First line changes to version 1.4.                                                                                 |
| August    |                                                                                                     |                                                                                    | First line changes to version 1.5.                                                                                 |
| 2004      |
| ?         |                                                                                                     |                                                                                    | ?                                                                                                                  |

Hardware assembly
-----------------

The Xbox was manufactured just like many other mass-produced electronic
products. Printed circuit boards were made in bulk and populated by
pick-and-place machines, the plastic case was formed by injection
molding, and finally all of the pieces were assembled by hand on
production lines.

The overall process didn't vary much between hardware revisions. The
only notable change was related to the BIOS ROM. Up until version 1.6,
the BIOS was flashed onto a TSOP ROM before being soldered to the
motherboard. Revision 1.6 introduced the Xyclops chip, which integrated
both the [System Management
Controller](/wiki/System_Management_Controller "wikilink") and the BIOS ROM on
a single die. This meant the BIOS had to be flashed by the chip
manufacturer instead of on the Xbox production line.

<File:Manufacturing19.jpg%7CPlastic> case molding & assembly.
<File:Manufacturing18.jpg%7CLids> <File:Manufacturing9.jpg%7CGreen>
jewels <File:Manufacturing10.jpg%7CGreen> jewels
<File:Manufacturing16.jpg%7CJewels> are dispensed from hoppers.
<File:Manufacturing7.jpg%7CLid> shield is positioned.
<File:Manufacturing3.jpg%7CLid> is placed over shield.
<File:Manufacturing5.jpg%7CJewel> is aligned...
<File:Manufacturing4.jpg>|...and then applied.
<File:Manufacturing6.jpg%7CThe> tape is left on to prevent scratches.
<File:Manufacturing15.jpg%7CShield> and drive caddies are installed in
the bottom half. <File:Manufacturing20.jpg%7CAssembled> cases are placed
on pallets. <File:Manufacturing11.jpg> <File:Manufacturing12.jpg>
<File:Manufacturing13.jpg> <File:Manufacturing14.jpg%7CCases> ready for
their electronics. <File:Manufacturing25.jpg%7CComponents> arrive in
bulk at the assembly line. <File:Manufacturing26.jpg>
<File:Manufacturing24.jpg%7CTechnicians> assemble components.
<File:Manufacturing23.jpg> <File:Manufacturing22.jpg>
<File:Manufacturing27.jpg> <File:Manufacturing28.jpg>
<File:Manufacturing21.jpg%7CFully> assembled consoles are placed on
carts, ready for initialization.

Software initialization
-----------------------

When a fully-assembled Xbox reached the end of the production line, it
had a:

-   valid BIOS image
-   blank EEPROM
-   blank, unlocked hard disk

To initialize the EEPROM and hard disk in the final stage of production,
a technician would insert a DVD containing a setup program. The setup
program, named default.xbe just like any other primary Xbox executable,
was built by the XDK with the “allow eject” flag set and the region code
set to 0x80000000 (“DEBUG”).

When the Xbox kernel initializes, it checksums the EEPROM. If it fails,
the Xbox will be in DEBUG mode, i.e. the region code is set to
0x80000000. With the region code set to this value, the kernel ignores
it if the hard disk is not locked. Because the region code matches, the
kernel will run the executable from the DVD, which does the following:

1.  Format the three swap partitions
2.  Copy XMTAXBOX.XBE from the DVD to the first cache partition and run
    it

Then the DVD can be ejected and put into the next Xbox.

### XMTAXBOX.XBE

This XMTAXBOX.XBE is an XBE retail-signed for hard disk, also with the
region code set to 0x80000000, that does the following:

1.  Retrieve the EEPROM contents from a network server
2.  Retrieve the contents of the system and data partitions from a
    network server
3.  Lock the hard disk
4.  Run some self tests and send a report to the server

A new Xbox still contains the file XMTAXBOX.XBE on the first cache
partition, as well as some temporary files on the third one.

Xbox kernels since version 4034 have another backdoor that even works if
the EEPROM check succeeds: If bit 30 of the media flag of an XBE is set,
the condition of the hard disk is ignored as well. This change allows
Microsoft to replace broken hard disks without replacing a valid EEPROM
on an Xbox sent in for repair. Before this change, they had to rewrite
the EEPROM whenever they replaced the drive.

<File:Manufacturing30.jpg%7CConsoles> are initialized by setup program.
<File:Manufacturing33.jpg%7CInitialized> consoles undergo final
integration testing. <File:Manufacturing34.jpg>
<File:Manufacturing2.jpg>

Packing and shipping
--------------------

<File:Manufacturing17.jpg%7CStyrofoam> inserts are prepared.
<File:Manufacturing31.jpg%7CConsoles> are packed into boxes.
<File:Manufacturing32.jpg%7CBoxes> are sealed...
<File:Manufacturing29.jpg>|...and loaded on pallets, ready for shipping.

Credits
-------

This article is adapted from [Xbox Manufacturing
Process](https://web.archive.org/web/20100617013616/http://www.xbox-linux.org/wiki/Xbox_Manufacturing_Process)
by Michael Steil. Its content is reproduced here under the [GNU Free
Documentation License
1.2](https://www.gnu.org/licenses/old-licenses/fdl-1.2.en.html).

Production line photos are from [Andy Rostad's
gallery](https://web.archive.org/web/20100608022830/http://andy.saturn9.ws:80/Photo%20Albums/Xbox%20Manufacturing%20Facility/).
