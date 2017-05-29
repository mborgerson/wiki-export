---
title: Boot Process
permalink: wiki/Boot_Process/
layout: wiki
---

Overview
--------

MCPX
----

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

### RC4 Decryption of the 2BL

Decryption of the 2BL seems to happen in 4 stages.

#### Stage 1

Initialising the working space. The MCPX ROM seems to just write 1, 2,
3, 4.... 253, 254, 255 in memory addresses 0x8f000 to 0x850FF. This
might look something like:

    void init_rc4() {
        uint32_t stack_pointer = 0x8f000;

        for (int iterator = 0; iterator <= 255; iterator++) {
            set_memory_byte(stack_pointer + iterator, iterator);
        }
    }

#### Stage 2

Preparing for decryption. This gets the key from memory location
0xFFFFFFA5 and starts preparing and environment for decryption of the
2BL.

    void load_key() {
        uint32_t key_location = 0xffffffa5;
        uint32_t stack_pointer = 0x8f000;
        uint8_t i, j = 0;

        for (int iterator = 0; iterator <= 255; iterator++) {
            i = get_memory_byte(iterator + stack_pointer);
            j += i + get_memory_byte(key_location + (iterator % 16));
            set_memory_byte(iterator+stack_pointer, get_memory_byte(j+stack_pointer));
            set_memory_byte(j+stack_pointer, i);
        }
    }

#### Stage 3

Perform the decryption. The MCPX reads the 2BL from 0xFFFF9E00 and
decrypts it to 0x00090000. It is 24KiB in size.

    void decrypt() {
        uint32_t stack_pointer = 0x0008F000;
        uint32_t encrypted = 0xFFFF9E00;
        uint32_t decrypted = 0x00090000;

        uint8_t a, b, j, i = 0;

        i = get_memory_byte(stack_pointer + 0x100); // 0
        j = get_memory_byte(stack_pointer + 0x101); // 0

        for (int edi = 0; edi <= 0x6000; ++edi) {
            ++i;

            a = get_memory_byte(stack_pointer + i);
            j += a;
            b = get_memory_byte(stack_pointer + j);
            set_memory_byte(stack_pointer + i, b);
            set_memory_byte(stack_pointer + j, a);
            a += b;
            b = get_memory_byte(edi + encrypted);
            a = get_memory_byte(stack_pointer + a);
            b ^= a;
            set_memory_byte(edi + decrypted, b);
        }
    }

#### Stage 4

Verification. Finally the MCPX reads a string from the un-encrypted 2BL
and compares it to a magic number. If it matches, all was successful,
and we jump to the start of the 2BL to start decrypting the kernel.

    void verify() {
        if (get_memory_dword(0x95FE4) == MAGIC_NUMBER) {
            eip = 0x900000;
        } else {
            // Else, things have gone wrong
            eip = 0xFFFFFF94;
        }
    }

### Notes

The RC4 algorithm was included as part of MCPX 1.0 and seems to work
fine with BIOS versions 3944, 4034, and 4134.

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

#### SMC handling

The [SMC](/wiki/SMC "wikilink") has a watchdog functionality which must be
turned off. This is done by querying the SMC registers 0x1C - 0x1F. If
all of them are 0x00 the 2BL will shutdown the system. If this is not
the case, the bootloader calculates the watchdog challenge response and
sends it to SMC registers 0x20 and 0x21.

Additionally, the 2BL will set SMC register 0x01 to 0 (which resets the
cursor position for reading the SMC revision information).

#### Weird stuff 0

#### Memory cleanup

The 2BL fills memory with 0xCC from 0x80090000 to 0x80095FFF. These are
the 24 kiB where the 2BL was stored previously.

#### Setup RAM timing

Not described yet, this is complicated. This got a lot more complicated
when Microsoft started using Hynix RAM instead of Samsung RAM sometime
after [Hardware Revision 1.6](/wiki/Hardware_Revisions#1.6 "wikilink") was
already out. These Xboxes with non-Samsung RAM are sometimes referred to
as 1.6b by the modding community.

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

### Startup animation

### Kernel Re-initialization

References
----------

-   [Understanding the Xbox boot
    process](http://hackspot.net/XboxBlog/?p=1)
-   [Deconstructing the Boot
    ROM](https://mborgerson.com/deconstructing-the-xbox-boot-rom) (I
    cannot express enough just how helpful this page is)

