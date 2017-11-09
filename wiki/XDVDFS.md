---
title: XDVDFS
permalink: wiki/XDVDFS/
layout: wiki
---

XDVDFS (Also known as XISO) is the image format used for [Xbox Game
Discs](/wiki/Xbox_Game_Disc "wikilink"). It is stored in the data area.

Format
------

Each sector is 2048 bytes. The first data is the magic at sector 32. It
is always “MICROSOFT\*XBOX\*MEDIA”.

### Filesystem

File entry flags:

-   READONLY = 0x01
-   HIDDEN = 0x02
-   SYSTEM = 0x04
-   DIRECTORY = 0x10
-   ARCHIVE = 0x20
-   NORMAL = 0x80

### Security blocks

### Random blocks

Tools
-----

-   [XBISO](https://github.com/OpenXDK/xbiso)
-   [extract-xiso](https://github.com/mborgerson/extract-xiso)

