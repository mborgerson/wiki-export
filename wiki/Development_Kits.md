---
title: Development Kits
permalink: wiki/Development_Kits/
layout: wiki
---

There are a few hardware diferences and software diferences between
development kits, but generaly the following hardware is known to be
exist:

-   Alpha I and II kits
-   DVT3 Development Kits
-   DVT4 Development Kits
-   Debug kits

Further more are diferences internaly from diferent MCP revisions, GPU
revisions and board layouts. Sega Chihiro boards seem to be based on
DVT4 Development/Debug kits. either using overproduction or on purpose
produced MCPX-X2 as found in the developement kits and debugkits.

Some rare boards are found with diferent MCPX chips that also have a
special port near the CPU, and a extra USB port on the backside of the
board (connected to the MCPX to unkown port) its been rumoured that the
blocked USB port (unblocked on DVT3) on the back of Developement kits
where either a leftover of these extra usb port or used for JVS
development and/or testing.

Alpha
-----

Contructed using “off the shelf” hardware, probably before retail
release from Intel. consisting of:

### Alpha I

-   Intel VC820, running a prerelease Bios
-   128Mb RDRAM with an terminator
-   600Mhz CPU
-   Nvidia Geforece3 NV20 prerelease hardware?
-   An OPTI 82C861 2 USB port PCI card
-   Intel 82559(ic) Network card

These where then programmed by use of a recovery disk that “recovered”

### Apha II

The Alpha II was a upgrade kit or constructed the same as a Alpha I with
the following upgrades:

-   733MHz CPU SL3XN
-   Nvidia Geforce3 NV20 (64MB ram) with a Preretail firmware

An supplied Recovery made the nececairy firmware upgrades and updated
the dashboard.

### Franken Alpha

This is a listing of notes on how to make a Alpha I or II yourself, also
known as a FrankenAlpha/

Same hardware as a Alpha I/II, but you dont seem to need the preretail
firmwares or bioses. Also, any Geforce3 is rumoured to work, as long as
the GPU is of the NV20 series. The AV out suboard isnt nececairy, as VGA
seems to work fine. color or diferent kinds of fans, cooling blocks or
OEM vendor names doesnt seem to matter. The Opti usb board can be green,
yellow or black. as long as the IC sports the 82C861 wording. if the
VC820 sports any USB or Sound, it will not be used, the listed hardware
boards are used instead, regardless of other hardware. (drivers arent in
the kernel).

Debug
-----

Shaped and main parts consist of retail hardware with the following
differences:

-   128MB ram instead of default 64MB ram
-   MCPX revision X2, thus capable of running only Debug signed code
-   Hardisks loaded with retail and Debug shell files

DVT3/DVT4
---------

Harddisk is said to be locked by default with a 16size byte 0x00 key,
but allowing to run with an unlocked harddrive of any size(larger then
20GB for minimal OS). All functions a debug with the addition of:

-   [ DVD emulation](/wiki/DVD_Emulator "wikilink")
-   [ Kernel\_Debug](/wiki/Kernel_Debugging "wikilink") over a dedicated [ IO
    board ](/wiki/Super_I/O "wikilink")

