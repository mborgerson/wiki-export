---
title: BIOS
permalink: wiki/BIOS/
layout: wiki
---

The BIOS is either a 1MB or 256KB memory which is loaded to the top 16MB
of memory (from 0xFF000000). There are a couple of things to understand
in order for this to make sense. First, the 1MB BIOS is actually 256KB
of data repeated 4 times. So, if you do:

    $ split -n 4 xbox.bin 
    $ md5sum xa*
    542c62cb976a4993c8c5027dff9638ce  xaa
    542c62cb976a4993c8c5027dff9638ce  xab
    542c62cb976a4993c8c5027dff9638ce  xac
    542c62cb976a4993c8c5027dff9638ce  xad

You'll notice it is the same file repeated 4 times. That explains how
some BIOS chips are 1MB and some are 256KB. The next thing is that the
BIOS is repeated from 0xFF000000 until it fills the rest of memory. In
other words, that 256KB of data is repeated 64 times.

Components
----------

The BIOS is split into different components. These are largely the same
from BIOS to BIOS, but with some differences.

### Unknown

From 0x00000000 - 0x000000079

Not sure what this does. Some people think it might be involved with
initialising the [MCPX ROM](/wiki/MCPX_ROM "wikilink"), but I don't know. The
Reset Vector on the Pentium 3 would mean that this wasn't called before
the MCPX ROM, but I'm guessing there are people out there who know a lot
more about it than me.

### xcodes

From 0x00000080 - Copyright string

These are the xcode operations run by the MCPX interpreter. The first
couple of lines appear to be nonsense (they don't execute any
functionality), but then the first actual codes that I have found are:

The xcodes in the BIOS versions 3944, 4034, 4134 all start with: 04 10
08 00 80 01 80 00 00

The xcodes in the BIOS versions 4817, 5101, 5530, 5713, 5838 all start
with: 04 84 08 00 80 01 80 00 00

This leads me to believe that the first three BIOS versions that I have
are compatible with the 1.0 MCPX, and the rest are compatible with the
1.1 MCPX.

Next, some people believed that there was another unknown section
between the xcodes and the copyright string. As far as I can tell, that
section was to allow the xcode instruction set to expand, as the 5838
instructions are considerably larger than those in the 3944 BIOS.

### Copyright String

57 bytes long with the following start positions:

-   3394 - 0xcfa
-   4034 - 0xcfa
-   4134 - 0xcfa
-   4817 - 0xdb9
-   5101 - 0xe49
-   5530 - 0xe59
-   5713 - 0xe59
-   5838 - 0xdcc

Literally contains the text: “Copyright (c) Microsoft Corporation. All
rights reserved.”.

### Kernel

Still need to analyse

### Kernel Data Segment

Still need to analyse

### 2BL

Still need to analyse

### Decoy bootloader

This is very similar to the [MCPX ROM](/wiki/MCPX_ROM "wikilink"), only the
xcode interpreter is different, and it doesn't include any
decryption/hashing algorithms.

Known Retail BIOSs
------------------

-   3394
-   4034
-   4134
-   4817
-   5101
-   5530
-   5713
-   5838

See Also
--------

[BIOS Dumping](/wiki/BIOS_Dumping "wikilink")

References
----------

-   [Understanding the Xbox boot
    process](http://hackspot.net/XboxBlog/?p=1)

