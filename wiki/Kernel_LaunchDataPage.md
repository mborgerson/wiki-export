---
title: Kernel/LaunchDataPage
permalink: wiki/Kernel/LaunchDataPage/
layout: wiki
---

LaunchDataPage is a pointer to a memory page (4096 Bytes) which is
persisted across reboots. The pointer might be `NULL` until the memory
page is allocated. Allocation and persisting the page is responsibility
of the application.

    struct {
      uint32_t launch_data_type;
      uint32_t title_id;
      char launch_path[520];
      uint32_t flags;
      uint8_t pad[492];
      uint32_t launch_data[3072];
    }* LaunchDataPage;

Launch data
-----------

### Type 0x00000000: Switch to title

Will reboot to a new XBE. The data is application dependent. Official
game demos should respect additional data:

    struct LaunchData00000000 {
      uint32_t id;
      uint32_t runmode; // 0x00000001 = Kiosk / 0x00000002 = Normal (user selected)
      uint32_t timeout;
      char launcher_xbe_path[64];
      char title_xbe_path[64];
      uint8_t padding[3072 - 140];
    };

### Type 0x00000001: Switch to dashboard

Will reboot to the [Dashboard](/wiki/Dashboard "wikilink").

    struct LaunchData00000001 {
      uint32_t reason;
      uint32_t context;
      uint32_t parameters[2];
      uint8_t padding[3072 - 16];
    };

| `reason`                                 | `parameters[0]`                | `parameters[1]` | Note |
|------------------------------------------|--------------------------------|-----------------|------|
| 0 = Show Dashboard                       |                                |                 |
| 1 = Error                                | 1 = Invalid XBE                |                 |      |
| 2 = Invalid Harddrive                    |                                |                 |
| 3 = Region                               |                                |                 |
| 4 = Parental control                     |                                |                 |
| 5 = Media type bad                       | Rating which caused this error |                 |
| 2 = Savedata                             |                                |                 |      |
| 3 = Settings (flags)                     | 0x01 = Clock                   |                 |      |
| 0x02 = Timezone                          |                                |                 |
| 0x04 = Language                          |                                |                 |
| 0x08 = Video                             |                                |                 |
| 0x10 = Audio                             |                                |                 |
| 4 = Music                                |                                |                 |      |
| 5                                        |                                |                 |      |
| 6 = Network Configuration                |                                |                 |      |
| 7 = Create new account                   |                                |                 |      |
| 8 = MessageServerInfo                    |                                |                 |      |
| 9 = Show policies (flags)                | 0x01 = Subscription agreement  |                 |      |
| 0x02 = Terms of use                      |                                |                 |
| 0x04 = Code of conduct                   |                                |                 |
| 0x08 = Privacy statement                 |                                |                 |
| 10 = Online menu                         |                                |                 |      |
| 11 = Force change of account name        |                                |                 |      |
| 12 = Force change of billing information |                                |                 |      |

### Type 0x00000002: Switch from dashboard

This passes back the context which was originally passed to the
dashboard.

    struct LaunchData00000002 {
      uint32_t context;
      uint8_t padding[3072 - 4];
    };

### Type 0x00000003: Run from debugger

    struct LaunchData00000003 {
      char command_line[3072];
    };

### Type 0x00000004: Switch to title update

### Type 0x00000006: Switch from title update

    struct LaunchData00000006 {
      uint32_t context;
      uint32_t hr; // Microsoft result code
      char padding[3072 - 8];
    };

### Type 0xFFFFFFFF: None

`launch_data` remains unused.
