---
title: NV2A/Vertex attributes
permalink: wiki/NV2A/Vertex_attributes/
layout: wiki
---

This article documents the attribute types which are supported by the
Xbox GPU. Please take everything with a grain of salt - it's still being
researched.

Draw methods
------------

### Draw Arrays

Data uploaded through DMA. Each attribute has it's own offset and
stride. Decoded using the attribute information.

### Inline Buffer

Data uploaded and decoded through PGRAPH methods.

#### NV097\_SET\_VERTEX\_DATA2F\_M ... NV097\_SET\_VERTEX\_DATA2F\_M + 0x7c

#### NV097\_SET\_VERTEX\_DATA4F\_M ... NV097\_SET\_VERTEX\_DATA4F\_M + 0xfc

#### NV097\_SET\_VERTEX\_DATA2S ... NV097\_SET\_VERTEX\_DATA2S + 0x3c

#### NV097\_SET\_VERTEX\_DATA4UB ... NV097\_SET\_VERTEX\_DATA4UB + 0x3c

#### NV097\_SET\_VERTEX\_DATA4S\_M ... NV097\_SET\_VERTEX\_DATA4S\_M + 0x7c

### Inline Array

Data uploaded through PGRAPH method NV097\_INLINE\_ARRAY. The attributes
are tightly packed in that buffer. Decoded using the attribute
information.

### Inline Elements

Same as “Draw Arrays” but with an extra index buffer. The index buffer
is uploaded through PGRAPH methods NV097\_ARRAY\_ELEMENT16 and
NV097\_ARRAY\_ELEMENT32.

Attribute Types
---------------

### Normalized unsigned byte (D3D)

Unsigned bytes aranged as ZYXW (BGRA) in memory (Also see [this GL
extension](http://www.opengl.org/registry/specs/ARB/vertex_array_bgra.txt)).
This is commonly used with an attribute count of 4. FIXME: Does this
still work with attribute count =/= 4 ? Each byte will be mapped into
the range 0.0 to 1.0. FIXME: Verify

### Normalized unsigned byte (GL)

Unsigned bytes arranged as XYZW (RGBA) in memory. Each byte will be
mapped into the range 0.0 to 1.0. FIXME: Verify

### Normalized short

Shorts, arranged as XYZW. Each short will be mapped into the range -1.0
to 1.0 FIXME: Verify

### Float

IEEE-754 single precision float arranged as XYZW. FIXME: What happens to
NaN or denormals?

### Short

Shorts, arranged as XYZW. Each short will be mapped into the range
-32768.0 to 32767.0 FIXME: Verify

### Compressed normal

Each 32 bit attribute is consisting of 11 bit X, 11 bit Y, 10 bit Z.
Each component is signed and will be mapped into the range -1.0 to 1.0.
FIXME: Verify FIXME: Is the attribute count ignored?
