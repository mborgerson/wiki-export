---
title: Xbox Game Disc
permalink: wiki/Xbox_Game_Disc/
layout: wiki
---

Xbox games are shipped on DVDs. They are commonly referred to as Xbox
Game Discs (XGD).

Dumping
-------

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

