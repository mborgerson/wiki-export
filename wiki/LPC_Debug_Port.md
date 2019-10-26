---
title: LPC Debug Port
permalink: wiki/LPC_Debug_Port/
layout: wiki
---

| Name          | Pin | Pin | Name     |
|---------------|-----|-----|----------|
| SERIRQ (v1.0) | 16  | 15  | 3.3V     |
| SDA           | 14  | 13  | SCL      |
| GND           | 12  | 11  | LAD0     |
| LAD1          | 10  | 9   | 3.3V     |
| LAD2          | 8   | 7   | LAD3     |
| 5V            | 6   | 5   | LRESET\# |
| PWR (v1.6)    | 4   | 3   | LFRAME\# |
| GND           | 2   | 1   | LCLK     |

The **LPC Debug Port** is an unpopulated 2x8 2.54mm (0.1") footprint on
the Xbox motherboard that provides access to the system's [Low Pin
Count](https://en.wikipedia.org/wiki/Low_Pin_Count) Bus, as well as the
[SMBus](/wiki/SMBus "wikilink"). Throughout the Xbox's lifetime, Microsoft
modified the LPC port in an attempt to prevent modding and, in some
cases, it must be “rebuilt” to restore the port's full functionality.
Rebuilding the LPC debug port involves soldering wires between it and
very small vias on the motherboard.

The LPC bus is controlled by the [MCPX southbridge](/wiki/MCPX "wikilink"),
which conforms to the Intel Low Pin Count Specification 1.0.

Pins
----

-   **3.3V** (15 & 9) - Provides 3.3V while the Xbox is powered on. On
    1.6 hardware, pin 9 is disconnected. Reconnect to pin 15 to restore
    the supply voltage.
-   **SDA** (14) - [SMBus](/wiki/SMBus "wikilink") data and address signal.
-   **SCL** (13) - [SMBus](/wiki/SMBus "wikilink") clock signal.
-   **GND** (12 & 2) - Ground.
-   **LAD\[3:0\]** (11, 10, 8, 7) - LPC address and data signals.
-   **5V** (6) - Provides 5V while the Xbox is powered on or v1.6 always
    on when AC plugged in.
-   **LRESET\#** (5) - LPC reset signal; same as PCIRST\#.
-   **PWR** (4) - On pre-1.6 hardware, this pin does not physically
    exist. On 1.6 hardware, this pin is connected to the power button.
    Shorting it to ground will turn the Xbox on and off.
-   **LFRAME\#** (3) - LPC start-of-cycle signal. On 1.3-1.5 hardware,
    this pin is disconnected. See [LFRAME\#
    reconnect](/wiki/LPC_Debug_Port#LFRAME.23_reconnect "wikilink") for a
    possible reconnection.
-   **LCLK** (1) - 33MHz LPC clock signal; same as PCICLK.

The debug port lacks the optional LDRQ\#, SERIRQ, CLKRUN\#, PME\#,
LPCPD\#, and LSMI\# signals. This means peripherals connected to the
debug port cannot utilize interrupt, DMA, bus mastering, or power
management features.

LFRAME\# reconnect
------------------

<img src="LFRAME-for-v1_3-and-up.png" title="Disconnected LFRAME# can be reconstructed with via found on all MCPX revisions" alt="Disconnected LFRAME# can be reconstructed with via found on all MCPX revisions" width="200" />

It is possible to reconnect the LFRAME\# signal to the LPC port on
1.3-1.5 hardware by tapping the MCPX soldermask and soldering a jumper
wire from the via to the disconnected pin on the LPC port.

Result should look similar to this:

<img src="Dreimer-lframe-v1-3-reconnect.jpg" title="Image provided by dreimer" alt="Image provided by dreimer" width="500" />

In the end, we can gain kernel debug output from Super I/O serial again:

<img src="Dreimer-lframe-v1-3-windbg.png" title="Image provided by dreimer" alt="Image provided by dreimer" width="500" />

This reconnection was tested on 1.3 hardware, and should also work with
the later 1.4 and 1.5 revisions.

References
----------

-   [LFRAME-for-v1\_3-and-up.pdf](https://web.archive.org/web/20140805105234/http://dwl.xbox-scene.com/~xbox/xbox-scene/tutorials/LFRAME-for-v1_3-and-up.pdf)
    An old manual from Xbox-Scene that explains how to reconnect
    **LFRAME\#** (3) for a Cheapmod. This we don't care about, but is
    what we want for [kernel debugging](/wiki/Kernel_Debug "wikilink") via
    [Super I/O](/wiki/Super_I/O "wikilink"). Thanks to dreimer for finding and
    testing this documentation.

