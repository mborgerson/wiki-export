---
title: MCPX ROM
permalink: wiki/MCPX_ROM/
layout: wiki
---

The purpose of the MCPX ROM is to setup the GPT table, enter 32 bit
mode, Enable caching, decrypt the second bootloader (2BL) and then
transfer control over to it. It has a fair few things to do, so also
contains an interpreter to read instructions from the BIOS (known as
xcodes). There are two known versions of the MCPX ROM, 1.0 and 1.1.

1.0 was found in the 1.0 Xbox and used an RC4 algorithm to decrypt the
2BL. For 1.1 Microsoft switched to a TEA algorithm. The code on the
chips is largely the same, but for those two differences.

Dumping the MCPX ROM
--------------------

This is no mean feat. In the event of failure, or shortly after the 2BL
execution has started, the Xbox executes the following codes:

    mov eax,0x80000880
    mov dx,0xcf8
    out dx,eax

This turns off the MCPX ROM, making it invisible to anything trying to
read it.

See [MCPX exploits](/wiki/Exploits#MCPX "wikilink") for more details.

MCPX Common
-----------

As mentioned, the MCPX chips are largely the same. When the Xbox is
powered on, the BIOS is loaded to the top 16 MB of memory (See \[BIOS\]
for more details), and the MCPX ROM overlays the last 512 bytes of that
memory. The Reset Vector tells the CPU to start executing code from
0xFFFFFFF0 (well, 0xFFFF:FFF0 due to being in 16 bit mode, but that is
beyond the scope of this article). The first thing it does is setup the
memory to consist of a 4GB continuous area, and then activates 32 bit
mode. Starts executing code from BIOS (starting at 0xFF000080) using the
\[xcode interpreter\]. Next it initialises the MTRR, enables caching,
and then things become different.

MCPX 1.0
--------

The 1.0 ROM runs an RC4 algorithm to decrypt the 2BL from flash
(starting at 0xFFFF9E00) and load the unencrypted 2BL to memory
(0x90000). It checks the signature of the decrypted 2BL and if it is
correct, starts executing code at 0x90000. Otherwise it errors.

MCPX 1.1
--------

Because of the flaws of the previous method, Microsoft changed the way
the 2BL was decrypted. Microsoft implemented a TEA algorithm to create a
hash of the FBL and if that was correct, it would jump to 0xFFFFD400.
This isn't in Memory, but rather in the BIOS. Otherwise it would error.

Error Handling
--------------

The MCPX turns itself off when there is an error and then jumps to
0x000001FA. This is technically in the BIOS (there is no MCPX to read
from any more), but the code is still relevant. It pretty much just
makes the LEDs flash and hang.

See Also
--------

-   [MCPX HLE](/wiki/MCPX_HLE "wikilink")

References
----------

-   [Deconstructing the Xbox Boot
    ROM](https://mborgerson.com/deconstructing-the-xbox-boot-rom)
-   [disassemble-mcpx.sh: A bash-script to disassemble/annotate MCPX 1.0
    dumps](https://github.com/JayFoxRox/xqemu-tools/blob/master/disassemble-mcpx.sh)

