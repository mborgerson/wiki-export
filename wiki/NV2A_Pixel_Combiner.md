---
title: NV2A/Pixel Combiner
permalink: wiki/NV2A/Pixel_Combiner/
layout: wiki
---

The NV2A implements
[NV\_register\_combiners](https://www.opengl.org/registry/specs/NV/register_combiners.txt)
(and
[NV\_register\_combiners2](https://www.opengl.org/registry/specs/NV/register_combiners2.txt)?)

Texturing modes
---------------

| ID   | Name                                      | Stage 1 | Stage 2 | Stage 3 | Stage 4 | Notes |
|------|-------------------------------------------|---------|---------|---------|---------|-------|
| 0x00 | PS\_TEXTUREMODES\_NONE                    | -       | -       | -       | -       |       |
| 0x01 | PS\_TEXTUREMODES\_PROJECT2D               | -       | -       | -       | -       |       |
| 0x02 | PS\_TEXTUREMODES\_PROJECT3D               | -       | -       | -       | -       |       |
| 0x03 | PS\_TEXTUREMODES\_CUBEMAP                 | -       | -       | -       | -       |       |
| 0x04 | PS\_TEXTUREMODES\_PASSTHRU                | -       | -       | -       | -       |       |
| 0x05 | PS\_TEXTUREMODES\_CLIPPLANE               | -       | -       | -       | -       |       |
| 0x06 | PS\_TEXTUREMODES\_BUMPENVMAP              |         | -       | -       | -       |       |
| 0x07 | PS\_TEXTUREMODES\_BUMPENVMAP\_LUM         |         | -       | -       | -       |       |
| 0x08 | PS\_TEXTUREMODES\_BRDF                    |         |         | -       | -       |       |
| 0x09 | PS\_TEXTUREMODES\_DOT\_ST                 |         |         | -       | -       |       |
| 0x0A | PS\_TEXTUREMODES\_DOT\_ZW                 |         |         | -       | -       |       |
| 0x0B | PS\_TEXTUREMODES\_DOT\_RFLCT\_DIFF        |         |         | -       |         |       |
| 0x0C | PS\_TEXTUREMODES\_DOT\_RFLCT\_SPEC        |         |         |         | -       |       |
| 0x0D | PS\_TEXTUREMODES\_DOT\_STR\_3D            |         |         |         | -       |       |
| 0x0E | PS\_TEXTUREMODES\_DOT\_STR\_CUBE          |         |         |         | -       |       |
| 0x0F | PS\_TEXTUREMODES\_DPNDNT\_AR              |         | -       | -       | -       |       |
| 0x10 | PS\_TEXTUREMODES\_DPNDNT\_GB              |         | -       | -       | -       |       |
| 0x11 | PS\_TEXTUREMODES\_DOTPRODUCT              |         | -       | -       |         |       |
| 0x12 | PS\_TEXTUREMODES\_DOT\_RFLCT\_SPEC\_CONST |         |         |         | -       |       |

Debugging
---------

PIX from the Microsoft XDK provides great debugging capabilities.

References and links
--------------------

-   <http://developer.download.nvidia.com/assets/gamedev/docs/GDC2K1_DX8_Pixel_Shaders.pdf>
-   <http://developer.download.nvidia.com/assets/gamedev/docs/ProgrammableTextureBlending.pdf>

