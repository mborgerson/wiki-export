---
title: Kernel/LaunchDataPage
permalink: wiki/Kernel/LaunchDataPage/
layout: wiki
---

### Switch to Dashboard

    struct LaunchDataPage_Dashboard {
      uint32_t reason;
      uint32_t context;
      uint32_t parameters[2];
      uint8_t padding[3072 - 16];
    };

| `reason`            | `parameters[0]`                | `parameters[1]` | Note |
|---------------------|--------------------------------|-----------------|------|
| 0 = MainMenu        |                                |                 |
| 1 = Error           | 1 = InvalidXbe                 |                 |      |
| 2 = InvalidHdd      |                                |                 |
| 3 = Region          |                                |                 |
| 4 = ParentalControl |                                |                 |
| 5 = MediaType       | Rating which caused this error |                 |
| 2 = Savedata        |                                |                 |      |
| 3 = Settings flags  | 0x01 = Clock                   |                 |      |
| 0x02 = Timezone     |                                |                 |
| 0x04 = Language     |                                |                 |
| 0x08 = Video        |                                |                 |
| 0x10 = Audio        |                                |                 |
| 4 = Music           |                                |                 |      |


