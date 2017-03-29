---
title: Memory
permalink: wiki/Memory/
layout: wiki
---

The Xbox has 64MB Memory. This could be expanded to 128MB of memory (and
the motherboard has empty spots where these could have been) but no
games took advantage of it. The debug Xbox and the Chihiro both
contained 128MB Memory.

The memory was shared between the CPU and GPU. On the retail Xbox, the
[BIOS](/wiki/BIOS "wikilink") and [MCPX ROM](MCPX_ROM "wikilink") are also
mapped to memory at the top 16MB and the top 512bytes respectively. On
Debug Xboxs and Chihiro, only the BIOS is mapped.

| Memory Type | Retail Xbox Range       | Debug/Chihiro Range     |
|-------------|-------------------------|-------------------------|
| Main Memory | 0x00000000 - 0x04000000 | 0x00000000 - 0x08000000 |
| BIOS        | 0xFF000000 - 0xFFFFFFFF | 0xFF000000 - 0xFFFFFFFF |
| MCPX        | 0xFFFFFE00 - 0xFFFFFFFF | N/A                     |

Code for emulating the memory might consist of:

    #ifdef DEBUG || CHIHIRO
    #define MAIN_MEMORY 128 * 1024 * 1024
    int mcpx_active = 0;
    #else
    #define MAIN_MEMORY 64 * 1024 * 1024
    int mcpx_active = 1;
    #endif

    #define BIOS_SIZE 256 * 1024
    #define BIOS_MEMORY_SIZE 16 * 1024 * 1024
    #define BIOS_MEMORY (0xFFFFFFFF - BIOS_MEMORY_SIZE + 1)
    #define MCPX_SIZE   0x200
    #define MCPX_MEMORY (0xFFFFFFFF - MCPX_SIZE + 1)

    uint8_t memory[MAIN_MEMORY] = {0};
    uint8_t bios[BIOS_SIZE] = {0};
    uint8_t mcpx[MCPX_SIZE] = {0};

    uint8_t get_memory_byte(uint32_t location) {
        if (location < MAIN_MEMORY) {
            return memory[location];
        }

        if (mcpx_active && location >= MCPX_MEMORY) {
            return mcpx[location - MCPX_MEMORY];
        }

        if (location >= BIOS_MEMORY) {
            return bios[(location - BIOS_MEMORY) % BIOS_SIZE];
        }

        printf("Memory in unspecified range: %08X\n", location);
        return 0;
    }

    uint16_t get_memory_word(uint32_t location) {
        return get_memory(location + 1) << 8 | get_memory(location);
    }

    uint32_t get_memory_dword(uint32_t location) {
        return get_memory_word(location + 2) << 16 | get_memory_word(location);
    }

    void deactivate_mcpx() {
        mcpx = 0;
    }
