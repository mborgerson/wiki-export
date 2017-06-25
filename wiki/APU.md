---
title: APU
permalink: wiki/APU/
layout: wiki
---

The [MCPX](/wiki/MCPX "wikilink") contains an APU (Audio Processing Unit).

-   SSL = Stream Segment List
-   SGE = Scatter Gather Entry
-   PRD = Physical Resource Descriptor (Same thing as SGE?!)

Global Processor (GP)
---------------------

-   GPS = GP Scratch (?)
-   GPF = GP FIFO

Encode Processor (EP)
---------------------

-   EPS = EP Scratch (?)
-   EPF = EP Frames (?)

Voice Processor (VP)
--------------------

-   VPV = VP Voices
-   VPSGE = VP SGEs
-   VPSSL = VP SSLs

### Voice lists

-   2D
-   3D
-   MP

### Voice structure

This is 0x80 bytes

#### Pitch calculation

    p = 4096 * log2(f / 48000)
    f = pow2(p / 4096) * 48000

Related
-------

-   [ACI](/wiki/ACI "wikilink")
-   [DSP](/wiki/DSP "wikilink")
-   [Xbox ADPCM](/wiki/Xbox_ADPCM "wikilink")
-   [Script to inspect APU registers and voice buffers (See master
    revision for
    updates)](https://github.com/JayFoxRox/xbox-tools/blob/5e114f24e4a83bd626f31674e024f439f3709d19/python-scripts/dsp.py)

