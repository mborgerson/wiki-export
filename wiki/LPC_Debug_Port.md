---
title: LPC Debug Port
permalink: wiki/LPC_Debug_Port/
layout: wiki
---

| Name       | Pin | Pin | Name     |
|------------|-----|-----|----------|
| -          | 16  | 15  | 3.3V     |
| SDA        | 14  | 13  | SDL      |
| GND        | 12  | 11  | LAD0     |
| LAD1       | 10  | 9   | 3.3V     |
| LAD2       | 8   | 7   | LAD3     |
| 5V         | 6   | 5   | LRESET   |
| PWR (v1.6) | 4   | 3   | LFRAME\# |
| GND        | 2   | 1   | LCLK     |

The **LPC Debug Port** is an unpopulated 2x8 2.54mm (0.1") footprint on
the Xbox motherboard that provides access to the system's Low Pin Count
Bus, as well as the [SMBus](/wiki/SMBus "wikilink"). Throughout the Xbox's
lifetime, the LPC port has been modified in an attempt to prevent
modding and, in some cases, it must be “rebuilt” to restore the port's
full functionality. Rebuilding the LPC debug port involves soldering
wires between it and very small vias on the motherboard.

Pins
----

-   **3.3V** (15 & 9) - Provides 3.3V when the Xbox is powered on.
-   **SDA** (14) - [SMBus](/wiki/SMBus "wikilink") data and address signal.
-   **SCL** (13) - [SMBus](/wiki/SMBus "wikilink") clock signal.
-   **GND** (12 & 2) - Ground.
-   **LAD\[3:0\]** (11, 10, 8, 7) - LPC address and data signals.
-   **5V** (6) - Provides 5V as long as the Xbox is plugged in.
-   **LRESET\#** (5) - LPC reset signal.
-   **PWR** (4) - On pre-1.6 hardware, this pin does not exist. On 1.6
    hardware, this pin is connected to the power button. Shorting it to
    ground will turn the Xbox on and off.
-   **LFRAME\#** (3) - LPC start-of-cycle signal.
-   **LCLK** (2) - LPC clock signal.

