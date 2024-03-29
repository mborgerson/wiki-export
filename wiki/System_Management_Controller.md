---
title: System Management Controller
permalink: wiki/System_Management_Controller/
layout: wiki
---

The System Management Controller (SMC) is a PIC16LC63A-04/SO
microcontroller which handles a variety of tasks on the Xbox. This
includes rebooting the system, returning the connected kind of video
cable, the DVD tray state, controlling the fan, LED control and sensing
temperature. It is also the hardware which is connected to the Power and
Eject buttons. The PIC is running at 20 MHz with its own ROM, RAM and
I/O lines.

The PIC is always running, even if the Xbox is turned off. When the
power cable is unplugged, it gets its energy from a capacitor for some
hours.

It is connected via I²C and located on address 0x10.

Revisions
---------

The chip is also marked with a revision. Known revisions include:

-   P01
-   P05
-   P11
-   P2L
-   D01 (Seen in a debug kit)
-   D05 (seen in a earlier model chihiro)

