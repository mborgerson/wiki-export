---
title: NV2A/Vertex Shader
permalink: wiki/NV2A/Vertex_Shader/
layout: wiki
---

The Xbox implements
<https://www.opengl.org/registry/specs/NV/vertex_program.txt> and
<https://www.opengl.org/registry/specs/NV/vertex_program1_1.txt> This
article will mainly focus on actual encoding on hardware as the
behaviour is mostly outlined in those GL extensions already.

Operating modes
---------------

-   Fixed / Programmable
-   Writeable / Read-Only constants
-   Low-constants only / all constants
-   Vertex processing / State program

Registers
---------

-   16 input registers v\[0\] to v\[15\] (from vertex, v\[0\] is fed
    from LAUNCH\_DATA for VSPs)
-   output registers: o\[0\] to o\[?\] (initialized to XYZ=0x00000000
    W=0x3F800000)
    -   Following indices are aliased: Pos, oD0, oD1, oFog, oPts, oB0,
        oB1, oT0, oT1, oT2, oT3
-   1 Address register: A0.x
-   12 temporary registers: R0 to R11 (initialized to XYZW=0x00000000)
    -   The POS register is mirrored as R12 so it can be used as source
        operand, so effectively you have 13 temporaries

### Constant space

There are 192 constant registers in two seperate blocks with 96
constants each. They can be accessed through the PGRAPH RDI:
select=0x17. Each constant slot is 4x DWORD, ordered as WZYX.
Alternatively they can be uploaded through method \[FIXME\], with 4x
DWORD, ordered XYZW.

Instructions
------------

In total, there are 136 instruction slots.

Each slot consists of 16 bytes, describing the operation:

|               |             |         |
|---------------|-------------|---------|
| Offset (bits) | Size (bits) | Meaning |
| 0             | 0           | TODO    |
| 0             | 0           | TODO    |


