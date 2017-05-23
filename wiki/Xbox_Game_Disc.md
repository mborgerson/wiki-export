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

| Drive             | Standard | Original Firmware download                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                   | Name of modified Firmware for dumping |
|-------------------|----------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|---------------------------------------|
|                   |          |                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                              | 0800                                  |
| Toshiba SD-M2012C | IDE      |                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                              | Kreon                                 |
| Samsung SH-D162C  | IDE      | Kreon 0.80 (September 9th 2006)                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                              | Kreon 1.00 (October 9th 2007)         |
| Samsung SH-D162D  | IDE      | \[<https://web.archive.org/web/20090601193905/http://www.samsungodd.com:80/korlib/download.asp?no>=&fname=200706281644411972\_SH-D162D\_SB00.bin&path=/UploadFiles/FW/FWDOWNLOAD/ENG/\]\[<https://web.archive.org/web/20090916202345/http://www.samsungodd.com:80/korlib/download.asp?no>=&fname=200811051941150901\_SH-D162D\_SB01.exe&path=/UploadFiles/FW/FWDOWNLOAD/ENG/\]\[<https://web.archive.org/web/20090402052613/http://www.samsungodd.com:80/korlib/download.asp?no>=&fname=200903191825218171\_SH-D162D\_SB03.exe&path=/UploadFiles/FW/FWDOWNLOAD/ENG/\]\[<https://web.archive.org/web/20120123040117/http://www.samsungodd.com:80/korlib/download.asp?no>=&fname=200909281412336931\_SH-D162D\_SB04.exe&path=/UploadFiles/FW/FWDOWNLOAD/ENG/\] | Kreon 1.00 (November 18th 2007)       |
| Toshiba TS-H352C  | IDE      |                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                              | Kreon                                 |
| Toshiba TS-H352D  | IDE      |                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                              | Kreon                                 |
| Samsung SH-D163A  | SATA     |                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                              | Kreon 1.00 (October 9th 2007)         |
| Samsung SH-D163B  | SATA     |                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                              | Kreon (November 18th 2007)            |
| Toshiba TS-H353A  | SATA     |                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                              |                                       |
| Toshiba TS-H353B  | SATA     |                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                              |                                       |

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

