---
title: Fatal Error
permalink: wiki/Fatal_Error/
layout: wiki
---

These are the errors which will be displayed
<img src="Fatal_Error_13.png" title="fig:Fatal Error 13 being displayed" alt="Fatal Error 13 being displayed" width="300" />

Checked after cold-boot, in this particular order:

| Value (2 digit decimal) | Step / Condition                                             |
|-------------------------|--------------------------------------------------------------|
| 07                      | Harddrive never left busy state (after reset from cold-boot) |
| 09                      | Setting Harddrive PIO mode failed                            |
| 09                      | Setting Harddrive DMA mode failed                            |
| 08                      | Harddrive ATA identify device command failed                 |
| 09                      | Harddrive is too small for cache partitions                  |
| 06                      | Unable to unlock Harddrive                                   |
| 08                      | Harddrive ATA identify device after unlock command failed    |
| 08                      | Setting Harddrive device parameters failed                   |
| 10                      | DVD Drive never left busy state (after reset from cold-boot) |
| 12                      | Setting DVD Drive PIO mode failed                            |
| 12                      | Setting DVD Drive DMA mode failed                            |
| 11                      | DVD Drive ATAPI identify device command failed               |
| 05                      | Harddrive not locked; Not reported if either:                
                                                                
  -   XBE is allowed to run from unlocked HDD                   
  -   Xbox is in manufacturing region                           
  -   Kernel is made for Chihiro                                
  -   Kernel is made for XDK                                    |

Additional error codes, or no precise order known:

| Value (2 digit decimal) | Generic meaning                                                                                            |
|-------------------------|------------------------------------------------------------------------------------------------------------|
| 00                      | No error (used internally to clear error flags from EEPROM)                                                |
| 01                      |                                                                                                            |
| 02                      | EEPROM Checksum failure                                                                                    |
| 03                      |                                                                                                            |
| 04                      | RAM bad                                                                                                    |
| 13                      | Dashboard broken, while [Kernel/LaunchDataPage](/wiki/Kernel/LaunchDataPage "wikilink") `reason == 0` (MainMenu) |
| 14                      | Dashboard broken, while [Kernel/LaunchDataPage](/wiki/Kernel/LaunchDataPage "wikilink") `reason == 1` (Error)    |
| 15                      |                                                                                                            |
| 16                      | Dashboard settings error                                                                                   |
| 17                      |                                                                                                            |
| 18                      |                                                                                                            |
| 19                      |                                                                                                            |
| 20                      | Dashboard failed to launch, but DVD security check was successful                                          |
| 21                      | Used to manually display fatal error after reboot (on purpose)                                             |


