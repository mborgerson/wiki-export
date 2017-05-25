---
title: Xbox Game Disc
permalink: wiki/Xbox_Game_Disc/
layout: wiki
---

Xbox games are shipped on DVDs. They are commonly referred to as Xbox
Game Discs (XGD).

Visible information on ring
---------------------------

**The DVD inner ring usually contains:**

(The examples are from a German [FIFA Soccer
2003](/wiki/FIFA_Soccer_2003 "wikilink") disc)

An outer portion with labels:

-   Outer ring Layer 1
    -   Code 39 Barcode of the the Mastering Code (\*EA02302E L1\*)
    -   Text for Mastering code (“EA02302E L1 02 0MM”, where “02” is a
        smaller font and slightly higher than the previous baseline,
        followed by “0MM” on the original baseline)
    -   Mastering SID Code (“IFPI L126”)
-   Inner ring for Layer 0
    -   Code 39 Barcode of the the Mastering Code (\*EA02302E L0\*)
    -   Text for Mastering code (“EA02302E L0 06”, where “06” is a
        smaller font and slightly higher than the previous baseline)
    -   Mastering SID Code (“IFPI L126”)

An inner porition with Xbox logo:

-   3 times “XBOX” text with “X Logo” in the background on each side
-   1 time “XBOX” text with blank background
-   3 times “XBOX” text with “X Logo” in the background on each side
-   Another tiny pattern segmented into 7 portions in alternating
    position,(opposite of the “XBOX” text without logo)
    -   4 times a Xbox logo
    -   2 times the word “genuine”
    -   and in the middel the word ASPnnnn where n is a number

''' ASP code ''' ![Detail of the DVD hologram, reflecting the ASP5080 by
the flash of the camera. found on a demodisk
(IM00113E-IM)](Asp_demodisk.jpg "fig:Detail of the DVD hologram, reflecting the ASP5080 by the flash of the camera. found on a demodisk (IM00113E-IM)")
The following table lists known ASPnnnn numbers found on Xbox dvd disks,
they are also on 360 disks but we dont list those in this wiki. The
games listed are examples, its known for sure more disks can have these
numbers and further research can be done, to determine the meaning. It
is rumoured it might be a version string of some sorts slowly raising in
xbox years old.

| ASP number | found on                                           | game serial |
|------------|----------------------------------------------------|-------------|
| ASP0180    | Xbox Hardware Refresh Disc                         | XB01101W    |
| ASP0380    | Tom Clancy's Splinter Cell Exclusive Playable Demo | US01251E    |
| ASP0980    | Tom Clancy's Rainbow Six 3 DEMO DISC               | US03152E-US |
| ASP5080    | The official xbox 50 best games (Demo disk)        | IM00113E-IM |
| ASP5180    | Rayman 3 hoodlum havoc                             |             |
| ASP5280    | Xbox Music Mixer                                   | MS09005A-MS |

Dumps
-----

### Files

#### Disc Manufacturing Information (DMI.bin)

2048 Bytes

READ DVD STRUCTURE with format 0x04

#### Physical Format Information (PFI.bin)

2048 Bytes Read from the Lead-In.

READ DVD STRUCTURE with format 0x00

#### Security Sectors (SS.bin)

Challenge entry (11 Bytes):

| Offset | Type | Field             | Notes                                                                           |
|--------|------|-------------------|---------------------------------------------------------------------------------|
| 0      | u8   | Valid             | Always 1 if the challenge is valid, else the challenge is ignored               |
| 1      | u8   | Challenge id      |                                                                                 |
| 2      | u32  | Challenge value   |                                                                                 |
| 6      | u8   | Response modifier | multimedia.cx says this might be a Response id. However, it's always 0 anyway?! |
| 7      | u32  | Response value    |                                                                                 |

Security sector range (9 Bytes)

| Offset | Type | Field     | Notes |
|--------|------|-----------|-------|
| 3      | u24  | Start PSN |       |
| 6      | u24  | End PSN   |       |

Unknown1 (320 Bytes)

| Offset | Type      | Field | Notes                                                        |
|--------|-----------|-------|--------------------------------------------------------------|
| 0      | u64       |       | Yet another timestamp?! (Similar to 1183 in complete format) |
| 8      | u32       |       | Unknown                                                      |
| 27     | u8        |       | Unknown                                                      |
| 28     | u8\[16\]  |       | Unknown                                                      |
| 44     | u8\[20\]  |       | Unknown                                                      |
| 64     | u8\[256\] |       | Unknown                                                      |

Complete format (2048 Bytes):

