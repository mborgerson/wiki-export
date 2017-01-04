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

Registers
---------

There are:

-   12 temporary registers: R0 to R11
-   output registers: oPos, oD0, oD1, oFog, oPts, oB0, oB1, oT0, oT1,
    oT2, oT3, A0.x
-   Input data: ???

R12 is a mirror of the output register oPos

Instructions
------------

???
