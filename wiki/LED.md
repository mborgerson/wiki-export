---
title: LED
permalink: wiki/LED/
layout: wiki
---

The communication protocol is documented at
<https://xboxdevwiki.net/PIC#The_LED>

The Xbox's front LED is driven by the PIC16LC63A (SMC) motherboard
component. Upon startup, a set of 4 time slots are reserved for it and
looped in succession. The first three slots consume 180 ms
(milliseconds) and the last 200 ms for a total of 740 ms per full LED
cycle.

The time slot used for the first color in a cycle depends on when
(specific timing TBD) the SMBus set led command was executed relative to
the PIC startup, therefore we cannot easily predict which color gets the
additional 20 ms via emulation at the moment; what we can assume however
is each full cycle takes 740 ms.

An LED cycle can be interrupted (specific timing TBD) but the currently
shown color is displayed for its full time slot before honoring the new
cycle request.
