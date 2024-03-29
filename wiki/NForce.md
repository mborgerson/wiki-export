---
title: NForce
permalink: wiki/NForce/
layout: wiki
---

This documents collects information about the Xbox chipset and its
sibling, the nVidia nForce chipset, as well as further relatives.

nForce
------

The nForce chipset consists of the IGP (Integrated Graphics Processor)
Northbridge and the MCP (Media and Communications Processor)
Southbridge. Both are available in different flavours:

-   IGP-64: 64 bit memory bus
-   IGP-128: 128 bit memory bus (TwinBank), requires two DIMM modules
    for 128 bit operation
-   MCP-D: includes Dolby Digital encoder
-   MCP: Dolby Digital encoder disabled

So these are the four possible combinations:

|         |            |             |
|---------|------------|-------------|
|         | MCP        | MCP-D       |
| IGP-64  | nForce 220 | nForce 220D |
| IGP-128 | nForce 420 | nForce 420D |

[1](https://web.archive.org/web/20100617023830/http://www.theregister.co.uk/2001/05/31/nvidia_crush_chipset_named_nforce/)
[2](https://web.archive.org/web/20100617023830/http://www.theregister.co.uk/2001/06/01/nvidia_crush_is_called_nforce/)

The VGA controller inside the IGP is a “GeForce2 MX Integrated Graphics”
(PCI ID:10de/01a0). Its internal name is NV1A.

[3](https://web.archive.org/web/20100617023830/http://pciids.sourceforge.net/iii/?i=10de)
[4](https://web.archive.org/web/20100617023830/http://www.nvitalia.com/articoli/editoriali/produzione_nvidia_2001.htm)

Although IGP-64 and IGP-128 are different and their respective chipsets
have different codenames (Crush11 and Crush12, see below), there seems
to be no difference from the software side:

-   The VGA BIOS of the MS-6367 mainboard (nForce 420D configuration,
    i.e. Crush12) has the internal name “CR11BT.ROM”. It also includes
    the strings “NVIDIA GeForce2 Integrated GPU”, “CR11 Board” and “Chip
    Rev B2”.
-   The PCI IDs seem to be the same for the GPUs inside IGP-64 and
    IGP-128.

Crush
-----

“Crush” was the codename of the nForce chipset. Crush11 is the nForce
220/220D/230/230-T, Crush12 is the nForce 420/420D/430/430-T, and
Crush18 is the nForce2. The “11” probably derives from “NV11”, the
internal name of the GeForce2 MX.

[5](https://web.archive.org/web/20100617023830/http://users.erols.com/chare/chipsets.htm)
[6](https://web.archive.org/web/20100617023830/http://www.theregister.co.uk/2000/11/17/nvidias_super_secret_crush_spec/)

nForce & Xbox
-------------

The Xbox has an IGP-128 that uses an NV2A video core (PCI ID:10de/02a5),
which is between the GeForce3 (NV20) and the GeForce4 (NV25). The
Southbridge is called “MCP-X” and lacks the PCI card bus (PCI bus \#1).

[7](https://web.archive.org/web/20100617023830/http://www.digit-life.com/articles/nvidianforce/)
[8](https://web.archive.org/web/20100617023830/http://www.anandtech.com/showdoc.html?i=1484)
[9](https://web.archive.org/web/20100617023830/http://www.anandtech.com/cpuchipsets/showdoc.aspx?i=1535)
[10](https://web.archive.org/web/20100617023830/http://www.anandtech.com/systems/showdoc.aspx?i=1561&p=3)

AMD Heritage
------------

There is the following rumour, which is not fully verified yet:

-   Microsoft wanted AMD to make the CPU and the chipset for the Xbox,
    and nVidia to make the video hardware.
-   When alpha hardware had already bee built, Intel made a better deal
-   Microsoft agreed to have Intel CPUs; Intel had to modify AMD's
    chipset to support an Intel CPU
-   Intel insisted that the brand name AMD could not be associated with
    the Xbox, so nVidia licensed the AMD chipset. Now the Xbox chipset
    was by nVidia.
-   nVidia sold the same chipset for PCs, calling it “nForce”.

This is the reason why

-   the Xbox is the only nForce chipset with an Intel CPU
-   the AMD chipset and the nForce chipset are so similar

Evidence:

-   The AMD and nForce AMD IDE controllers are fully compatible. (Linux
    kernel: “AMD 755/756/766/8111 and nVidia
    nForce/2/2s/3/3s/CK804/MCP04 IDE driver for Linux.”
    [11](https://web.archive.org/web/20100617023830/http://lxr.linux.no/source/drivers/ide/pci/amd74xx.c))
-   The I2C/SMBus controller on the nForce is fully AMD-756/766/68
    compatible.
    [12](https://web.archive.org/web/20100617023830/http://lxr.linux.no/source/drivers/i2c/busses/i2c-amd756.c)
-   The audio controller is i810 compatible - as is the audio controller
    of the AMD-768 and the AMD-8111.
-   The nForce and AMD-768 modems are compatible.
-   At least one register (“VGA\_en”) in the nForce PCI-to-AGP bridge is
    compatible with the AMD chipset (AMD-761, 24081.pdf, page 136).
-   The nForce uses HyperTransport.
-   [13](https://web.archive.org/web/20100617023830/http://www.uwsg.iu.edu/hypermail/linux/kernel/0307.3/0922.html),
    [14](https://web.archive.org/web/20100617023830/http://www.uwsg.iu.edu/hypermail/linux/kernel/0301.3/0305.html)
-   *“One man's guess, the silicon is not a major factor. Because the
    nForce and 760 MP have a similar pin count, they are going to be
    cost comparable.”*
    [15](https://web.archive.org/web/20100617023830/http://overclockers.com/articles446/)

|            |             |             |
|------------|-------------|-------------|
|            | Northbridge | Southbridge |
| AMD-760    | AMD-761     | AMD-766     |
| AMD-760MP  | AMD-762     | AMD-766     |
| AMD-760MPX | AMD-762     | AMD-768     |

[AMD-760™ Chipset Tech
Docs](https://web.archive.org/web/20100617023830/http://www.amd.com/us-en/Processors/TechnicalResources/0,,30_182_873_1133,00.html)
[AMD-760™ MP Chipset Tech
Docs](https://web.archive.org/web/20100617023830/http://www.amd.com/us-en/Processors/TechnicalResources/0,,30_182_739_1130,00.html)
[AMD-760™ MPX Chipset Tech
Docs](https://web.archive.org/web/20100617023830/http://www.amd.com/us-en/Processors/TechnicalResources/0,,30_182_873_4296,00.html)  
The nForce chipset might be based on the AMD-760 chipset.

More Links
----------

[16](https://web.archive.org/web/20100617023830/http://www.duxcw.com/digest/guides/mb_chip/nforce/print.htm)
