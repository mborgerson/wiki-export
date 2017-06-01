---
title: BIOS
permalink: wiki/BIOS/
layout: wiki
---

The **BIOS** (an acronym for **Basic Input/Output System** and also
known as the **BIOS ROM** or **Xbox ROM**) is a firmware image that is
mapped to the top 16MiB of the CPU's physical address space
(0xFF000000-0xFFFFFFFF). Like the standard [PC
BIOS](https://en.wikipedia.org/wiki/BIOS), it is responsible for
initializing the Xbox hardware and booting the system. Unlike the PC
BIOS, however, the Xbox BIOS image also contains the kernel in a
compressed and encrypted form.

On a standard Xbox, the BIOS image is stored in the
[Flash](/wiki/Flash "wikilink"). The BIOS image is actually 256KiB, duplicated
4 times to fill the 1MiB ROM chip. You can verify this by running:

    $ split -n 4 xbox.bin 
    $ md5sum xa*
    542c62cb976a4993c8c5027dff9638ce  xaa
    542c62cb976a4993c8c5027dff9638ce  xab
    542c62cb976a4993c8c5027dff9638ce  xac
    542c62cb976a4993c8c5027dff9638ce  xad

You'll notice it is the same file repeated 4 times. That explains how
some BIOS chips are 1MB and some are 256KiB. The next thing is that the
BIOS is repeated from 0xFF000000 until it fills the rest of memory. In
other words, that 256KiB of data is repeated 64 times.

Components
----------

The BIOS is split into different components. These are largely the same
from BIOS to BIOS, but with some differences.

|                     | 3944    | 4034    | 4134    | 4817    | 5101    | 5530    | 5713    | 5838    |
|---------------------|---------|---------|---------|---------|---------|---------|---------|---------|
| Unknown             | 0x00000 | 0x00000 | 0x00000 | 0x00000 | 0x00000 | 0x00000 | 0x00000 | 0x00000 |
| X-Codes             | 0x00080 | 0x00080 | 0x00080 | 0x00080 | 0x00080 | 0x00080 | 0x00080 | 0x00080 |
| Copyright String    | 0x00CFA | 0x00CFA | 0x00CFA | 0x00DB9 | 0x00E49 | 0x00E59 | 0x00E59 | 0x00DCC |
| Kernel              |         |         |         |         |         |         |         |         |
| Kernel Data Segment |         |         |         |         |         |         |         |         |
| 2BL                 | 0x39E00 | 0x39E00 | 0x39E00 |         |         |         |         |         |
| Decoy Boot Loader   | 0x3FE00 | 0x3FE00 | 0x3FE00 | 0x3FE00 | 0x3FE00 | 0x3FE00 | 0x3FE00 | 0x3FE00 |

For information how these sections are used, see [Boot
Process](/wiki/Boot_Process "wikilink").

### Unknown

From 0x00000000 - 0x000000079

Not sure what this does. Some people think it might be involved with
initialising the [MCPX](/wiki/MCPX "wikilink"). The Reset Vector on the
Pentium 3 would mean that this wasn't called before the MCPX ROM.

### xcodes

These are the xcode operations run by the MCPX interpreter. The first
couple of lines appear to be nonsense (they don't execute any
functionality), but then the first actual codes that I have found are:

The xcodes in the BIOS versions 3944, 4034, 4134 all start with:
`04 10 08 00 80 01 80 00 00`

The xcodes in the BIOS versions 4817, 5101, 5530, 5713, 5838 all start
with: `04 84 08 00 80 01 80 00 00`

This leads me to believe that the first three BIOS versions that I have
are compatible with the 1.0 MCPX, and the rest are compatible with the
1.1 MCPX.

Next, some people believed that there was another unknown section
between the xcodes and the copyright string. As far as I can tell, that
section was to allow the xcode instruction set to expand, as the 5838
instructions are considerably larger than those in the 3944 BIOS.

### Copyright String

Literally contains the ASCII encoded text:
“`Copyright (c) Microsoft Corporation. All rights reserved.`” (57 bytes
long).

### Decoy bootloader

This is very similar to the [MCPX ROM](/wiki/MCPX_ROM "wikilink"), only the
xcode interpreter is different, and it doesn't include any
decryption/hashing algorithms.

See Also
--------

[BIOS Dumping](/wiki/BIOS_Dumping "wikilink")

References
----------

-   [Xbox BIOS dat file from Redump
    Project](http://redump.org/datfile/xbox-bios/)

