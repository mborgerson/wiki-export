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

| 19  | 18  | 17  | SMC Constant | Region      | Official Microsoft product name                                                                                                      | Signal                    | 24  | 22  | 11  | 9   | Kernel av-Constant     |
|-----|-----|-----|--------------|-------------|--------------------------------------------------------------------------------------------------------------------------------------|---------------------------|-----|-----|-----|-----|------------------------|
| 0   | 0   | 0   | 0            | PAL         | [Advanced SCART Cable](https://web.archive.org/web/20040216131316/http://www.xbox.com:80/en-gb/hardware/scartcable.htm)              | SCART (15 kHz RGB) Stereo | V   | R   | G   | B   | AV\_PACK\_SCART = 3    |
| 0   | 0   | 1   | 1            | NTSC        | [High Definition AV Pack](https://web.archive.org/web/20040210040422/http://www.xbox.com:80/en-US/hardware/highdefinitionavpack.htm) | Component SPDIF           | -   | Pr  | Y   | Pb  | AV\_PACK\_HDTV = 4     |
| 0   | 1   | 0   | 2            | -           | *Not officially available / supported*                                                                                               | VGA (31 kHz RGB) Stereo   | -   | R   | G   | B   | AV\_PACK\_VGA = 5      |
| 0   | 1   | 1   | 3            | NTSC / PAL  | [RF Adapter](https://web.archive.org/web/20040319001330/http://www.xbox.com:80/en-us/hardware/rfadapter.htm)                         | RF Mono                   | V   | C   | Y   | -   | AV\_PACK\_RFU = 2      |
| 1   | 0   | 0   | 4            | NTSC        | [Advanced AV Pack](https://web.archive.org/web/20040319001247/http://www.xbox.com:80/en-us/hardware/advancedavpack.htm)              | S-Video Stereo            | V   | C   | Y   | -   | AV\_PACK\_SVIDEO = 6   |
| 1   | 0   | 1   | 5            | PAL / SECAM | *Not officially available / supported*                                                                                               | RF Mono                   | V   | C   | Y   | -   | AV\_PACK\_NONE = 0     |
| 1   | 1   | 0   | 6            | PAL         | [Standard AV Cable](https://web.archive.org/web/20040216130745/http://www.xbox.com:80/en-gb/hardware/avcable.htm)                    | Composite Stereo          | V   | C   | Y   | -   | AV\_PACK\_STANDARD = 1 |
| 1   | 1   | 1   | 7            | -           | *No cable connected*                                                                                                                 | -                         | -   | -   | -   | -   | AV\_PACK\_NONE = 0     |

A signal value of 0 means connection to ground (Pins 7, 6, 5), whereas a
value of 1 means an open-connection

The region only identifies which territory the cable was originally
available / intended for. The kernel still might support other
video-standards (NTSC / PAL / SECAM) using the same cable.

Related links
-------------

-   [List of signals and
    pinout](http://www.gamesx.com/avpinouts/xbox.htm)

