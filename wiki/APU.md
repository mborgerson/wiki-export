---
title: APU
permalink: wiki/APU/
layout: wiki
---

The [MCPX](/wiki/MCPX "wikilink") contains an APU (Audio Processing Unit).

-   SSL = Stream Segment List
-   SGE = Scatter Gather Entry
-   PRD = Physical Resource Descriptor (Same thing as SGE?!)

Voice Processor (VP)
--------------------

A powerful voice processor. There can be up to 256 voices and 64 of
those can be 3D.

Per-voice settings:

-   Input type (8bit, 16bit, 24bit, ADPCM)
-   [Head-related transfer
    function](wikipedia:Head-related_transfer_function "wikilink")
    (HRTF)
-   [Low-frequency
    oscillation](wikipedia:Low-frequency_oscillation "wikilink") (LFO)
-   Pitch
-   2x Pitch (?) envelope
-   2x LFO (?) envelope
-   8 target bins, each with a custom volume for this voice

There are 32 bins which these voices will be mixed into.

### Related APU memory

-   VPV = VP Voices
-   VPHT = VP HRTF Target
-   VPHC = VP HRTF Current
-   VPSGE = VP SGEs
-   VPSSL = VP SSLs

### Voice lists

The voices are kept in a single-linked list. There are 3 voice lists:

-   2D
-   3D
-   MP (Multipass?)

### Voice structure

This is 0x80 bytes

#### Pitch calculation

The 16 bit signed pitch value (*p*) can be converted to and from a
unsigned frequency in Hz (*f*) using the following formulas:

    p = 4096 * log2(f / 48000)
    f = pow2(p / 4096) * 48000

Global Processor (GP)
---------------------

The GP runs all enabled sound effects on the voice bins. DirectSound
allows to load custom DSP code for these effects.

The GP DSP seems to run at 160 MHz

### Memory map

### Related APU memory

-   GPS = GP Scratch (?)
-   GPF = GP FIFO

Encode Processor (EP)
---------------------

The EP encodes the final AC3 stream for SPDIF. It is not used during the
[Boot Animation](/wiki/Boot_Animation "wikilink").

### Memory map

### Related APU memory

-   EPS = EP Scratch (?)
-   EPF = EP FIFO

Related
-------

-   [ACI](/wiki/ACI "wikilink")
-   [DSP](/wiki/DSP "wikilink")
-   [Xbox ADPCM](/wiki/Xbox_ADPCM "wikilink")
-   [Script to inspect APU registers and voice buffers (See master
    revision for
    updates)](https://github.com/JayFoxRox/xbox-tools/blob/5e114f24e4a83bd626f31674e024f439f3709d19/python-scripts/dsp.py)

