---
title: XBE
permalink: wiki/XBE/
layout: wiki
---

### .text

The .text section contains all x86 subroutines to be executed by the
[processor](/wiki/CPU "wikilink").

### .rdata

The .rdata section contains the [kernel thunk table](/wiki/Kernel "wikilink").
The ordinals in the table are to be resolved to the kernel's actual
calling routine, when loaded.
