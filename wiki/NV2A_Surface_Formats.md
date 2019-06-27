---
title: NV2A/Surface Formats
permalink: wiki/NV2A/Surface_Formats/
layout: wiki
tags:
 - NV2A
---

Texture formats
---------------

-   [List of implemented texture formats, according to
    NVIDIA](http://download.nvidia.com/developer/OpenGL_Texture_Formats/nv_ogl_texture_formats.pdf)

### Texture decoding / sampling

The textures are sampled by the texture shader portion of
[NV2A/Pixel\_Combiner](/wiki/NV2A/Pixel_Combiner "wikilink").

#### Texture signedness

Each component of the texture can be either signed (two's-complement) or
unsigned.[1](https://github.com/xqemu/xqemu/issues/75).

#### Texture filtering

The GPU implements the standard texture filters as known from OpenGL. In
addition, it supports convolution
filters[2](https://github.com/xqemu/xqemu/issues/238).

Framebuffer formats
-------------------

Surfaces are rendertargets of the GPU, they can be swizzled or linear.
Additionally, they can be optimized using tiling.

### Color

Z and O stand for Zero and One respectively. These fields will always be
cleared (Zero) or all bis will be set (One).

-   NV097\_SET\_SURFACE\_FORMAT\_COLOR\_LE\_X1R5G5B5\_Z1R5G5B5
-   NV097\_SET\_SURFACE\_FORMAT\_COLOR\_LE\_X1R5G5B5\_O1R5G5B5
-   NV097\_SET\_SURFACE\_FORMAT\_COLOR\_LE\_R5G6B5
-   NV097\_SET\_SURFACE\_FORMAT\_COLOR\_LE\_X8R8G8B8\_Z8R8G8B8
-   NV097\_SET\_SURFACE\_FORMAT\_COLOR\_LE\_X8R8G8B8\_O8R8G8B8
-   NV097\_SET\_SURFACE\_FORMAT\_COLOR\_LE\_X1A7R8G8B8\_Z1A7R8G8B8
-   NV097\_SET\_SURFACE\_FORMAT\_COLOR\_LE\_X1A7R8G8B8\_O1A7R8G8B8
-   NV097\_SET\_SURFACE\_FORMAT\_COLOR\_LE\_A8R8G8B8
-   NV097\_SET\_SURFACE\_FORMAT\_COLOR\_LE\_B8 (not suitable for
    displaying)
-   NV097\_SET\_SURFACE\_FORMAT\_COLOR\_LE\_G8B8 (not suitable for
    displaying)

### Depth

The depth buffer can be configured to be fixed point or floating point.

Additionally, the GPU allows hardware Z-Buffer compression.

-   NV097\_SET\_SURFACE\_FORMAT\_ZETA\_Z16
-   NV097\_SET\_SURFACE\_FORMAT\_ZETA\_Z24S8

