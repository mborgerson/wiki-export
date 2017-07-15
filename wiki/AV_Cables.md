---
title: AV Cables
permalink: wiki/AV_Cables/
layout: wiki
---

Microsoft released a handful of AV packs for the Xbox. They are
connected to the port labelled “Audio Video Input/Output” on the
backside of the console.

Connector
---------

### Pinout

| Pin | Description         |
|-----|---------------------|
| 1   | Audio Right         |
| 2   | Audio Right GND     |
| 3   | SPDIF Digital Audio |
| 4   | V-Sync (VGA Mode)   |
| 5   | Mode GND            |
| 6   | Mode GND            |
| 7   | Mode GND            |
| 8   | GND                 |
| 9   | Variable            |
| 10  | Pin 9 GND           |
| 11  | Variable            |
| 12  | Pin 11 GND          |

| Pin | Description       |
|-----|-------------------|
| 13  | Vcc               |
| 14  | Audio Left        |
| 15  | Audio Left GND    |
| 16  | H Sync (VGA Mode) |
| 17  | Mode Select 1     |
| 18  | Mode Select 2     |
| 19  | Mode Select 3     |
| 20  | +12V              |
| 21  | Pin 22 GND        |
| 22  | Variable          |
| 23  | Pin 24 GND        |
| 24  | Variable          |

Supported signals / AV cables
-----------------------------

Below is a table which lists all known officially AV cables released by
Microsoft and their respective signal. The constants used in the
av-prefixed Kernel-functions are also listed.

| 19  | 18  | 17  | Official Microsoft product name        | Signal             | 24  | 22  | 11  | 9   | Kernel av-Constant     |
|-----|-----|-----|----------------------------------------|--------------------|-----|-----|-----|-----|------------------------|
| 0   | 0   | 0   | Advanced SCART Cable                   | SCART (15 kHz RGB) | V   | R   | G   | B   | AV\_PACK\_SCART = 3    |
| 0   | 0   | 1   | High Definition AV Pack                | Component          | -   | Pr  | Y   | Pb  | AV\_PACK\_HDTV = 4     |
| 0   | 1   | 0   | *Not officially available / supported* | VGA (31 kHz RGB)   | -   | R   | G   | B   | AV\_PACK\_VGA = 5      |
| 0   | 1   | 1   | RF Adapter                             | NTSC Mono          | V   | C   | Y   | -   | AV\_PACK\_RFU = 2      |
| 1   | 0   | 0   | Advanced AV Pack                       | PAL Stereo         | V   | C   | Y   | -   | AV\_PACK\_SVIDEO = 6   |
| 1   | 0   | 1   | *Not officially available / supported* | PAL/Secam Mono     | V   | C   | Y   | -   | AV\_PACK\_NONE = 0     |
| 1   | 1   | 0   | Standard AV Cable                      | NTSC Stereo        | V   | C   | Y   | -   | AV\_PACK\_STANDARD = 1 |
| 1   | 1   | 1   | *No cable connected*                   | -                  | -   | -   | -   | -   | AV\_PACK\_NONE = 0     |

A signal value of 0 means connection to ground (Pins 7, 6, 5), whereas a
value of 1 means an open-connection

Related links
-------------

-   [List of signals and
    pinout](http://www.gamesx.com/avpinouts/xbox.htm)