<table>
<thead>
<tr class="header">
<th><p>Offset</p></th>
<th><p>Type</p></th>
<th><p>Field</p></th>
<th><p>Notes</p></th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><p>0</p></td>
<td><p>PFI</p></td>
<td><p>Physical Format Information</p></td>
<td><p>PFI for the actual data, unknown size</p></td>
</tr>
<tr class="even">
<td><p>720</p></td>
<td><p>u32</p></td>
<td><p>Unknown</p></td>
<td></td>
</tr>
<tr class="odd">
<td><p>768</p></td>
<td><p>u8</p></td>
<td><p>Version of challenge table</p></td>
<td><p>Always 1</p></td>
</tr>
<tr class="even">
<td><p>769</p></td>
<td><p>u8</p></td>
<td><p>Number of challenge entries</p></td>
<td><p>Always 23</p></td>
</tr>
<tr class="odd">
<td><p>770</p></td>
<td><p>Challenge entry[]</p></td>
<td><p>Encrypted challenge entries</p></td>
<td></td>
</tr>
<tr class="even">
<td><p>1055</p></td>
<td><p>u64</p></td>
<td></td>
<td><p>Some large number timestamp?</p></td>
</tr>
<tr class="odd">
<td><p>1083</p></td>
<td><p>u8[16]</p></td>
<td></td>
<td><p>Unknown</p></td>
</tr>
<tr class="even">
<td><p>1183</p></td>
<td><p>Unknown1</p></td>
<td></td>
<td><p>Unknown, 44 bytes SHA-1 hash are generated here to be used as RC4 key to decrypt challenge entries</p></td>
</tr>
<tr class="odd">
<td><p>1503</p></td>
<td><p>Unknown1</p></td>
<td></td>
<td><p>Unknown</p></td>
</tr>
<tr class="even">
<td><p>End of data readable by a stock Xbox drive (1632 Bytes)</p></td>
</tr>
<tr class="odd">
<td><p>1632</p></td>
<td><p>u8</p></td>
<td><p>Number of security sector ranges</p></td>
<td><p>Always 23</p></td>
</tr>
<tr class="even">
<td><p>1633</p></td>
<td><p>Security sector range[]</p></td>
<td><p>Security sector ranges</p></td>
<td><p>Only 16 of which are used.</p></td>
</tr>
<tr class="odd">
<td><p>1840</p></td>
<td><p>Security sector range[]</p></td>
<td><p>Security sector ranges</p></td>
<td><p>Only 16 of which are used.<br />
<em>(Copy from Offset 1633)</em></p></td>
</tr>
</tbody>
</table>

All other fields are assumed to be zero!

##### Decryption of challenge entries

Starting at offset 1183, a 44 byte SHA-1 hash is generated. The
resulting hash - the first part of it, to be exact - is fed as the key
into RC4 decryption. The output of SHA-1 contains 160 bits of
information. 160 / 8 = 20 bytes of information. To express this as a
printable hex digest requires 40 characters. The SHA-1 hash is converted
to a hex digest and then the first 7 of the characters are fed into the
RC4 initialization function as the key. Then, the RC4 decrypter does its
work on the 253 Bytes of the challenge entries (Offset 770).

There'll only be a handful of valid entries in the challenge entries.
However there'll be at least 2.

### Dumping

To dump Xbox Game Discs you need one of the following drives /
firmwares:

