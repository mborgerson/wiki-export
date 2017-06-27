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

### Operation

Voices are stored in VPV. Input data (from the CPU) is loaded using
VPSGE. Voices are then processed and written to the GP MIXBUF.

Global Processor (GP)
---------------------

The GP runs all enabled sound effects on the voice bins.

The GP DSP seems to run at 160 MHz

### MIXBUF

The MIXBUF is a 0x400 word (24-Bit, stored as 32-Bit) section. It is
split into 32 \* 0x20 words. Each 0x20 word block represents one of the
32 voice bins of the VP. The 0x20 words are 24-Bit PCM mono samples to
be played back at 48kHz. The duration of each frame is hence 0.ms.

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

Usage in DirectSound
--------------------

*This topic deserves it's own article*

The bins are used DirectSound allows to load custom GP DSP code for
filter / effects. The GP waits for the frame interrupt which signals
that MIXBUF data is available. It then goes through a filter chain. At
the end of the chain, the GP DSP will verify that the execution didn't
take longer than the frame duration.

The GP will then issue 6 DMA requests to output the processed frames to
a ringbuffer in scratch space. The frameformat will be the same format
as the GP MIXBUF format (also 0x20 words per channel). Each ringbuffer
is 0x200 words and therefore holds the last 16 frames. Therefore, the
ringbuffer region is 6 \* 0x800 Bytes = 0x3000 Bytes in physical memory.

The order of the channels in the ringbuffer is (also DMA order):

-   Front Left
-   Center
-   Front Right
-   Rear Left
-   Rear Right
-   [Low-frequency effects](/wiki/Wikipedia:Low-frequency_effects "wikilink")
    (LFE)

The EP maps the same data to its own scratch space. It is assumed that
it will DMA this region to its own internal memory. The EP then AC3
encodes the audio data and writes it to the EP FIFO memory. The data is
then send to the ACI AC97 using EP FIFO channels 0 (PCM) and 1 (SPDIF).

### Modifications for Boot Animation

During the [Boot Animation](/wiki/Boot_Animation "wikilink") a different
version of DirectSound is used. The EP is disabled in this case. The
data is then send to the ACI AC97 using GP FIFO channel 0 (PCM). There
is no AC3 / SPDIF during the boot animation.

Related
-------

-   [ACI](/wiki/ACI "wikilink")
-   [DSP](/wiki/DSP "wikilink")
-   [Xbox ADPCM](/wiki/Xbox_ADPCM "wikilink")
-   [Script to inspect APU registers and voice buffers (See master
    revision for
    updates)](https://github.com/JayFoxRox/xbox-tools/blob/5e114f24e4a83bd626f31674e024f439f3709d19/python-scripts/dsp.py)

