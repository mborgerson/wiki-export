---
title: Boot Process
permalink: wiki/Boot_Process/
layout: wiki
---

Overview
--------

The Xbox has a 256 kiB ROM containing the startup animation and sound,
as well as the Xbox kernel, which contains a stripped down version of
the Windows 2000 (NT 5.0) microkernel, the HAL, filesystems, as well as
HDD and DVD drivers.

When the Xbox is turned on, the software in ROM is decompressed into
RAM, and the kernel initializes the hardware. Because there are no audio
or video drivers in the kernel, the startup code plays the animation and
sound by accessing the registers of the hardware directly. As soon as
the Xbox logo is on the display, the kernel unlocks the hard disk and
checks whether there is a valid game medium in the DVD drive. If not,
the file xboxdash.xbe gets loaded from partition \#3 (Typically the
[Dashboard](/wiki/Dashboard "wikilink")). In either case, the Microsoft logo
is shown below the Xbox logo and the executable is started. If an error
occurs (no/wrong hard disk, wrong signature, ...), the boot loader shows
a [Fatal Error](/wiki/Fatal_Error "wikilink") screen and halts.

### Chain of trust

The Xbox uses a chain of trust during the boot process:

-   The MCPX ROM contains a key to decrypt the 2BL.
-   The 2BL is run. The MCPX ROM is hidden at this point. The 2BL
    decryption key is (overwritten with 0x00-Bytes). The 2BL contains a
    kernel decryption key.
-   Once the kernel is decrypted and initialized, the INIT section is
    discarded. The kernel decryption key is overwritten with 0x00-Bytes.
-   The kernel only runs signed [XBE](/wiki/XBE "wikilink") files from allowed
    media.

There are also a handful of assumptions:

-   The CPU will start execution in the MCPX ROM.
-   The MCPX ROM can not be read or modified.
-   The decrpyted 2BL or Kernel can not be read entirely.
-   All parts of the software following the MCPX are not-attackable and
    signed.

See [Exploits](/wiki/Exploits "wikilink") for possible options to break the
chain of trust.

MCPX ROM
--------

Certain things are still missing, for example, getting the CPU to 32 bit
protected mode and enabling caching.

### Xcodes

The xcode interpreter is common through both versions of the MCPX ROM.
The high level interpretation of the MCPX ROM might look like this:

    void xcode_interpreter() {
        int run_xcodes = 1;
        uint32_t eip = 0xff000080; // Not really EIP. This is just a pointer to the next xcode
        uint32_t result, scratch = 0;
        while (run_xcodes) {
            opcode    = get_memory_byte(eip);
            operand_1 = get_memory_dword(eip+1);
            operand_2 = get_memory_dword(eip+5);

            if (opcode == 0x07) {
                opcode    = operand_1;
                operand_1 = operand_2;
                operand_2 = result;
            }

            switch (opcode) {
                case 0x02:
                    result = get_memory_dword(operand_1 & 0x0fffffff);
                    break;
                case 0x03:
                    set_memory_dword(operand_1) = operand_2;
                    break;
                case 0x06:
                    result = (result & operand_1) | operand_2;
                    break;
                case 0x04:
                    if (operand_1 == 0x80000880) {
                        operand_2 &= 0xfffffffd;
                    }
                    outl(operand_1, 0xcf8);
                    outl(operand_2, 0xcfc);
                    break;
                case 0x05:
                    outl(operand_1, 0xcf8);
                    result = inl(0xcfc);
                    break;
                case 0x08:
                    if (result != operand_1) {
                        eip += operand_2;
                    }
                    break;
                case 0x09:
                    eip += operand_2;
                    break;
                case 0x10:
                    scratch = (scratch & operand_1) | operand_2;
                    result = scratch;
                    break;
                case 0x11:
                    outb(operand_2, operand_1);
                    break;
                case 0x12:
                    result = inb(operand_1);
                    break;
                case 0xee:
                    run_xcodes = 0;
                default:
                    break;
            }

            eip += 9;
        }
    }

### MCPX 1.0: RC4 Decryption of the 2BL

Version 1.0 of the ROM uses RC4 to decrypt the 2BL.

#### Stage 1: Key Scheduling