<table>
<thead>
<tr class="header">
<th><p>Drive</p></th>
<th><p>Standard</p></th>
<th><p>Original Firmware download</p></th>
<th><p>Name of modified Firmware for dumping</p></th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td></td>
<td></td>
<td></td>
<td><p>0800</p></td>
</tr>
<tr class="even">
<td><p>Toshiba SD-M2012C</p></td>
<td><p>IDE</p></td>
<td></td>
<td><p>Kreon</p></td>
</tr>
<tr class="odd">
<td><p>Samsung SH-D162C</p></td>
<td><p>IDE</p></td>
<td></td>
<td><p>SB00 Kreon 0.60 (July 30th 2006)<br />
SB00 Kreon 0.80 (September 9th 2006)<br />
SB01 Kreon 1.00 (October 9th 2007)</p></td>
</tr>
<tr class="even">
<td><p>Samsung SH-D162D</p></td>
<td><p>IDE</p></td>
<td><p>[<a href="https://web.archive.org/web/20090601193905/http://www.samsungodd.com:80/korlib/download.asp?no" class="uri">https://web.archive.org/web/20090601193905/http://www.samsungodd.com:80/korlib/download.asp?no</a>=&amp;fname=200706281644411972_SH-D162D_SB00.bin&amp;path=/UploadFiles/FW/FWDOWNLOAD/ENG/ SB00]<br />
[<a href="https://web.archive.org/web/20090916202345/http://www.samsungodd.com:80/korlib/download.asp?no" class="uri">https://web.archive.org/web/20090916202345/http://www.samsungodd.com:80/korlib/download.asp?no</a>=&amp;fname=200811051941150901_SH-D162D_SB01.exe&amp;path=/UploadFiles/FW/FWDOWNLOAD/ENG/ SB01]<br />
SB02<br />
[<a href="https://web.archive.org/web/20090402052613/http://www.samsungodd.com:80/korlib/download.asp?no" class="uri">https://web.archive.org/web/20090402052613/http://www.samsungodd.com:80/korlib/download.asp?no</a>=&amp;fname=200903191825218171_SH-D162D_SB03.exe&amp;path=/UploadFiles/FW/FWDOWNLOAD/ENG/ SB03]<br />
[<a href="https://web.archive.org/web/20120123040117/http://www.samsungodd.com:80/korlib/download.asp?no" class="uri">https://web.archive.org/web/20120123040117/http://www.samsungodd.com:80/korlib/download.asp?no</a>=&amp;fname=200909281412336931_SH-D162D_SB04.exe&amp;path=/UploadFiles/FW/FWDOWNLOAD/ENG/ SB04]</p></td>
<td><p>SB00 Kreon 1.00 (November 18th 2007)</p></td>
</tr>
<tr class="odd">
<td><p>Toshiba TS-H352C</p></td>
<td><p>IDE</p></td>
<td></td>
<td><p>Kreon</p></td>
</tr>
<tr class="even">
<td><p>Toshiba TS-H352D</p></td>
<td><p>IDE</p></td>
<td></td>
<td><p>Kreon</p></td>
</tr>
<tr class="odd">
<td><p>Samsung SH-D163A</p></td>
<td><p>SATA</p></td>
<td></td>
<td><p>SB01 Kreon 1.00 (October 9th 2007)</p></td>
</tr>
<tr class="even">
<td><p>Samsung SH-D163B</p></td>
<td><p>SATA</p></td>
<td></td>
<td><p>Kreon 1.00 (November 18th 2007)</p></td>
</tr>
<tr class="odd">
<td><p>Toshiba TS-H353A</p></td>
<td><p>SATA</p></td>
<td></td>
<td></td>
</tr>
<tr class="even">
<td><p>Toshiba TS-H353B</p></td>
<td><p>SATA</p></td>
<td></td>
<td></td>
</tr>
</tbody>
</table>

Please note that the modified firmwares are based on copyrighted
material and can therefore not be legally shared here. Patch files to
patch original firmwares into dumping-firmwares would be appreciated.

Flashing software:

-   TSDNMAC for MacOS
-   [SFDNWIN](http://web.archive.org/web/20070301000000/http://www.samsungodd.com/KorLib/File/sfdnwin.exe)
    for Microsoft Windows 2000 and XP
-   TSDNWIN for Microsoft Windows Vista and 7
-   Dell SFDNDOS and the newer TSDNDOS for Microsoft DOS

For current dumping instructions see [the Dumping Guide by the Redump
Project](http://forum.redump.org/topic/6073/xbox-1-360-dumping-instructions/).

### Xbox related commands

#### Enable Unlock 1 (xtreme) state

Supported by: Kreon 1.00

    FF 08 01 01

Enable Unlock 1 (xtreme) state' as we already know it from the 360
xtreme modded drives. This command is supported for legacy reasons only.
Custom applications should use the new 'Set Lock State' instead.

#### Set Lock State

Supported by: Kreon 1.00

    FF 08 01 11 xx

-   `xx=00` - Drive locked (no unlock state)
-   `xx=01` - Unlock State 1 (xtreme) enabled
-   `xx=02` - Unlock state 2 (wxripper) enabled

#### SS extract command

Supported by: Kreon 1.00

    AD 00 FF 02 FD FF FE 00 08 00 xx C0

This is the well known from the xtreme firmware.

#### Get Feature List

Supported by: Kreon 1.00

    FF 08 01 10

This command will return a list of the additional features supported by
the drive. All values returned are 16 bit values, and the list is
terminated with null (`0x0000`). The two first words of the returned
list always reads as `0xA55A 0X5AA5` in order to guarantee that a reply
from a drive not supporting this command correctly isn't mistaken for a
feature list.

An example feature list could be:
`0xA55A, 0x5AA5, 0x0100, 0xF000, 0xF001, 0x0000`

This list would indicate that the drive supports XBOX360 Unlock 1, Lock
and Error Skip, as it can be seen from the values defined below:

XBOX 360 related features:

-   `0x0100` : The drive supports the unlock 1 state (xtreme)
-   `0x0101` : The drive supports the unlock 2 state (wxripper)
-   `0x0120` : The drive can read and decrypt the SS
-   `0x0121` : The drive has full challenge response functionality

XBOX related features:

-   `0x0200` : The drive supports the unlock 1 state (xtreme)
-   `0x0201` : The drive supports the unlock 2 state (wxripper)
-   `0x0220` : The drive can read and decrypt the SS
-   `0x0221` : The drive has full challenge response functionality

General drive features:

-   `0xF000` : The drive supports the lock (cancel any unlock state)
    command
-   `0xF001` : The drive supports error skipping

This is the complete list of defined features at the moment. If you're
working on a custom application you might want to contact me in order to
get the latest list.

References and links
--------------------

-   [<http://dark.ellende.eu/public/360DVDfirmwareRelatedInfo.pdf>](https://web.archive.org/web/20150616131202/http://dark.ellende.eu/public/360DVDfirmwareRelatedInfo.pdf)
-   [Overview of the challenge/response security
    protocol](https://multimedia.cx/eggs/xbox-sphinx-protocol/)
-   [Xbox Game Discs preserved by the Redump
    Project](http://redump.org/discs/system/xbox/)
-   [Missing Xbox Game Disc dumps at Redump
    Project](http://wiki.redump.org/index.php?title=Discs_not_yet_dumped#Microsoft_Xbox)

