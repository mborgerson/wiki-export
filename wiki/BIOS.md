---
title: BIOS
permalink: wiki/BIOS/
layout: wiki
---

The **BIOS** (an acronym for **Basic Input/Output System** and also
known as the **BIOS ROM** or **Xbox ROM**) is a firmware image that is
mapped to the top 16MiB of the CPU's physical address space (0xFF000000
- 0xFFFFFFFF). Like the standard [PC
BIOS](https://en.wikipedia.org/wiki/BIOS), it is responsible for
initializing the Xbox hardware and booting the system. Unlike the PC
BIOS, however, the Xbox BIOS image also contains the kernel in a
compressed and encrypted form.

On a standard Xbox, the BIOS image is stored in the [Flash
ROM](/wiki/Flash_ROM "wikilink"). The BIOS image is actually 256 kiB,
duplicated 4 times to fill the 1 MiB ROM chip. You can verify this by
running:

    $ split -n 4 xbox.bin 
    $ md5sum xa*
    542c62cb976a4993c8c5027dff9638ce  xaa
    542c62cb976a4993c8c5027dff9638ce  xab
    542c62cb976a4993c8c5027dff9638ce  xac
    542c62cb976a4993c8c5027dff9638ce  xad

You'll notice it is the same file repeated 4 times. That explains how
some BIOS chips are 1 MiB and some are 256 kiB. The next thing is that
the BIOS is repeated from 0xFF000000 until it fills the rest of memory.
In other words, that 256 kiB of data is repeated 64 times.

Components
----------

The BIOS is split into different components. These are largely the same
from BIOS to BIOS, but with some differences.

<table>
<thead>
<tr class="header">
<th></th>
<th><p>3944</p></th>
<th><p>4034</p></th>
<th><p>4134</p></th>
<th><p>4817</p></th>
<th><p>5101</p></th>
<th><p>5530</p></th>
<th><p>5713</p></th>
<th><p>5838</p></th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><p>NV2A Initialization Table</p></td>
<td><p>0x00000</p></td>
<td><p>0x00000</p></td>
<td><p>0x00000</p></td>
<td><p>0x00000</p></td>
<td><p>0x00000</p></td>
<td><p>0x00000</p></td>
<td><p>0x00000</p></td>
<td><p>0x00000</p></td>
</tr>
<tr class="even">
<td><p>MCPX Initialization Table</p></td>
<td><p>0x00070</p></td>
<td><p>0x00070</p></td>
<td><p>0x00070</p></td>
<td><p>0x00070</p></td>
<td><p>0x00070</p></td>
<td><p>0x00070</p></td>
<td><p>0x00070</p></td>
<td><p>0x00070</p></td>
</tr>
<tr class="odd">
<td><p>X-Codes</p></td>
<td><p>0x00080</p></td>
<td><p>0x00080</p></td>
<td><p>0x00080</p></td>
<td><p>0x00080</p></td>
<td><p>0x00080</p></td>
<td><p>0x00080</p></td>
<td><p>0x00080</p></td>
<td><p>0x00080</p></td>
</tr>
<tr class="even">
<td><p>Copyright String</p></td>
<td><p>0x00CFA</p></td>
<td><p>0x00CFA</p></td>
<td><p>0x00CFA</p></td>
<td><p>0x00DB9</p></td>
<td><p>0x00E49</p></td>
<td><p>0x00E59</p></td>
<td><p>0x00E59</p></td>
<td><p>0x00DCC</p></td>
</tr>
<tr class="odd">
<td><p>Kernel</p></td>
<td><p>0x0619C</p></td>
<td></td>
<td></td>
<td></td>
<td></td>
<td></td>
<td></td>
<td></td>
</tr>
<tr class="even">
<td><p>Kernel Data Segment</p></td>
<td><p>0x3944C</p></td>
<td></td>
<td></td>
<td></td>
<td></td>
<td></td>
<td></td>
<td></td>
</tr>
<tr class="odd">
<td><p>2BL<br />
<em>Always 0x6000 bytes</em></p></td>
<td><p>0x39E00</p></td>
<td><p>0x39E00</p></td>
<td><p>0x39E00</p></td>
<td></td>
<td></td>
<td></td>
<td></td>
<td></td>
</tr>
<tr class="even">
<td><p>FBL<br />
<em>Always 0x2880 bytes</em></p></td>
<td></td>
<td></td>
<td></td>
<td><p>0x3D400</p></td>
<td><p>0x3D400</p></td>
<td><p>0x3D400</p></td>
<td><p>0x3D400</p></td>
<td><p>0x3D400</p></td>
</tr>
<tr class="odd">
<td><p>Decoy Boot Loader</p></td>
<td><p>0x3FE00</p></td>
<td><p>0x3FE00</p></td>
<td><p>0x3FE00</p></td>
<td><p>0x3FE00</p></td>
<td><p>0x3FE00</p></td>
<td><p>0x3FE00</p></td>
<td><p>0x3FE00</p></td>
<td><p>0x3FE00</p></td>
</tr>
</tbody>
</table>

For information how these sections are used, see [Boot
Process](/wiki/Boot_Process "wikilink").

### NV2A Initialization Table

The first DWORD is a pointer to a table of values that the NV2A
northbridge uses to initialize itself, but with the least-significant
bit always set to 1 (likely some sort of sanity check). The second DWORD
is the unmodified table pointer. On all Xbox BIOS, the NV2A
initialization table is always at file offset 8, virtual address
0xFF000008.

The first DWORD of the NV2A initialization table is the magic number
0x2B16D065 (called the “boot header”). The purpose of the remaining
values in the table is unknown.

### MCPX Initialization Table

This is a table of values used to initialize the MCPX southbridge. It
*must* be placed at offset 0x70 in the ROM image.

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
BIOS has considerably more xcodes than the 3944 BIOS.

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

