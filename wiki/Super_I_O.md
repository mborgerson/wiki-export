---
title: Super I/O
permalink: wiki/Super_I/O/
layout: wiki
---

The Super I/O board is a feature of [Development
Kits](/wiki/Development_Kits "wikilink"). The board is build around a SMSC
LPC47M157 chip
([Datasheet](https://drive.google.com/uc?export=download&id=0BxOesalXbGtOanoxenlqQUh6Y0k))

The board provides the following ports:

-   RS232 (used for [ Kernel debugging](/wiki/Kernel_Debug "wikilink"), not to
    be confused with [Xbox Debug
    Monitor](/wiki/Xbox_Debug_Monitor "wikilink"))

unpopulated ports or functions are:

-   PS/2 Mouse port
-   PS/2 Keyboard port
-   something MCPX (SMBus?)
-   Temp something (SMBus?)
-   Post code (SMBus?)
-   Flash-ROM / BIOS (like modchips, replaces onboard kernel?)

[Picture of the board](http://codeasm.com/xbox/images/dvt4/SL734874.JPG)

Related links
-------------

-   [Super I/O emulation in
    XQEMU](https://github.com/espes/xqemu/blob/xbox/hw/xbox/lpc47m157.c)

(SMBus?)
