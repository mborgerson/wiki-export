---
title: APU
permalink: wiki/APU/
layout: wiki
---

The [MCPX](/wiki/MCPX "wikilink") contains an APU (Audio Processing Unit).

-   SSL = Stream Segment List
-   SGE = Scatter Gather Entry
-   PRD = Physical Resource Descriptor (Same thing as SGE?!)

Frontend Engine (FE)
--------------------

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
-   Pitch (~187.5 Hz to ~12285920.7 Hz)
-   2 Envelopes (DAHDSR: Delay, Attack, Hold, Decay, Sustain, Release)

` * Volume Envelope`  
` * Pitch / Cutoff Envelope`

-   Optionally one of the following filters modes:

` * For 2D Mono:`  
`   * DLS2 Low-Pass`  
`   * Parametric Equalizer`  
`   * DLS2 Low-Pass + Parametric Equalizer`  
` * For 2D Stereo:`  
`   * DLS2 Low-Pass`  
`   * Parametric Equalizer`  
` * For 3D:`  
`   * DLS2 Low-Pass + I3DL2 Reverb`  
`   * Parametric Equalizer + I3DL2 Reverb`  
`   * I3DL2 Reverb`

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

### Envelopes

-   Delay = Time where envelope stays at 0% until attack
-   Attack = Rate at which the envelope goes from 0 to 100%
-   Hold = Time the envelope stays at 100%
-   Decay = Rate at which the envelope goes from 100% to sustain level
-   Sustain = Level at which the envelope stays while the voice is being
    played
-   Release = Rate at which the envelope goes from current level to 0%

#### Volume Envelope

#### Pitch / Cutoff Envelope

### Operation

Voices are stored in VPV. Input data (from the CPU) is loaded using
VPSGE. Voices are then processed and written to the GP MIXBUF.

Related notes
-------------

-   [Something about Parametric
    Equalizers](https://de.mathworks.com/help/audio/examples/parametric-equalizer-design.html)
-   [Mentions I3DL2
    Reverb](https://github.com/kcat/openal-soft/blob/master/Alc/effects/reverb.c)
-   [DLS2 low-pass filter with resonance and dynamic filter cutoff
    frequency](https://www.midi.org/specifications/item/dls-level-2-specification)

Global Processor (GP)
---------------------

The GP is a DSP to do programmable audio processing on the voice bins.

The GP DSP seems to run at 160 MHz.

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

The EP is a DSP to encode the audio signal.

### Memory map

### Related APU memory

-   EPS = EP Scratch (?)
-   EPF = EP FIFO

Usage in DirectSound
--------------------

*This topic deserves it's own article*

The bins are used DirectSound allows to load custom GP DSP code for a
filter / effects chain. The GP waits for the frame interrupt which
signals that MIXBUF data is available. It then goes through the filter
chain. At the end of the chain, the GP DSP will verify that the
execution didn't take longer than the frame duration.

The GP will then issue 6 DMA requests to output the processed frames to
a ringbuffer in scratch space. The frameformat will be the same format
as the GP MIXBUF format (also 0x20 words per channel). Each ringbuffer
is 0x200 words and therefore holds the last 16 frames. Therefore, the
ringbuffer region is 6 \* 0x800 Bytes = 0x3000 Bytes in physical memory.

The order of the channels in the ringbuffer is (also DMA order):

-   0: Front Left
-   1: Center
-   2: Front Right
-   3: Rear Left
-   4: Rear Right
-   5: [Low-frequency
    effects](/wiki/Wikipedia:Low-frequency_effects "wikilink") (LFE)

The EP maps the same data to its own scratch space. It is assumed that
it will DMA this region to its own internal memory. The EP then AC3
encodes the audio data and writes it to the EP FIFO memory. The data is
then send to the ACI AC97 using EP FIFO channels 0 (PCM) and 1 (SPDIF).
The EP code is loaded by DirectSound. The EP is not programmable using
DirectSound.

### Modifications for Boot Animation

During the [Boot Animation](/wiki/Boot_Animation "wikilink") a different
version of DirectSound is used. The EP is disabled in this case. The
data is send to the ACI AC97 using GP FIFO channel 0 (PCM). There is no
AC3 / SPDIF during the boot
animation[1](http://www.gamasutra.com/blogs/BrianSchmidt/20111117/90625/Designing_the_Boot_Sound_for_the_Original_Xbox.php).

Related
-------

-   [ACI](/wiki/ACI "wikilink")
-   [DSP](/wiki/DSP "wikilink")
-   [Xbox ADPCM](/wiki/Xbox_ADPCM "wikilink")
-   [Script to inspect APU registers and voice
    buffers](https://github.com/JayFoxRox/xbox-tools/blob/master/python-scripts/apu.py)

