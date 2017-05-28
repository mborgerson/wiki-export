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
decrypts it to 0x90000. It is 24K in size.

    void decrypt() {
        uint32_t stack_pointer = 0x8f000;
        uint32_t encrypted = 0xFFFF9E00;
        uint32_t decrypted = 0x90000;

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

The MTRR (Memory Type Range Register) will be setup (using `wrmsr`) in
the following way:

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

### GDT setup

### Paging

### Kernel decryption

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

