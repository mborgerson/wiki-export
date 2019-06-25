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
