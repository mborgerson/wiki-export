---
title: NV2A/Pixel Combiner
permalink: wiki/NV2A/Pixel_Combiner/
layout: wiki
tags:
 - NV2A
---

The NV2A implements at least parts of the following OpenGL extensions:

-   [NV\_texture\_shader](https://www.khronos.org/registry/OpenGL/extensions/NV/NV_texture_shader.txt)
-   [NV\_texture\_shader2](https://www.khronos.org/registry/OpenGL/extensions/NV/NV_texture_shader2.txt)
-   [NV\_texture\_shader3](https://www.khronos.org/registry/OpenGL/extensions/NV/NV_texture_shader3.txt)
-   [NV\_register\_combiners](https://www.opengl.org/registry/specs/NV/register_combiners.txt)
-   [NV\_register\_combiners2](https://www.opengl.org/registry/specs/NV/register_combiners2.txt)

Data types
----------

NV\_texture\_shader suggests that: *“The 8-bit and 16-bit signed
fixed-point types are used for signed internal texture formats, while
the 9-bit signed fixed-point type is used for register combiners
computations.”* Here is a table from the GL extension:

| floating-point | 8-bit fixed-point | 9-bit fixed-point | 16-bit fixed-point |
|----------------|-------------------|-------------------|--------------------|
| 1.0            | n/a               | 255               | n/a                |
| 0.99996...     | n/a               | n/a               | 32767              |
| 0.99218...     | 127               | n/a               | n/a                |
| 0.0            | 0                 | 0                 | 0                  |
| -1.0           | -128              | -255              | -32768             |
| -1.00392...    | n/a               | -256              | n/a                |

This means:

-   8-bit fixed-point: \[-128, 127\] → \[-128/128, 127/128\] → \[-1.0,
    0.99218...\]
-   9-bit fixed-point: \[-256, 255\] → \[-256/255, 255/255\] →
    \[-1.00392..., 1.0\]
-   16-bit fixed-point: \[-32768, 32767\] → \[-32768/32768,
    32767/32768\] → \[-1.0, 0.99996...\]

It is not known if the NV2A really implements these 3 datatypes. It is
also not yet known how exactly conversion or negation of these types
would work.

Texturing modes
---------------

<table>
<thead>
<tr class="header">
<th><p>ID</p></th>
<th><p>Name</p></th>
<th><p>D3D name</p></th>
<th><p>GL Name</p></th>
<th><p>Stage 1</p></th>
<th><p>Stage 2</p></th>
<th><p>Stage 3</p></th>
<th><p>Stage 4</p></th>
<th><p>Notes</p></th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><p>0x00</p></td>
<td><p>PS_TEXTUREMODES_NONE</p></td>
<td></td>
<td><p>NONE</p></td>
<td></td>
<td></td>
<td></td>
<td></td>
<td></td>
</tr>
<tr class="even">
<td><p>0x01</p></td>
<td><p>PS_TEXTUREMODES_PROJECT2D</p></td>
<td><p>tex</p></td>
<td><p>TEXTURE_2D</p></td>
<td></td>
<td></td>
<td></td>
<td></td>
<td></td>
</tr>
<tr class="odd">
<td><p>0x02</p></td>
<td><p>PS_TEXTUREMODES_PROJECT3D</p></td>
<td><p>tex</p></td>
<td><p>TEXTURE_3D</p></td>
<td></td>
<td></td>
<td></td>
<td></td>
<td></td>
</tr>
<tr class="even">
<td><p>0x03</p></td>
<td><p>PS_TEXTUREMODES_CUBEMAP</p></td>
<td><p>tex</p></td>
<td><p>TEXTURE_CUBE_MAP_ARB</p></td>
<td></td>
<td></td>
<td></td>
<td></td>
<td></td>
</tr>
<tr class="odd">
<td><p>0x04</p></td>
<td><p>PS_TEXTUREMODES_PASSTHRU</p></td>
<td><p>texcoord</p></td>
<td><p>PASS_THROUGH_NV</p></td>
<td></td>
<td></td>
<td></td>
<td></td>
<td></td>
</tr>
<tr class="even">
<td><p>0x05</p></td>
<td><p>PS_TEXTUREMODES_CLIPPLANE</p></td>
<td><p>texkill</p></td>
<td><p>CULL_FRAGMENT_NV</p></td>
<td></td>
<td></td>
<td></td>
<td></td>
<td></td>
</tr>
<tr class="odd">
<td><p>0x06</p></td>
<td><p>PS_TEXTUREMODES_BUMPENVMAP</p></td>
<td><p>texbem</p></td>
<td><p>OFFSET_TEXTURE_2D_NV</p></td>
<td></td>
<td></td>
<td></td>
<td></td>
<td></td>
</tr>
<tr class="even">
<td><p>0x07</p></td>
<td><p>PS_TEXTUREMODES_BUMPENVMAP_LUM</p></td>
<td><p>texbeml</p></td>
<td><p>OFFSET_TEXTURE_2D_SCALE_NV</p></td>
<td></td>
<td></td>
<td></td>
<td></td>
<td></td>
</tr>
<tr class="odd">
<td><p>0x08</p></td>
<td><p>PS_TEXTUREMODES_BRDF</p></td>
<td><p>texbrdf</p></td>
<td></td>
<td></td>
<td></td>
<td></td>
<td></td>
<td></td>
</tr>
<tr class="even">
<td><p>0x09</p></td>
<td><p>PS_TEXTUREMODES_DOT_ST</p></td>
<td><p>texm3x2tex</p></td>
<td><p>DOT_PRODUCT_TEXTURE_2D_NV</p></td>
<td></td>
<td></td>
<td></td>
<td></td>
<td></td>
</tr>
<tr class="odd">
<td><p>0x0A</p></td>
<td><p>PS_TEXTUREMODES_DOT_ZW</p></td>
<td><p>texm3x2depth</p></td>
<td></td>
<td></td>
<td></td>
<td></td>
<td></td>
<td></td>
</tr>
<tr class="even">
<td><p>0x0B</p></td>
<td><p>PS_TEXTUREMODES_DOT_RFLCT_DIFF</p></td>
<td><p>texm3x3diff</p></td>
<td><p>DOT_PRODUCT_DIFFUSE_CUBE_MAP_NV</p></td>
<td></td>
<td></td>
<td></td>
<td></td>
<td></td>
</tr>
<tr class="odd">
<td><p>0x0C</p></td>
<td><p>PS_TEXTUREMODES_DOT_RFLCT_SPEC</p></td>
<td><p>texm3x3vspec</p></td>
<td><p>DOT_PRODUCT_CONST_EYE_REFLECT_CUBE_MAP_NV</p></td>
<td></td>
<td></td>
<td></td>
<td></td>
<td></td>
</tr>
<tr class="even">
<td><p>0x0D</p></td>
<td><p>PS_TEXTUREMODES_DOT_STR_3D</p></td>
<td><p>texm3x3tex</p></td>
<td><p>DOT_PRODUCT_TEXTURE_3D_NV</p></td>
<td></td>
<td></td>
<td></td>
<td></td>
<td></td>
</tr>
<tr class="odd">
<td><p>0x0E</p></td>
<td><p>PS_TEXTUREMODES_DOT_STR_CUBE</p></td>
<td><p>texm3x3vspec</p></td>
<td><p>DOT_PRODUCT_REFLECT_CUBE_MAP_NV</p></td>
<td></td>
<td></td>
<td></td>
<td></td>
<td></td>
</tr>
<tr class="even">
<td><p>0x0F</p></td>
<td><p>PS_TEXTUREMODES_DPNDNT_AR</p></td>
<td><p>texreg2ar</p></td>
<td><p>DEPENDENT_AR_TEXTURE_2D_NV</p></td>
<td></td>
<td></td>
<td></td>
<td></td>
<td></td>
</tr>
<tr class="odd">
<td><p>0x10</p></td>
<td><p>PS_TEXTUREMODES_DPNDNT_GB</p></td>
<td><p>texreg2gb</p></td>
<td><p>DEPENDENT_GB_TEXTURE_2D_NV</p></td>
<td></td>
<td></td>
<td></td>
<td></td>
<td></td>
</tr>
<tr class="even">
<td><p>0x11</p></td>
<td><p>PS_TEXTUREMODES_DOTPRODUCT</p></td>
<td><p>texm3x3pad<br />
texm3x2pad</p></td>
<td><p>DOT_PRODUCT_NV</p></td>
<td></td>
<td></td>
<td></td>
<td></td>
<td></td>
</tr>
<tr class="odd">
<td><p>0x12</p></td>
<td><p>PS_TEXTUREMODES_DOT_RFLCT_SPEC_CONST</p></td>
<td><p>texm3x3spec</p></td>
<td><p>DOT_PRODUCT_CONST_EYE_REFLECT_CUBE_MAP_NV</p></td>
<td></td>
<td></td>
<td></td>
<td></td>
<td></td>
</tr>
</tbody>
</table>

Debugging
---------

PIX from the Microsoft XDK provides great debugging capabilities.

References and links
--------------------

-   [Overview about programmable fragment
    shading](http://developer.download.nvidia.com/assets/gamedev/docs/ProgrammableTextureBlending.pdf)
-   [Code from nvparse (NVIDIA SDK 9.52) in nxdk, which handles shader
    to OpenGL
    conversion](https://github.com/XboxDev/nxdk/blob/77b5de45f0c64e70f2ff68248873448d5edccc71/tools/fp20compiler/ps1.0_program.cpp#L227)
-   <http://developer.download.nvidia.com/assets/gamedev/docs/GDC2K1_DX8_Pixel_Shaders.pdf>

