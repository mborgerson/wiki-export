---
title: Halo: Combat Evolved
permalink: wiki/Halo:_Combat_Evolved/
layout: wiki
---

### Debugging

To enable debugging of the original NTSC version with xbdm (be sure to
use the retail xbdm version 5455 if on a retail box due to low memory
constraints) replace the single instance of `6A 00 FF 74 24 08 E8 35`
with `33 C0 40 C2 04 00 E8 35` in the default.xbe which will skip a call
to `XnTerm`.
