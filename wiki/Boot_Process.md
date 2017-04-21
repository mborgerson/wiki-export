---
title: Boot Process
permalink: wiki/Boot_Process/
layout: wiki
---

If we wish to HLE the MCPX, then there are certain things that can be
ignored, for example, getting the CPU to 32 bit protected mode and
enabling caching. The xcode interpreter is common through both versions
of the MCPX ROM. The high level interpretation of the MCPX ROM might
look like this:

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

References
----------

-   [Deconstructing the Boot
    ROM](https://mborgerson.com/deconstructing-the-xbox-boot-rom) (I
    cannot express enough just how helpful this page is)
