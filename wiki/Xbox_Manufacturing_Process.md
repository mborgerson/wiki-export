---
title: Xbox Manufacturing Process
permalink: wiki/Xbox_Manufacturing_Process/
layout: wiki
---

by *Michael Steil*

There are many different Xboxes. Although they all look the same (except
for the “Special Edition”), and all of them work with all games, they
have been produced in three different factories and may contain
different components.

This article describes the “reverse-engineered” internals of Xbox
manufacturing.

The Serial Number
-----------------

Every Xbox has a sticker on the bottom that looks like this:

<img src="https://web.archive.org/web/20100617013616im_/http://www.xbox-linux.org/pic/serial-sticker.jpg" alt="serial-sticker.jpg" />

It contains the manufacturing date and the 12-digit serial number:

    1166356 20903
    ||    | |||||__
    ||    | ||||___ factory number
    ||    | |||____
    ||    | ||_____ week of year (starting Mondays)
    ||    | |______ last digit of year
    ||    |________
    ||_____________ number of Xbox within week and factory
    |______________ production line within factory 

  
Three factories have produced the Xbox. Mexico (Guadalajara) is “02”,
Hungary (S�rv�r) is “03” and China (Doumen) is “05”.

So this Xbox has been manufactured in week \#9, 2002 in line 1 of the
factory in Hungary, and it was number 166,356 of this week.

Factories
---------

|          |                                                                                                                                                                                                                                 |                                                                                                                                 |                                                                                                                                         |
|----------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|---------------------------------------------------------------------------------------------------------------------------------|-----------------------------------------------------------------------------------------------------------------------------------------|
| **Date** | **Mexico**                                                                                                                                                                                                                      | **Hungary**                                                                                                                     | **China**                                                                                                                               |
| 10/2001  | Production of 110V 1.0 Xboxes with Thomson drives is started for the USA/Canada market.                                                                                                                                         | Production of 110V 1.0 Xboxes with Thomson drives is started for the USA/Canada market.                                         |
| 01/2002  | Production gets extended for Japan.                                                                                                                                                                                             | Complete switch to 220V European/Australian models.                                                                             |
| 04/2002  | Production lines \#1 and \#6 get closed and get moved to China. The other four lines continue production. The first Xboxes with Philips DVD drives are made, now 50% of all Mexican devices have Philips, and 50% have Thomson. |                                                                                                                                 |
| 05/2002  |                                                                                                                                                                                                                                 | Production stops after only less than 9 months, and after about 3 Million Xboxes. All four production lines get moved to China. |
| 08/2002  |                                                                                                                                                                                                                                 |                                                                                                                                 | Production of the 220V 1.1 Xbox for Europe and Australia only begins, with mostly Philips and sometimes Thomson DVD drives.             |
| 09/2002  |                                                                                                                                                                                                                                 |                                                                                                                                 | First Samsung DVD drives. Most Xboxes now contain Samsung devices, a few contain Philips ones, and very few still contain Thomson ones. |
| 10/2002  | Switch the remaining four lines to version 1.1.                                                                                                                                                                                 |                                                                                                                                 |                                                                                                                                         |
| 11/2002  |                                                                                                                                                                                                                                 |                                                                                                                                 | Production gets extended by 110V USA/Canada/Japan models.                                                                               |
| 12/2002  | Production ends after 14 months, and after nearly 7 Million Xboxes.                                                                                                                                                             |                                                                                                                                 | First line changes to version 1.2                                                                                                       |
| 02/2003  |                                                                                                                                                                                                                                 |                                                                                                                                 | Last line changes to version 1.2                                                                                                        |
| 03/2003  |                                                                                                                                                                                                                                 |                                                                                                                                 | First line changes to version 1.3                                                                                                       |
| 04/2003  |                                                                                                                                                                                                                                 |                                                                                                                                 | Last line changes to version 1.3                                                                                                        |
| 07/2003  |                                                                                                                                                                                                                                 |                                                                                                                                 | First line changes to version 1.4                                                                                                       |
| 08/2003  |                                                                                                                                                                                                                                 |                                                                                                                                 | First line changes to version 1.5                                                                                                       |

Look at the
<a href="/web/20100617013616/http://www.xbox-linux.org/wiki/Xbox_Versions_HOWTO" title="Xbox Versions HOWTO">Xbox
Versions HOWTO</a> for details on when which Chinese line switched to a
new version.

Manufacturing Process
---------------------

(Please note that a lot of this information has been concluded from
implicit information, it may contain errors.)

When a computer such as the Xbox is manufactured, three very different
tasks have to be done:

