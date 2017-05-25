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

<table>
<thead>
<tr class="header">
<th><p>ID</p></th>
<th><p>Name</p></th>
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
<td><p>PS_TEXTUREMODES_NONE<br />
texcoord?</p></td>
<td><p>NONE</p></td>
<td></td>
<td></td>
<td></td>
<td></td>
<td></td>
</tr>
<tr class="even">
<td><p>0x01</p></td>
<td><p>PS_TEXTUREMODES_PROJECT2D<br />
tex</p></td>
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
<td><p>TEXTURE_CUBE_MAP_ARB</p></td>
<td></td>
<td></td>
<td></td>
<td></td>
<td></td>
</tr>
<tr class="odd">
<td><p>0x04</p></td>
<td><p>PS_TEXTUREMODES_PASSTHRU<br />
texcoord?</p></td>
<td><p>PASS_THROUGH_NV</p></td>
<td></td>
<td></td>
<td></td>
<td></td>
<td></td>
</tr>
<tr class="even">
<td><p>0x05</p></td>
<td><p>PS_TEXTUREMODES_CLIPPLANE<br />
texkill</p></td>
<td><p>CULL_FRAGMENT_NV</p></td>
<td></td>
<td></td>
<td></td>
<td></td>
<td></td>
</tr>
<tr class="odd">
<td><p>0x06</p></td>
<td><p>PS_TEXTUREMODES_BUMPENVMAP<br />
texbem</p></td>
<td><p>OFFSET_TEXTURE_2D_NV</p></td>
<td></td>
<td></td>
<td></td>
<td></td>
<td></td>
</tr>
<tr class="even">
<td><p>0x07</p></td>
<td><p>PS_TEXTUREMODES_BUMPENVMAP_LUM<br />
texbeml</p></td>
<td><p>OFFSET_TEXTURE_2D_SCALE_NV</p></td>
<td></td>
<td></td>
<td></td>
<td></td>
<td></td>
</tr>
<tr class="odd">
<td><p>0x08</p></td>
<td><p>PS_TEXTUREMODES_BRDF<br />
texm3x2tex</p></td>
<td></td>
<td></td>
<td></td>
<td></td>
<td></td>
<td></td>
</tr>
<tr class="even">
<td><p>0x09</p></td>
<td><p>PS_TEXTUREMODES_DOT_ST<br />
texm3x2pad?</p></td>
<td><p>DOT_PRODUCT_NV</p></td>
<td></td>
<td></td>
<td></td>
<td></td>
<td></td>
</tr>
<tr class="odd">
<td><p>0x0A</p></td>
<td><p>PS_TEXTUREMODES_DOT_ZW<br />
texm3x2tex?</p></td>
<td><p>DOT_PRODUCT_NV</p></td>
<td></td>
<td></td>
<td></td>
<td></td>
<td></td>
</tr>
<tr class="even">
<td><p>0x0B</p></td>
<td><p>PS_TEXTUREMODES_DOT_RFLCT_DIFF</p></td>
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
<td><p>DOT_PRODUCT_REFLECT_CUBE_MAP_NV</p></td>
<td></td>
<td></td>
<td></td>
<td></td>
<td></td>
</tr>
<tr class="even">
<td><p>0x0F</p></td>
<td><p>PS_TEXTUREMODES_DPNDNT_AR<br />
texreg2ar</p></td>
<td><p>DEPENDENT_AR_TEXTURE_2D_NV</p></td>
<td></td>
<td></td>
<td></td>
<td></td>
<td></td>
</tr>
<tr class="odd">
<td><p>0x10</p></td>
<td><p>PS_TEXTUREMODES_DPNDNT_GB<br />
texreg2gb</p></td>
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
<td><p>DOT_PRODUCT_CONST_EYE_REFLECT_CUBE_MAP_NV</p></td>
<td></td>
<td></td>
<td></td>
<td></td>
<td></td>
</tr>
</tbody>
</table>

Also known from nvidia docs:

-   texm3x3pad \[stage 1, stage 2\]
-   texm3x3spec \[stage 3\]
-   texm3x3vspec \[stage 3\]
-   texm3x3tex \[stage 3\]

Debugging
---------

PIX from the Microsoft XDK provides great debugging capabilities.

References and links
--------------------

-   <http://developer.download.nvidia.com/assets/gamedev/docs/GDC2K1_DX8_Pixel_Shaders.pdf>
-   <http://developer.download.nvidia.com/assets/gamedev/docs/ProgrammableTextureBlending.pdf>

