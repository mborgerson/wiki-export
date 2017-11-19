---
title: XDVDFS
permalink: wiki/XDVDFS/
layout: wiki
---

XDVDFS (Also known as XISO) is the image format used for [Xbox Game
Discs](/wiki/Xbox_Game_Disc "wikilink"). It is stored in the data area.

Format
------

Each sector is 2048 bytes.

### Filesystem

### Volume descriptor

32 sectors which are zero-filled. The Volume descriptor is 4096 bytes,
but split into 2x 2048 sections.

-   **Section 1:** The first data is the magic at sector 32. It is
    always “MICROSOFT\*XBOX\*MEDIA”.
-   **Section 2:** The first data is the magic at sector 32. It is
    always “MICROSOFT\*XBOX\*MEDIA”.

#### Examples

-   [Azurik - Rise of Perathia](/wiki/Azurik_-_Rise_of_Perathia "wikilink")
    (NTSC)
    -   xblayout version: 1.0.3926.1
    -   xbpremaster version: 1.0.3926.1
    -   Starting bruteforce
    -   Found seed 0x7EA870D7
    -   Completed bruteforce (Success)
-   [Genma Onimusha](/wiki/Genma_Onimusha "wikilink") (PAL)
    -   xblayout version: 1.0.4039.1
    -   xbpremaster version: 1.0.4039.2
    -   Starting bruteforce
    -   Found seed 0x25BAB84C
    -   Completed bruteforce (Success)
-   [Max Payne](/wiki/Max_Payne "wikilink") (PAL)
    -   xblayout version: 1.0.4134.1
    -   xbpremaster version: 1.0.4242.1
    -   Starting bruteforce
    -   Found seed 0x62BBC340
    -   Completed bruteforce (Success)
-   [Petit Copter](/wiki/Petit_Copter "wikilink") (Japanese)
    -   xblayout version: 1.0.4361.1
    -   xbpremaster version: 1.0.4361.2
    -   Starting bruteforce
    -   Found seed 0xF401863E
    -   Completed bruteforce (Success)
-   [007 - Agent Under Fire](007_-_Agent_Under_Fire "wikilink") (PAL)
    -   xblayout version: 1.0.4432.1
    -   xbpremaster version: 1.0.4432.1
    -   Starting bruteforce
    -   Found seed 0x1796C12A
    -   Completed bruteforce (Success)
-   [Metal Gear Solid 2 -
    Substance](/wiki/Metal_Gear_Solid_2_-_Substance "wikilink") (NTSC)
    -   xblayout version: 1.0.4721.1
    -   Starting bruteforce
    -   Found seed 0xFACBB379
    -   Completed bruteforce (Success)
-   [Battle Engine Aquila](/wiki/Battle_Engine_Aquila "wikilink") (PAL)
    -   xblayout version: 1.0.4831.1
    -   Starting bruteforce
    -   Completed bruteforce (Failure)
-   [Metal Gear Solid 2 -
    Substance](/wiki/Metal_Gear_Solid_2_-_Substance "wikilink") (PAL)
    -   xblayout version: 1.0.5120.1
    -   Starting bruteforce
    -   Completed bruteforce (Failure)
-   [Shenmue II](/wiki/Shenmue_II "wikilink") (PAL)
    -   xblayout version: 1.0.5120.1
    -   Starting bruteforce
    -   Completed bruteforce (Failure)
-   [Star Wars - Knights of the Old
    Republic](/wiki/Star_Wars_-_Knights_of_the_Old_Republic "wikilink") (PAL)
    -   xbgamedisc version: 2.1.0.5233.1
    -   mastering tool version: 2.1.0.5233.1
    -   Starting bruteforce
    -   Completed bruteforce (Failure)
-   [Indiana Jones and the Emperor's
    Tomb](/wiki/Indiana_Jones_and_the_Emperor's_Tomb "wikilink") (PAL)
    -   xbgamedisc version: 2.1.0.5233.1
    -   mastering tool version: 2.1.0.5233.1
    -   Starting bruteforce
    -   Completed bruteforce (Failure)
-   [Dynasty Warriors 4](/wiki/Dynasty_Warriors_4 "wikilink") (PAL)
    -   xbgamedisc version: 2.1.0.5344.1
    -   mastering tool version: 2.1.0.5344.1
    -   Starting bruteforce
    -   Completed bruteforce (Failure)
-   [The Matrix - Path of Neo](/wiki/The_Matrix_-_Path_of_Neo "wikilink")
    (PAL)
    -   xbgamedisc version: 2.1.0.5849.1
    -   mastering tool version: 2.1.0.5849.1
    -   Starting bruteforce
    -   Completed bruteforce (Failure)
-   [The Suffering - Ties That
    Bind](/wiki/The_Suffering_-_Ties_That_Bind "wikilink")
    -   xbgamedisc version: 2.1.0.5849.1
    -   mastering tool version: 2.1.0.5849.1
    -   Starting bruteforce
    -   Completed bruteforce (Failure)
-   [Reservoir Dogs](/wiki/Reservoir_Dogs "wikilink") (PAL)
    -   xbgamedisc version: 2.1.0.5849.1
    -   mastering tool version: 2.1.0.5849.1
    -   Starting bruteforce
    -   Completed bruteforce (Failure)

### Directory Entry

#### Version 4361

File entry flags:

-   READONLY = 0x01
-   HIDDEN = 0x02
-   SYSTEM = 0x04
-   DIRECTORY = 0x10
-   ARCHIVE = 0x20
-   NORMAL = 0x80

### File data blocks

#### Version 4361

Incomplete sectors are padded with 0x00 bytes.

### Random blocks

#### Version 4361

Seeded and then starting to emit bytes in data area. Filled with
algorithm 1 (2048 bytes at a time, even buffer address).

### Security blocks

#### Version 4361

Treated like random block.

Tools
-----

-   [XBISO](https://github.com/OpenXDK/xbiso)
-   [extract-xiso](https://github.com/mborgerson/extract-xiso)

