---
title: ACI
permalink: wiki/ACI/
layout: wiki
---

The ACI of the Xbox provides AC97. It's similar to the AMD-768 ACI, but
it is accessed through MMIO instead. The datasheet for the AMD-768 can
be found at
<https://web.archive.org/web/20060322081039/http://www.amd.com/us-en/assets/content_type/white_papers_and_tech_docs/24467.pdf>
.

The AC97 implementation only supports playback of 16-bit PCM stereo
samples at 48kHz.

Register addresses
------------------

-   0x110 PCM
-   0x170 SPDIF

Usage in DirectSound
--------------------

AC97 is only used to receive buffers from the [APU](/wiki/APU "wikilink").

Related links
-------------

-   [https://github.com/JayFoxRox/xbox-tools/blob/master/python-scripts/aci.py
    Script to inspect ACI registers and AC97 input
    buffers](https://github.com/JayFoxRox/xbox-tools/blob/master/python-scripts/aci.py_Script_to_inspect_ACI_registers_and_AC97_input_buffers "wikilink")

