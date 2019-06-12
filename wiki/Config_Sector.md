---
title: Config Sector
permalink: wiki/Config_Sector/
layout: wiki
---

| Offset | Size | Description                    |
|--------|------|--------------------------------|
| 0x1020 | 4    | Static IP Address              |
| 0x1024 | 4    | Static Subnet Mask             |
| 0x1028 | 4    | Static Default Gateway         |
| 0x102C | 4    | Static Primary DNS Server      |
| 0x1030 | 4    | Static Secondary DNS Server    |
| 0x1034 | 32   | (Unknown padding)              |
| 0x1054 | 4    | Xbox Live IP Address           |
| 0x1058 | 4    | Xbox Live Subnet Mask          |
| 0x105C | 4    | Xbox Live Default Gateway      |
| 0x1060 | 4    | Xbox Live Primary DNS Server   |
| 0x1064 | 4    | Xbox Live Secondary DNS Server |
| 0x1068 | 40   | Xbox Live Hostname             |
| 0x1090 | 64   | Xbox Live PPPOE Username       |
| 0x10D0 | 64   | Xbox Live PPPOE Password       |
| 0x1110 | 40   | (Unknown padding)              |
| 0x1138 | 40   | Xbox Live PPPOE Service Name   |
| 0x1160 | 12   | (Unknown)                      |
| 0x116C | 4    | DHCP IP Address                |
| 0x1170 | 4    | DHCP Subnet Mask               |
| 0x1174 | 4    | DHCP Default Gateway           |
| 0x1178 | 4    | DHCP Primary DNS Server        |
| 0x117C | 4    | DHCP Secondary DNS Server      |
| 0x1180 | 4    | PPPOE IP Address (?)           |
| 0x1184 | 4    | PPPOE Subnet Mask (?)          |
| 0x1188 | 4    | PPPOE Gateway Address (?)      |
| 0x118C | 4    | PPPOE Primary DNS Server (?)   |
| 0x1190 | 4    | PPPOE Secondary DNS Server (?) |

References
----------

-   [Xbox live (accounts, xqemu and MU) | ASSEMbler - Home of the
    obscure
    (archive.org)](https://web.archive.org/web/20190604222002/https://assemblergames.com/threads/xbox-live-accounts-xqemu-and-mu.60352/)