The [RC4 Key-Scheduling
Algorithm](https://en.wikipedia.org/wiki/RC4#Key-scheduling_algorithm_.28KSA.29)
is used to initialize the RC4 “S” array, first initializing the identity
permutation (writing 1, 2, ..., 255 to 0x8f000 to 0x850FF), then
processed in a way similar to the PRGA to mix in the key.

    uint8_t  *s = (uint8_t *)0x8f000;
    uint32_t  i;

    for (i = 0; i <= 255; i++) {
        s[i] = i;
    }

    uint8_t *key = (uint8_t *)0xffffffa5; /* ROM offset 0x1a5. */
    uint8_t j, t;

    /* It is unclear why values s[0x100..0x101] are being set to 0. They are
     * not modified by the code, but later these will be be used as the initial
     * i, j values in the PRGA.
     */
    s[0x100] = 0x00;
    s[0x101] = 0x00;

    for (i = 0, j = 0; i <= 255; i++) {
        j = j + s[i] + key[i%16];

        /* Swap s[i] and s[j] */
        t    = s[i];
        s[i] = s[j];
        s[j] = t;
    }

#### Stage 2: PRGA

The [RC4 Pseudo-random generation algorithm
(PRGA)](https://en.wikipedia.org/wiki/RC4#Pseudo-random_generation_algorithm_.28PRGA.29)
is then used to decrypt the 2BL from 0xFFFF9E00, storing the decrypted
2BL at 0x00090000. It is 24KiB in size.

    uint8_t  *encrypted = (uint8_t*)0xFFFF9E00; /* 2bl */
    uint8_t  *decrypted = (uint8_t*)0x90000; /* Decrypted 2bl Destination */
    uint32_t  pos;

    /* As noted above, s[0x100..0x101] were set to 0 earlier, but have not been
     * modified since. The RC4 algorithm defines i and j both to be set to 0
     * before PRGA begins. */
    i = s[0x100];
    j = s[0x101];

    for (pos = 0; pos < 0x6000; pos++) {
        /* Update i, j. */
        i  = (i + 1) & 0xff;
        j += s[i];

        /* Swap s[i] and s[j]. */
        t    = s[i];
        s[i] = s[j];
        s[j] = t;

        /* Decrypt message and write output. */
        decrypted[pos] = encrypted[pos] ^ s[ s[i] + s[j] ];
    }

#### Stage 3: Signature Verification

Now that the Second-Stage Bootloader has been loaded, a quick
sanity-check is performed: a “magic” signature is verified. If the
signature doesn’t match, control goes to the error handler. If the
signature does match, the code will jump to the 2bl entry point, which
is given by the first dword of the decrypted 2bl.

    mov  eax, [0x95fe4]
    cmp  eax, MAGIC_NUMBER
    jne  0xffffff94 ; If signature check failed, jump to error handler
    mov  eax, [0x90000]
    jmp  eax        ; Jump to 2BL entry point

### Notes

The RC4 algorithm was included as part of MCPX 1.0 and seems to work
fine with BIOS versions 3944, 4034, and 4134.

### MCPX 1.1: TEA Decryption of the 2BL

2BL
---

Certain parts are still missing

### MTRR Setup

First, the cache is disabled. Then, the MTRR (Memory Type Range
Register) will be setup (using `wrmsr`) in the following way:

| MTRR (ecx)            | High value (edx)           | Low value (eax) | Notes                   |
|-----------------------|----------------------------|-----------------|-------------------------|
| 0x200                 | 0x00000000                 | 0x00000006      |                         |
| rowspan = “2” | 0x201 | rowspan = “2” | 0x0000000F | 0xFC000800      | *(For 64 MiB RAM BIOS)* |
| 0xF8000800            | (*For 128 MiB RAM BIOS*)   |
| 0x202                 | 0x00000000                 | 0xFFF80005      |                         |
| 0x203                 | 0x0000000F                 | 0xFFF80800      |                         |
| 0x204                 | 0x00000000                 | 0x00000000      | Clear all unused MTRR   |
| colspan = “3” | ...   |
| 0x20F                 | 0x00000000                 | 0x00000000      |
| 0x2FF                 | 0x00000000                 | 0x00000800      |                         |

Once the MTRR have been written, the cache is enabled.

### Register setup

Now the 2BL will set up the segment registers and stack:

| Register | Value      | Notes        |
|----------|------------|--------------|
| ds       | 0x0010     | Data segment |
| es       | 0x0010     |
| ss       | 0x0010     |
| esp      | 0x00400000 |              |
| fs       | 0x0000     |              |
| gs       | 0x0000     |              |

### Self-copy

Now the 2BL copies itself (24 kiB) from 0x00900000 to memory address
0x00400000.

### Paging

Now a PDE is prepared at address 0x0000F000:

<table>
<thead>
<tr class="header">
<th><p>Offset in PDE</p></th>
<th><p>Value</p></th>
<th><p>Notes</p></th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><p>0x000</p></td>
<td><p>0x000000E3</p></td>
<td><p>Identity maps the first 256MiB of RAM: 0x00000000 and 0x80000000 will both map to physical page 0<br />
<br />
0xE3: Flags:<br />
* 0x80: 4 MiB page<br />
* 0x40: Marked as previously written (Dirty)<br />
* 0x20: Marked as previously accessed<br />
* 0x02: Read/Write<br />
* 0x01: Present</p></td>
</tr>
<tr class="even">
<td><p>0x800</p></td>
<td><p>0x000000E3</p></td>
</tr>
<tr class="odd">
<td><p>0x004</p></td>
<td><p>0x004000E3</p></td>
</tr>
<tr class="even">
<td><p>0x804</p></td>
<td><p>0x004000E3</p></td>
</tr>
<tr class="odd">
<td><p>...</p></td>
</tr>
<tr class="even">
<td><p>0x8FC</p></td>
<td><p>0x0FC000E3</p></td>
</tr>
<tr class="odd">
<td><p>0x0FC</p></td>
<td><p>0x0FC000E3</p></td>
</tr>
<tr class="even">
<td><p>0x900</p></td>
<td><p>0x00000000</p></td>
<td><p>Unmapping the rest of the pages</p></td>
</tr>
<tr class="odd">
<td><p>0x100</p></td>
<td><p>0x00000000</p></td>
</tr>
<tr class="even">
<td><p>...</p></td>
</tr>
<tr class="odd">
<td><p>0xFFC</p></td>
<td><p>0x00000000</p></td>
</tr>
<tr class="even">
<td><p>0x7FC</p></td>
<td><p>0x00000000</p></td>
</tr>
<tr class="odd">
<td><p>0xC00</p></td>
<td><p>0x0000F063</p></td>
<td><p>Maps the PDE (4 kiB page) to address 0xC0000000<br />
<br />
0x63: Flags:<br />
* 0x40: Marked as previously written (Dirty)<br />
* 0x20: Marked as previously accessed<br />
* 0x02: Read/Write<br />
* 0x01: Present</p></td>
</tr>
<tr class="even">
<td><p>0xFFC</p></td>
<td><p>0xFFC000E3</p></td>
<td><p>Identity maps the upper portion of the Flash (4 MiB page) to address 0xFFC00000<br />
<br />
0xE3: Flags:<br />
* 0x80: 4 MiB page<br />
* 0x40: Marked as previously written (Dirty)<br />
* 0x20: Marked as previously accessed<br />
* 0x02: Read/Write<br />
* 0x01: Present</p></td>
</tr>
<tr class="odd">
<td><p>0xFD0</p></td>
<td><p>0xFD0000FB</p></td>
<td><p>Maps 16 MiB for the GPU control registers<br />
<br />
0xFB: Flags:<br />
* 0x80: 4 MiB page<br />
* 0x40: Marked as previously written (Dirty)<br />
* 0x20: Marked as previously accessed<br />
* 0x10: Cache disabled<br />
* 0x08: Write-Through caching<br />
* 0x02: Read/Write<br />
* 0x01: Present</p></td>
</tr>
<tr class="even">
<td><p>0xFD4</p></td>
<td><p>0xFD4000FB</p></td>
</tr>
<tr class="odd">
<td><p>0xFD8</p></td>
<td><p>0xFD8000FB</p></td>
</tr>
<tr class="even">
<td><p>0xFDC</p></td>
<td><p>0xFDC000FB</p></td>
</tr>
</tbody>
</table>

After setting up the PDE, the PAT is set up using `wrmsr`:

CR4 is touched

CR3 is touched

Now paging is activated by enabling the PG and WP bits in CR0.
Additionally, the same `or` instruction is used to enable the NE bit in
cr0.

### 2BL main

esp is now also reloaded to point at the relocated address. It will be
set to 0x80400000 (absolute value, independent of previous esp value).
The 2BL will now `call` into the relocated 2BL code somewhere near
0x00400000.

#### Disabling of the MCPX ROM

    out32(0xCF8, 0x80000880);
    out8(0xCFC, 0x02);

#### SMC handling

The [SMC](/wiki/SMC "wikilink") has a watchdog functionality which must be
turned off. This is done by querying the SMC registers 0x1C - 0x1F. If
all of them are 0x00 the 2BL will shutdown the system. If this is not
the case, the bootloader calculates the watchdog challenge response and
sends it to SMC registers 0x20 and 0x21.

Additionally, the 2BL will set SMC register 0x01 to 0 (which resets the
cursor position for reading the SMC revision information).

#### Enable IDE and NIC

    out32(0xCF8, 0x8000088C);
    out32(0xCFC, 0x40000000);

#### Memory cleanup

The 2BL fills memory with 0xCC from 0x80090000 to 0x80095FFF. These are
the 24 kiB where the 2BL was stored previously.

#### Setup RAM timing

Not described yet, this is complicated. This got a lot more complicated
when Microsoft started using different RAM sometime after [Hardware
Revision 1.6](/wiki/Hardware_Revisions#1.6 "wikilink") was already out.

#### Weird stuff 2

This does some PCI config, use unknown.

    out32(0xCF8, 0x80000854);
    out32(0xCFC, in32(0xCFC) | 0x88000000);

    out32(0xCF8, 0x80000064);
    out32(0xCFC, in32(0xCFC) | 0x88000000);

    out32(0xCF8, 0x8000006C);
    uint32_t tmp = in32(0xCFC);
    out32(0xCFC, tmp & 0xFFFFFFFE);
    out32(0xCFC, tmp);

    out32(0xCF8, 0x80000080);
    out32(0xCFC, 0x00000100);

#### Weird stuff 3

    out32(0xCF8, 0x80000808);
    uint8_t mcpx_revision = in8(0xCFC);    

    if (mcpx_revision >= 0xD1) {
      out32(0xCF8, 0x800008C8);
      out32(0xCFC, 0x00008F00);
    }

#### Loading the kernel

##### Kernel-copy

The Kernel is now copied into RAM.

##### Kernel decryption

The 2BL will copy the kernel decryption key (16 bytes) from offset 32 of
an array of 3 keys:

| Offset | Use             |
|--------|-----------------|
| 0      | EEPROM key      |
| 16     | Certificate key |
| 32     | Kernel key      |

The Kernel is then decrypted in-place using RC4.

##### Kernel decompression

The Kernel is decompressed directly to 0x80010000 where it will reside
until a full system shutdown.

#### Running the kernel

The xboxkrnl.exe header at 0x8001000 is checked. If it is invalid, . If
it is valid, the kernel entry point is looked up from the PE optional
header. The hardcoded image base of 0x8001000 is added to the entry
point. The entry-point is now being called. Argumnts are passed on the
stack, from right to left. The first argument is a commandline string
loaded from memory address 0x80400000. It is an empty string for retail
BIOS. A pointer to the previously mentioned array of 3 keys is passed as
the second argument.

Kernel
------

### Initialization

#### Stage 1 (Cold-boot only)

The entry point to the kernel will first parse the arguments. At the
end, the kernel will call the initialization routine for what we'll
refer to as: Stage 2a.

#### Stage 2 (Cold-boot only)

The kernel initialization will only happen once on a cold-boot. It will
not happen for reboots.

-   ebp is set to 0x00000000
-   esp is modified
-   GDT is prepared and loaded
-   cs and ds are reloaded
-   fs is set
-   TSS is loaded
-   cr3 is moved to 3 tasks
-   The CPU microcode is updated

After this comes Stage 3 initialization which will also be repeated on
kernel re-initialization.

#### Stage 3

This is code which is duplicated in INIT and .text sections.

-   In the INIT section it directly follows the Stage 2 initialization.
-   In the .text section it follows the Kernel re-initialization code
    mentioned below.

This code does the following:

-   IDT is prepared and loaded
-   

### Re-initialization

On reboots, initialization Stage 1 and 2 are not in memory anymore (as
the INIT section has been discarded), and can't be run anymore. Instead,
a seperate function replaces their functionality and then jumps directly
to Stage 3 initalization.

This code is the partial kernel reinitialization, which will be ran on
reboots using
[Kernel/HalReturnToFirmware](/wiki/Kernel/HalReturnToFirmware "wikilink").

-   ebp is set to 0x00000000
-   esp is modified
-   Some memory stuff in a seperate function
-   The .data section from [Flash](/wiki/Flash "wikilink") is loaded and
    replaces the running .data
-   The byte infront of KeSystemTime is set to 0x01, indicating the
    system comes from a reboot.

After this has completed, [\#Stage 3](#Stage_3 "wikilink") of the kernel
initialization will take over.

### Startup animation

References
----------

-   [Understanding the Xbox boot
    process](http://hackspot.net/XboxBlog/?p=1)
-   [Deconstructing the Boot
    ROM](https://mborgerson.com/deconstructing-the-xbox-boot-rom)

