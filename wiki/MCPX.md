---
title: MCPX
permalink: wiki/MCPX/
layout: wiki
---

The MCPX is the southbridge chip of the Xbox chipset by Nvidia. It
contains the sound processors ([APU](/wiki/APU "wikilink") and
[ACI](/wiki/ACI "wikilink")) and also the USB, PCI, IDE, etc,
controllers[1](https://web.archive.org/web/20010418214256/http://www.ga-hardware.com:80/preview.cfm?id=NVIDIAMCP),
[2](https://web.archive.org/web/20010410003338/http://www.nvnews.net/previews/mcpx/mcpx.shtml).

### ROM

The MCPX is home to the secret [MCPX ROM](/wiki/MCPX_ROM "wikilink").

### Pin L21: PC Speaker

The MCPX has PC Speaker pin which can be controlled using [the standard
PC Speaker interface](https://wiki.osdev.org/PC_Speaker). However, no
actual speaker is connected to the pin, so while the signal exists,
there will be no audible sound on a stock Xbox.

A speaker can be soldered to this pin and to make the signal audible
[3](https://www.youtube.com/watch?v=Te4MSskbBEE)[4](https://github.com/0DaveX/beep/)

The original Microsoft code does not drive the PC Speaker at all, so
this otherwise unused pin can also be used for inaudible forms of
unidirectional communication.

Image:XboxWithPcSpkr.jpg|'' '' Image:XboxPcSpkrTrace.jpg|'' ''
Image:XboxPcSpkrSolderPoints.jpg|'' ''
