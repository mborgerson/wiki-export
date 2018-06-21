---
title: AV Cables
permalink: wiki/AV_Cables/
layout: wiki
---

Microsoft has released a handful of **Audio Video Cables** for the
[Xbox](/wiki/Xbox "wikilink"), which they refer to internally as **AV Packs**.
They are connected to the port labelled “Audio Video Input/Output” on
the backside of the console.

Connector
---------

### Pinout

The following table gives the pinout in the way it is are arranged on
the **XBOX AVIP** cables, or **AV Packs**. The other end is reversed and
on the console.

| Description | Audio Right | Audio Right GND | SPDIF Digital Audio | V-Sync (VGA Mode) | Mode GND      | Mode GND      | Mode GND      | GND    | Variable | 9 GND    | Variable | 11 GND   |
|-------------|-------------|-----------------|---------------------|-------------------|---------------|---------------|---------------|--------|----------|----------|----------|----------|
| Pin         | 1           | 2               | 3                   | 4                 | 5             | 6             | 7             | 8      | 9        | 10       | 11       | 12       |
| Pin         | 13          | 14              | 15                  | 16                | 17            | 18            | 19            | 20     | 21       | 22       | 23       | 24       |
| Description | Vcc         | Audio Left      | Audio Left GND      | H Sync (VGA Mode) | Mode Select 1 | Mode Select 2 | Mode Select 3 | (+12V) | 22 GND   | Variable | 24 GND   | Variable |

Supported signals / AV cables
-----------------------------

Below is a table which lists all known officially **AV cables** released
by Microsoft and their respective signal. The constants used in the
av-prefixed Kernel-functions are also listed.

| Pin 19 | Pin 18 | Pin 17 | SMC Constant | Region     | Official Microsoft product name                                                                                                      | Signal                    | Pin 24 | Pin 22 | Pin 11 | Pin 9 | Kernel av-Constant     |
|--------|--------|--------|--------------|------------|--------------------------------------------------------------------------------------------------------------------------------------|---------------------------|--------|--------|--------|-------|------------------------|
| 0      | 0      | 0      | 0            | PAL        | [Advanced SCART Cable](https://web.archive.org/web/20040216131316/http://www.xbox.com:80/en-gb/hardware/scartcable.htm)              | SCART (15 kHz RGB) Stereo | V      | R      | G      | B     | AV\_PACK\_SCART = 3    |
| 0      | 0      | 1      | 1            | NTSC       | [High Definition AV Pack](https://web.archive.org/web/20040210040422/http://www.xbox.com:80/en-US/hardware/highdefinitionavpack.htm) | Component SPDIF           | -      | Pr     | Y      | Pb    | AV\_PACK\_HDTV = 4     |
| 0      | 1      | 0      | 2            | -          | *Not officially available / supported*                                                                                               | VGA (31 kHz RGB) Stereo   | -      | R      | G      | B     | AV\_PACK\_VGA = 5      |
| 0      | 1      | 1      | 3            | NTSC / PAL | [RF Adapter](https://web.archive.org/web/20040319001330/http://www.xbox.com:80/en-us/hardware/rfadapter.htm)                         | RF Mono                   | V      | C      | Y      | -     | AV\_PACK\_RFU = 2      |
| 1      | 0      | 0      | 4            | NTSC       | [Advanced AV Pack](https://web.archive.org/web/20040319001247/http://www.xbox.com:80/en-us/hardware/advancedavpack.htm)              | S-Video Stereo            | V      | C      | Y      | -     | AV\_PACK\_SVIDEO = 6   |
| 1      | 0      | 1      | 5            | -          | *Not officially available / supported*                                                                                               | Unknown Mono              | V      | C      | Y      | -     | AV\_PACK\_NONE = 0     |
| 1      | 1      | 0      | 6            | PAL        | [Standard AV Cable](https://web.archive.org/web/20040216130745/http://www.xbox.com:80/en-gb/hardware/avcable.htm)                    | Composite Stereo          | V      | C      | Y      | -     | AV\_PACK\_STANDARD = 1 |
| 1      | 1      | 1      | 7            | -          | *No cable connected*                                                                                                                 | -                         | -      | -      | -      | -     | AV\_PACK\_NONE = 0     |

A signal value of 0 means connection to ground (Pins 7, 6, 5), whereas a
value of 1 means an open-connection

The region only identifies which territory the cable was originally
available / intended for. The kernel still might support other
video-standards (NTSC / PAL / SECAM) using the same cable.

Related links
-------------

-   [GamesX List of signals and
    pinout](http://www.gamesx.com/avpinouts/xbox.htm)
-   [ASSEMBLERgames XBOX AV
    pinout](https://assemblergames.com/attachments/xboxavippinouttr0-png.13081/)
-   [uCON64
    Connectors](http://ucon64.sourceforge.net/ucon64misc/conn.html)

