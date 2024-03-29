---
title: Development Kits
permalink: wiki/Development_Kits/
layout: wiki
---

There are a few hardware differences and software differences between
development kits, but generally the following hardware is known to be
exist:

-   Alpha I and II kits
-   DVT3 Development Kits
-   DVT4 Development Kits
-   Debug kits

Further more there are differences internally from different MCPX
revisions, and GPU revisions, to different board layouts. Sega Chihiro
boards seem to be based on DVT4 Development/Debug kits. either using
overproduction or on purpose produced MCPX-X2 as found in the
development kits and debugkits.

Some rare boards are found with different MCPX chips that also have a
special port near the CPU, and a extra USB port on the backside of the
board (connected to the MCPX to unknown port) its been rumored that the
blocked USB port (unblocked on DVT3) on the back of Development kits
where either a leftover of these extra usb port or used for JVS
development and/or testing.

Alpha
-----

Contructed using “off the shelf” hardware, probably before retail
release from Intel. The alpha hardware allowed kernel debugging to be
done over one of the rs232 ports and standard WinDBG software can make
these messages and control work. This allows of careful debugging of the
running kernel to diagnose occuring faults or errors. At early boot of
the recovery software, a network IP address is attempted, probably to
setup for another way of diagnosing and remote control using the
available software like later XDK software.

The Alpha is built with the following parts or software:

### Alpha I

-   Intel VC820, running a pre-release BIOS
-   128Mb RDRAM with an terminator
-   600Mhz CPU
-   Nvidia Geforce 2 GTS 64MB
-   A xircom PCPGI2(OPTI 82C861) 2 USB port PCI card
-   Intel 82559(ic) Network card
-   Hard drive is a WD205AA (20.5 Gb) or fujitsu MPF3204AT (20.4 Gb)
-   ATNG 250w or a 300w powersupply

These were then programmed by use of a recovery disk that “recovered”
the system. The case is a customized globalwin ycc-802. (color silver
and an added jewel)

### Alpha II

The Alpha II was an upgrade kit or constructed the same as a Alpha I
with the following upgrades:

-   733MHz CPU SL3XN
-   Nvidia Geforce3 NV20 (64MB ram) with a Pre-retail firmware
    (180-p0050-0000-a09)

A supplied Recovery made the necessary firmware upgrades and updated the
dashboard.

### Franken Alpha

This is a listing of notes on how to make a Alpha I or II yourself, also
known as a FrankenAlpha.

Same hardware as a Alpha I/II, but you dont seem to need the pre-retail
firmwares or BIOSes. Also, any Geforce3 is rumoured to work, as long as
the GPU is of the NV20 series. The AV out sub-board isn't necessary, as
VGA seems to work fine. color or different kinds of fans, cooling blocks
or OEM vendor names doesnt seem to matter. The Opti usb board can be
green, yellow or black. as long as the IC sports the 82C861 wording. if
the VC820 sports any USB or Sound, it will not be used, the listed
hardware boards are used instead, regardless of other hardware. (drivers
arent in the kernel).

#### Posible solutions on fixing a (franken) alpha(2)

While fixing a frankenalpha, the following kernel errors occured while
intentionally incorrectly installing or configuring the computer. These
results may vary with official parts, configs, BIOS or firmware. Or due
to differences in hardware. These are results from recovery 3521 as
earlier ones did not work on the Alpha under test.

IDEX hard disk not configured (status=51).  
HDD too small

IDEX hard disk not configured (status=ff).  
HDD on the wrong bus, put on primary channel.(maybe wrong place on the
cable itself) maybe add a jumper for master select?

IDEX hard disk not found (status=7f).  
HDD not connected (or dead)

MP No video output because you are using an older video card.  
Well, insert a Geforce3 card (NV20 based)

WRN\[XNET\] EnetInitialize failed 0x801f0001  
Logo loads, with a halt when the sparkly bit is on the B of Xbox. The
network card is missing, insert a GD82559 based intel network card FCC:
EJMNPDALBANY (worked for me)

\[XNET\] NicExecuteActionCmdAndWait failed!WRN\[XNET\] EnetInitialize failed 0x801f0001  
Network on wrong pci lane or no screen connected (VGA or S-video)

-   Tested Slot 1 as good (real close to the GPU, would not recommend)
-   Tested Slot 2 as not good
-   Tested Slot 3 as not good (\*\*\* Assertion failed:
    hwres.ResourceData.Address\[0\].Type == CmResourceTypeMemory
-   Tested Slot 4 as good
-   Tested Slot 5 as not good

These might be wrong for official alphas, or only for the particular
motherboard that was being tested.

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
20GB for minimal OS).DVT3 seems to have the back USB port to be
uncovered and a slightly more glossy or shinier jewel on the top of the
case. According to some sources, the DVT3 cannot be updated to further
kernels/dashboards than 3911, the lowest/oldest being 3823.1(Borman said
this?)

Has all the functions of a debug, with the addition of:

-   [ DVD emulation](/wiki/DVD_Emulator "wikilink")
-   [ Kernel\_Debug](/wiki/Kernel_Debugging "wikilink") over a dedicated [ IO
    board ](/wiki/Super_I/O "wikilink")

