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

A powerful voice processor. There can be up to 256 voices
[1](http://www.gamasutra.com/blogs/BrianSchmidt/20111117/90625/Designing_the_Boot_Sound_for_the_Original_Xbox.php)[2](https://web.archive.org/web/20010410003338/http://www.nvnews.net/previews/mcpx/mcpx.shtml)
and 64[3](http://www.nvidia.com/object/IO_20010530_6177.html) of those
can be 3D.

Per-voice settings:

-   Input type (8bit, 16bit, 24bit, ADPCM)
-   8 target bins, each with controllable volume for this voice
-   [Head-related transfer
    function](wikipedia:Head-related_transfer_function "wikilink")
    (HRTF)
-   [Low-frequency
    oscillation](wikipedia:Low-frequency_oscillation "wikilink") (LFO)
-   Pitch (~187.5 Hz to ~12285920.7 Hz)
-   Optionally one of the following filters modes:
    -   For 2D Mono:
        -   DLS2 Low-Pass
        -   Parametric Equalizer
        -   DLS2 Low-Pass + Parametric Equalizer
    -   For 2D Stereo:
        -   DLS2 Low-Pass
        -   Parametric Equalizer
    -   For 3D:
        -   DLS2 Low-Pass + I3DL2 Reverb
        -   Parametric Equalizer + I3DL2 Reverb
        -   I3DL2 Reverb
-   2 Envelopes (DAHDSR: Delay, Attack, Hold, Decay, Sustain, Release)
    -   Amplitude Envelope
    -   Pitch / DLS2 Low-Pass Cutoff Envelope
-   Tracking of certain parameters
    -   Volume
    -   LFO parameters
    -   DLS2 Low-Pass parameters?

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

### LFO

### Tracking

Some of the parameters of the VP can be controlled by setting timing
parameters. New values will then be slowly be adapted over time.

### Envelopes

There are seperate segments of the envelopes, 2 registers (CUR and
COUNT) per envelope keeps track of this and control the envelopes level
(LVL):

-   0: Off = Envelope is not used
-   1: Delay = Time where envelope stays at 0% until attack
    -   COUNT register counts down from DELAYTIME.
-   2: Attack = Rate at which the envelope goes from 0 to 100%
    -   COUNT register counts up to ATTACKRATE.
    -   LVL seems to be linear.
-   3: Hold = Time the envelope stays at 100%
    -   COUNT register counts down from HOLDTIME.
-   4: Decay = Rate at which the envelope goes from 100% to 0%
    -   COUNT register counts down from DECAYRATE.
    -   The LVL is not linear, it's a curve which drops steep at first
        and then slowly becomes
        level[4](https://docs.google.com/spreadsheets/d/1fNsfkvBfnlRQ9XplNnTgmh8jBaWJ56bvh2AVWn8B_k0/edit?usp=sharing)
    -   The best approximation I could come up with so far is:
        `255*pow(0.99988799,(DECAYRATE*16-COUNT)*4096/DECAYRATE)`.
    -   When the output level (not LVL!) reaches the sustain level the
        decay section is over (In my example above, this happens when
        the output level is less than 0.2 away from the sustain level;
        so if sustain is 0, the voice would switch to the sustain
        segment when a value below 0.2 is reached)
    -   Writes to the LVL register are ignored
    -   Writes to count (any value?) impact the LVL
    -   If the COUNT is larger than the decayrate the envelope will
        switch to the sustain state
    -   If the COUNT results in a smaller output level than the
        sustainlevel the envelope will switch to the sustain state
-   5: Sustain = Level at which the envelope stays while the voice is
    being played
    -   COUNT register writes are ignored, it will stay at 0
    -   LVL register writes are ignored, it will stay at the current
        sustain level
-   6: Release = Rate at which the envelope goes from current level to
    0%
    -   Can start at any time
    -   COUNT register starts at RELEASERATE, regardless of the current
        sustain level
    -   COUNT register counts down
    -   LVL is not updated during this phase (it will keep it's previous
        value)
    -   Writes to LVL have an impact on the output volume
    -   If the COUNT register is higher than the releaserate, the output
        will be silent and LVL will drop to zero
    -   The actual output level is probably determined like:
        `int(COUNT * LVL / (RELEASERATE * 16))`
    -   COUNT will keep counting until 0 even after the output level has
        hit 0
-   7: Force Release = Unknown still
    -   Seems to happen during invalid conditions? Happened to me when
        modifying ebo during playback
    -   LVL and COUNT seem to be ignored during this, but writes go
        through? Output level seems to stay at 100% ? (I only got
        repeating 32 samples during this and the whole Xbox crashed
        shortly after)

All durations are described using unsigned 12-bit times/rates. The level
of sustain is stored unsigned in 8-bit. The COUNT register is stored in
unsigned 16-bit.

The 12-bit times/rates are multiplied by 16 when loading them into the
16-bit COUNT register. The COUNT register counts at 1500
Hz[5](https://docs.google.com/spreadsheets/d/11jxeJ9aey_TVkyiMmmd6SKuow4j4GR9E9fRZ6HXc2WU/edit#gid=396423867).
A unit in the COUNT register is therefore 0. ms.

The 12-bit values of the envelope sections are given in units of 0. ms
\* 16 = 10. ms. This can also be written as 512 / (48000 Hz) = 10. ms.
The maximum length of an envelope section is therefore 4095 \* 10. ms =
43.68 seconds.

As the envelope counter runs at a fixed clock speed, it is independent
of the voice pitch and duration.

If the Amplitude Envelope COUNT hits 0 during release, DirectSound
already deletes the voice, regardless of the Filter Envelope.

The sustain level can be changed during playback. Also the attack
register can be changed to a lower value while the counter is counting
up, however, if the COUNTER does not compare equal to the set value, it
will keep counting, even after an overflow. It will not leave the attack
phase and keep counting until it sees value COUNTER / 16 in the attack
register. If the attack register is set to a higher value while
counting, the volume is going down again. Also, if the attack register
value is zero while counting, there won't be any audio output during the
attack phase. This indicates that the COUNT register is used to
calculate the actual value from the current rates.

The initial state of each envelope can be controlled by the
NV1BA0\_PIO\_VOICE\_ON command. It can either be: DISABLE, DELAY, ATTACK
or HOLD.

#### Amplitude Envelope

The amplitude envelope is mixed with the volume during mixing. The
volume registers are not modified. It is not yet known how many bits of
the envelope state are used.

#### Filter Envelope

The pitch scale is multiplied with the current envelope state and added
to the current pitch during mixing. The pitch registers are not
modified.

    f = 2^((signed_pitch+signed_pitch_mod*32*envelope_state_float)/4096)*48000 # envelope_state_float: [0, 1]

It is not yet known how many bits of the envelope state are used.

### Filters

#### DLS2

Formulas from DirectSound

    FreqToHardwareCoeff(frequency): # Input in Hz
      if (frequency < 30) { return 0x8000 }
      if (frequency > 8000) { return 0x0000 }
      FC = 2 * sin(PI * frequency / 48000)
      octaves = 4096 * log2(FC)
      return octaves & 0xFFFF
    hardware_coefficient[0] = FreqToHardwareCoeff(frequency)

    dBToHardwareCoeff(resonance): # Input in dB
      resonance = min(resonance, 22.5)
      Q = pow(10, -0.05 * resonance)
      return min(0xFFFF)
    hardware_coefficient[1] = dBToHardwareCoeff(resonance_in_db)

Stuff from the DLS2 spec:

There are 2 coeffiecents per channel:

-   F\_c (Cutoff frequency)
-   resonance

From Page 8 of “DLS 2.2 Version
1.0”[6](https://www.midi.org/specifications/item/dls-level-2-specification)

-   b\_1 = -2 \* r \* cos(θ)
-   b\_2 = r \* r
-   K = g \* (1 + b\_1 + b\_2)

<!-- -->

    y[i] = K * x[i] - b_1 * y[i-1] - b_2 * y[i-2]

Where y\[i-2\] and y\[i-1\] are the last two frames of the output and
x\[i\] the current input.

### Operation

Voices are stored in VPV. Input data (from the CPU) is loaded using
VPSGE. Voices are then processed and written to the GP MIXBUF.

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
DirectSound APIs.

### Modifications for Boot Animation

During the [Boot Animation](/wiki/Boot_Animation "wikilink") a different
version of DirectSound is used. The EP is disabled in this case. The
data is send to the ACI AC97 using GP FIFO channel 0 (PCM). There is no
AC3 / SPDIF during the boot
animation[7](http://www.gamasutra.com/blogs/BrianSchmidt/20111117/90625/Designing_the_Boot_Sound_for_the_Original_Xbox.php).

Related notes
-------------

-   [ACI](/wiki/ACI "wikilink")
-   [DSP](/wiki/DSP "wikilink")
-   [Xbox ADPCM](/wiki/Xbox_ADPCM "wikilink")
-   [Information at NVIDIAs
    website](http://www.nvidia.com/object/apu.html)
-   [“Technical Brief: NVIDIA nForce MCP Audio Processing Unit” by
    NVIDIA](http://www.nvidia.com/attach/9004)
-   [Scripts to inspect APU registers and voice
    buffers](https://github.com/JayFoxRox/xbox-tools/blob/master/python-scripts/)
-   Filter information
    -   [Something about Parametric
        Equalizers](https://de.mathworks.com/help/audio/examples/parametric-equalizer-design.html)
    -   [Mentions I3DL2
        Reverb](https://github.com/kcat/openal-soft/blob/master/Alc/effects/reverb.c)
    -   [DLS2 low-pass filter with resonance and dynamic filter cutoff
        frequency](https://www.midi.org/specifications/item/dls-level-2-specification)

