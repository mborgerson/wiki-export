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

#### Version 4361

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

