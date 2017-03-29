---
title: Azurik: Rise of Perathia
permalink: wiki/Azurik:_Rise_of_Perathia/
layout: wiki
---

### Known tricky behaviour

#### Skinning code / Shader rounding mode

This game suffers from issues with non-exact rounding in the shaders
which do the skinning / skeletal animation.

![](http://i.imgur.com/tpmLpjP.png "http://i.imgur.com/tpmLpjP.png")

IIRC Code is like:

`A0 = c[113].x + c[113].z = 18`  
`*do stuff with c[96+A0] to c[98+A0]*`  
  
`A0 = c[113].x + c[113].z = 21`  
`*do stuff with c[96+A0] to c[98+A0]* `  
  
`A0 = c[113].y * v2`  
`*do stuff with c[96+A0] to c[98+A0]*`  
  
`c[113] is vec4(15, 765, 3, 0). 765 = 3*255.`  
`v2 is GL_UNSIGNED_BYTE, normalized => v2 = raw/255.`  
`A0 = 765 * raw/255 = 3*raw.`

This should all be working, but it's not. NV2A expects round-to-zero,
GLSL and other modern graphic APIs have it undefined.. It works with A0
+= 1.0/255.0 as a temporary hack