-   assemble the hardware
-   copy the software on the hardware
-   test the device

The most interesting part about this is when the software is copied, and
when and how the device is tested, because there is a lot of room for
optimization.

Not counting the firmware of independent components such as the HD, the
DVD and the SMC, the Xbox contains three pieces of software:

-   The Xbox kernel in Flash ROM
-   The hard disk contents (and hard disk key)
-   The EEPROM contents

It depends on the device whether it is more convenient to program it
before putting it into the Xbox or doing in-system-programming.

### Putting the System together

The Flash ROM chips gets programmed externally with the final Xbox
kernel and then gets soldered onto the Xbox motherboard. The EEPROM is
empty when it gets soldered onto it. So is the hard disk when it gets
connected.

### The Installation CD

So when the Xbox is complete, but the EEPROM and the hard disk are still
empty and the hard disk is not locked yet, an optical media gets
inserted into the DVD drive (this needn't be an Xbox DVD), which
contains a properly retail-signed default.xbe built with the XDK that
has the allow eject flag set and has the region code set to 0x80000000
(“DEBUG”).

When the Xbox kernel initializes, it checksums the EEPROM. If it fails,
the Xbox will be in DEBUG mode, i.e. the region code is set to
0x80000000. With the region code set to this value, the kernel ignores
it if the hard disk is not locked. Because the region code matches, the
kernel will run the executable from CD, which does the following:

-   format the three swap partitions
-   copy XMTAXBOX.XBE from CD to the first cache partition and run it

Then the DVD can be ejected and put into the next Xbox.

### XMTAXBOX.XBE

This XMTAXBOX.XBE is an XBE retail-signed for hard disk, also with the
region code set to 0x80000000, that does the following:

-   retrieve the EEPROM contents from a network server
-   retrieve the contents of the system and data partitions from a
    network server
-   lock the hard disk
-   make some self tests and send the report to the server

A new Xbox still contains the file XMTAXBOX.XBE on the first cache
partition, as well as some temporary files on the third one.

Xbox kernels since version 4034 have another backdoor that even works if
the EEPROM check succeeds: If bit 30 of the media flag of an XBE is set,
the condition of the hard disk is ignored as well. This change allows
Microsoft to replace broken hard disks without replacing a valid EEPROM
on an Xbox sent in for repair. Before this change, they had to rewrite
the EEPROM whenever they replaced the drive.

<img src="https://web.archive.org/web/20100617013616im_/http://www.xbox-linux.org/pic/repairdvd.jpg" alt="repairdvd.jpg" />
<img src="https://web.archive.org/web/20100617013616im_/http://www.xbox-linux.org/pic/xboxqc.jpg" alt="xboxqc.jpg" />
[1](https://web.archive.org/web/20100617013616/http://www.amcham.hu/BusinessHungary/16-01/articles/16-01_26.asp)

Further Reading
---------------

-   [Xbox Manufacturing Process
    Pictures](https://web.archive.org/web/20100617013616/http://www.xbox-linux.org/wiki/Xbox_Manufacturing_Process_Pictures)
-   [2](https://web.archive.org/web/20100617013616/http://www.ifm.eng.cam.ac.uk/ctm/idm/cases/xbox.html)
    Innovation and Design Management case study
-   O'Brien, J. [The making of the
    Xbox](https://web.archive.org/web/20100617013616/http://www.wired.com/wired/archive/9.11/flex_pr.html),
    Wired, Issue 9.11, November 2001
-   Shah, J. and Serant, C. [Microsoft's Xbox sets supply chain
    standard](https://web.archive.org/web/20100617013616/http://www.ebnonline.com/story/OEG20020311S0076),
    EBN Online, 11 March 2002
-   Olavsrud, T. [Flextronics relocates Xbox manufacturing
    facility](https://web.archive.org/web/20100617013616/http://www.internetnews.com/bus-news/article.php/1129171),
    Internetnews.com, 15 May 2002
-   Penz, B. [Game Over? Xbox production heads
    east](https://web.archive.org/web/20100617013616/http://www.amcham.hu/BusinessHungary/16-06/articles/16-06_27.asp),
    Business Hungary, Vol 16 No 6, June 2002
-   Carbone, J. [Outsourcing the
    Xbox](https://web.archive.org/web/20100617013616/http://manufacturing.net/pur/article/CA237778?stt=001&pubdate=08%2F15%2F02),
    Purchasing Magazine Online, 15 August 2002
-   Neilley, R: [Molding is big man on this
    campus](https://web.archive.org/web/20100617013616/http://www.immnet.com/articles?article=1763)

