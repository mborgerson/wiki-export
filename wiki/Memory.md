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
[Flash](/wiki/Flash "wikilink") and [MCPX ROM](/wiki/MCPX_ROM "wikilink") are also
mapped to memory at the top 16 MiB and the top 512 Bytes respectively.
On Debug Xboxs and Chihiro, only the Flash is mapped as they don't
contain an MCPX ROM.

| Memory Type                            | Retail Xbox Range       | Debug/Chihiro Range     |
|----------------------------------------|-------------------------|-------------------------|
| Main Memory                            | 0x00000000 - 0x04000000 | 0x00000000 - 0x08000000 |
| [GPU (NV2A) Registers](/wiki/GPU "wikilink") | 0xFD000000 - 0xFE000000 |
| APU Registers                          | 0xFE800000 - 0xFE880000 |
| ACI (AC97) Registers                   | 0xFEC00000 - 0xFEC01000 |
| USB 0 Registers                        | 0xFED00000 - 0xFED01000 |
| USB 1 Registers                        | 0xFED08000 - 0xFED09000 |
| NIC (NVNet) Registers                  | 0xFEF00000 - 0xFEF00400 |
| [Flash](/wiki/Flash "wikilink")              | 0xFF000000 - 0xFFFFFFFF |
| [MCPX ROM](/wiki/MCPX_ROM "wikilink")        | 0xFFFFFE00 - 0xFFFFFFFF | N/A                     |

Code for emulating the memory might consist of:

    #ifdef DEBUG || CHIHIRO
    #define MEMORY_SIZE 128 * 1024 * 1024
    int mcpx_active = 0;
    #else
    #define MEMORY_SIZE 64 * 1024 * 1024
    int mcpx_active = 1;
    #endif

    #define FLASH_SIZE 256 * 1024
    #define FLASH_MAP_SIZE 16 * 1024 * 1024
    #define FLASH_MAP_ADDRESS (0xFFFFFFFF - FLASH_MAP_SIZE + 1)

    #define MCPX_SIZE   0x200
    #define MCPX_MAP_ADDRESS (0xFFFFFFFF - MCPX_MAP_SIZE + 1)

    uint8_t memory[MEMORY_SIZE] = {0};
    uint8_t flash[FLASH_SIZE] = {0};
    uint8_t mcpx[MCPX_SIZE] = {0};

    uint8_t get_memory_byte(uint32_t location) {
        if (location < MEMORY_SIZE) {
            return memory[location];
        }

        if (mcpx_active && location >= MCPX_MAP_ADDRESS) {
            return mcpx[location - MCPX_MAP_ADDRESS];
        }

        if (location >= FLASH_MAP_ADDRESS) {
            return flash[(location - FLASH_MAP_ADDRESS) % FLASH_SIZE];
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
        mcpx_active = 0;
    }
