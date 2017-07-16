---
title: Super I/O
permalink: wiki/Super_I/O/
layout: wiki
---

The Super I/O board is a feature of [Development
Kits](/wiki/Development_Kits "wikilink"). The board is build around a SMSC
LPC47M157 chip. A datasheet for a close relative can be found at
<http://datasheet.seekic.com/PdfFile/LPC/LPC47M140NC_SMSC_SMSC_Corporation.pdf>
(a working link seems to be:
<http://pdf-file.ic37.com/pdf1/SMSC/LPC47M140_datasheet_138298/225601/LPC47M140_datasheet.pdf>)

The board provides the following ports:

-   RS232 (used for [ Kernel debugging](/wiki/Kernel_Debug "wikilink"), not
    the same as [Xbox Debug Monitor](/wiki/Xbox_Debug_Monitor "wikilink"))

unpopulated ports or functions are:

-   PS2 mouse and keyboard
-   something MCPX (SMBus?)
-   Temp something (SMBus?)
-   Post code (SMBus?)
-   Flashrom bios (like modchips, replaces onboard kernel?)

[Picture of the board](http://codeasm.com/xbox/images/dvt4/SL734874.JPG)

Related links
-------------

-   [Super I/O emulation in
    XQEMU](https://github.com/espes/xqemu/blob/xbox/hw/xbox/lpc47m157.c)

(SMBus?)
