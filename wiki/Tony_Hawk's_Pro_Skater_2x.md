---
title: Tony Hawk's Pro Skater 2x
permalink: wiki/Tony_Hawk's_Pro_Skater_2x/
layout: wiki
---

Shaders
-------

These shaders have been disassembled. Any comment was added manually and
does not come from the game files.

### blackskin.xvu

    xvs.1.1
    add r2.x, c32.z, -v1.x
    dph r3.x, v0, c0
    dph r3.y, v0, c1
    dph r3.z, v0, c2
    dph r4.x, v0, c3
    dph r4.y, v0, c4
    dph r4.z, v0, c5
    mul r5.xyz, r3.xyz, v1.x
    mad r6.xyz, r4.xyz, r2.x, r5.xyz
    dph oPos.x, r6, c21
    dph oPos.y, r6, c22
    dph oPos.z, r6, c23
    dph oPos.w, r6, c24
    mul oPos.xyz, r12, c-38
    +rcc r1.x, r12.w
    mad oPos.xyz, r12, r1.x, c-37

### cloth.xvu

    xvs.1.1
    mov r2.w, c48.x
    mul r3.w, r2.w, c80.z
    mad r4.xyz, c80.x, v0.xyz, r3.w
    add r5.x, c32.z, -v1.x
    dph r6.x, v0, c0
    +exp r1.y, r4
    dph r6.y, v0, c1
    mul r1.x, r1.y, r1.y
    mul r1.z, r1.x, r1.y
    dp3 r7.x, r1.xyz, c36
    dph r6.z, v0, c2
    +exp r1.y, r4.y
    dph r8.x, v0, c3
    mul r1.x, r1.y, r1.y
    mul r1.z, r1.x, r1.y
    dp3 r7.y, r1.xyz, c36
    dph r8.y, v0, c4
    +exp r1.y, r4.z
    dph r8.z, v0, c5
    mul r1.x, r1.y, r1.y
    mul r1.z, r1.x, r1.y
    dp3 r7.z, r1.xyz, c36
    mul r9.xyz, r6.xyz, v1.x
    mad r10.xyz, r8.xyz, r5.x, r9.xyz
    mul r11.w, v3.y, c80.y
    mul r0.w, r11.w, c92.w
    mad r2.xyz, r7.xyz, r0.w, r10.xyz
    mad r3.xyz, -c92.xyz, v3.z, r2.xyz
    dp3 r4.x, v2, c0
    dp3 r4.y, v2, c1
    dp3 r4.z, v2, c2
    dp3 r6.x, v2, c3
    dp3 r6.y, v2, c4
    dp3 r6.z, v2, c5
    mul r7.xyz, r4.xyz, v1.x
    mad r8.xyz, r6.xyz, r5.x, r7.xyz
    dph oPos.x, r3, c21
    dph oPos.y, r3, c22
    dph oPos.z, r3, c23
    dph oPos.w, r3, c24
    dp3 r8.w, r8.xyz, r8.xyz
    dph oT0.x, v9, c9
    mov r9, c58
    +rsq r1.w, r8.w
    dph oT0.y, v9, c10
    mul r10.xyz, r8.xyz, r1.w
    dp3 r11.x, r10, -c56
    dp3 r11.y, r10, -c57
    dph oFog.x, r3, c24
    max r0.xy, r11.xy, c32.x
    mad r2, r0.x, c59, r9
    mad r3, r0.y, c60, r2
    mul oPos.xyz, r12, c-38
    +rcc r1.x, r12.w
    mul oD0, r3, c90
    mad oPos.xyz, r12, r1.x, c-37

### geom.xvu

    xvs.1.1
    add r0.xyz, v0.xyz, c91.xyz
    mov oD0, v3
    dph oPos.x, r0, c21
    dph oPos.y, r0, c22
    dph oPos.z, r0, c23
    dph oPos.w, r0, c24
    dph oFog.x, r0, c24
    mov oT0.xy, v9
    mul oT1.xy, v9, c91.w
    mul oPos.xyz, r12, c-38
    +rcc r1.x, r12.w
    mad oPos.xyz, r12, r1.x, c-37

### grass.xvu

**Vertex attributes**

-   v0.xyz = Position
-   v3.xyzw = Diffuse color
-   v9.xy = Texture1 coordinates

**Constants**

-   c-38.xyz = viewport? (D3D boilerplate)
-   c-37.xyz = viewport? (D3D boilerplate)
-   c21.xyzw - c24.xyzw = world-view-projection-matrix ?
-   c32.w = Texture1 scale
-   c91.xyz = Object position

**Code**

    xvs.1.1

    # Add object position to vertex position
    add r0.xyz, v0, c91

    # Forward diffuse color
    mov oD0, v3

    # Calculate output position
    dph oPos.x, r0, c21
    dph oPos.y, r0, c22
    dph oPos.z, r0, c23
    dph oPos.w, r0, c24

    # Generate fog depth (same as oPos.w - somehow not optimized?)
    dph oFog.x, r0, c24

    # Scale texture
    mul oT0.xy, v9, c32.w

    # Viewport or something? (D3D boilerplate)
    mul oPos.xyz, r12, c-38
    +rcc r1.x, r12.w
    mad oPos.xyz, r12, r1.x, c-37

### projectorlight.xvu

    xvs.1.1
    dph r2.x, v0, c0
    dph r2.y, v0, c1
    dph r2.z, v0, c2
    dp3 r3.x, v2, c92
    dph oPos.x, r2, c21
    dph oPos.y, r2, c22
    dph oPos.z, r2, c23
    dph oPos.w, r2, c24
    mad oD0.xyz, r3.x, c32.y, c32.y
    dph r4.x, r2, c6
    dph r4.y, r2, c7
    dph r4.z, r2, c8
    mul oPos.xyz, r12, c-38
    +rcc r1.x, r12.w
    dph r5.w, r4, c12
    dph r7.y, r4, c10
    dph r7.x, r4, c9
    +rcp r1.w, r5.w
    dph oT1.x, r4, c13
    max r6.w, r1.w, -r1.w
    mul oT0.xy, r7.xy, r6.w
    mad oPos.xyz, r12, r1.x, c-37

### projectorshadow.xvu

    xvs.1.1
    dph r2.x, v0, c0
    dph r2.y, v0, c1
    dph r2.z, v0, c2
    dp3 r3.x, v2, c92
    dph oPos.x, r2, c21
    dph oPos.y, r2, c22
    dph oPos.z, r2, c23
    dph oPos.w, r2, c24
    mad oD1.xyz, -r3.x, c32.y, c32.y
    dph r4.x, r2, c6
    dph r4.y, r2, c7
    dph r4.z, r2, c8
    mul oPos.xyz, r12, c-38
    +rcc r1.x, r12.w
    dph r5.x, r4, c9
    dph r5.y, r4, c10
    dph oT1.x, r4, c11
    mul oT0.xy, r5.xy, c33
    mad oPos.xyz, r12, r1.x, c-37

### projectorskin.xvu

    xvs.1.1
    add r2.x, c32.z, -v1.x
    dp3 r3.x, v2, c0
    dp3 r3.y, v2, c1
    dp3 r3.z, v2, c2
    dp3 r4.x, v2, c3
    dp3 r4.y, v2, c4
    dp3 r4.z, v2, c5
    mul r5.xyz, r3.xyz, v1.x
    mad r6.xyz, r4.xyz, r2.x, r5.xyz
    dph r7.x, v0, c0
    dph r7.y, v0, c1
    dph r7.z, v0, c2
    dph r8.x, v0, c3
    dph r8.y, v0, c4
    dph r8.z, v0, c5
    mul r9.xyz, r7.xyz, v1.x
    mad r10.xyz, r8.xyz, r2.x, r9.xyz
    dp3 r11.w, r6.xyz, r6.xyz
    dph oPos.y, r10, c22
    dph oPos.x, r10, c21
    +rsq r1.w, r11.w
    dph oPos.z, r10, c23
    mul r0.xyz, r6.xyz, r1.w
    dp3 r2.x, r0.xyz, c92
    dph oPos.w, r10, c24
    mad oD0.xyz, r2.x, c32.y, c32.y
    dph r3.x, r10, c6
    dph r3.y, r10, c7
    dph r3.z, r10, c8
    mul oPos.xyz, r12, c-38
    +rcc r1.x, r12.w
    dph r4.w, r3, c12
    dph r6.y, r3, c10
    dph r6.x, r3, c9
    +rcp r1.w, r4.w
    dph oT1.x, r3, c13
    max r5.w, r1.w, -r1.w
    mul oT0.xy, r6.xy, r5.w
    mad oPos.xyz, r12, r1.x, c-37

### skin.xvu

**Vertex attributes**

-   v0.xyzw = Position
-   v1.x = vertex weight
-   v2.xyz = Normal
-   v9.xyz = Texture1 coordinates

**Constants**

-   c-38.xyz = viewport? (D3D boilerplate)
-   c-37.xyz = viewport? (D3D boilerplate)
-   c0.xyzw - c3.xyzw = matrix bone\[0\] ?
-   c3.xyzw - c5.xyzw = matrix bone\[1\] ?
-   c9.xyzw = Texture U matrix
-   c10.xyzw = Texture V matrix
-   c21.xyzw - c24.xyzw = projection-matrix ?
-   c32.x = CONSTANT() ?
-   c32.z = CONSTANT(1.0)
-   c56.xyz - c57.xyz = some normal matrix
-   c58.xyzw = ?
-   c59.xyzw = ?
-   c60.xyzw = ?
-   c90.xyzw = light diffuse color ?

**Code**

    xvs.1.1

    # Get (1.0 - vertex weight)
    add r2.x, c32.z, -v1.x

    # Transform bone[0] normal ?
    dp3 r3.x, v2, c0
    dp3 r3.y, v2, c1
    dp3 r3.z, v2, c2

    # Transform bone[1] normal ?
    dp3 r4.x, v2, c3
    dp3 r4.y, v2, c4
    dp3 r4.z, v2, c5

    # Blend normals: r6.xyz = mix(r4.xyz, r3.xyz, v1.x)
    mul r5.xyz, r3.xyz, v1.x
    mad r6.xyz, r4.xyz, r2.x, r5.xyz

    # Transform bone[0] vertex
    dph r7.x, v0, c0
    dph r7.y, v0, c1
    dph r7.z, v0, c2

    # Transform bone[1] vertex
    dph r8.x, v0, c3
    dph r8.y, v0, c4
    dph r8.z, v0, c5

    # Blend vertices: r10.xyz = mix(r8.xyz, r7.xyz, v1.x)
    mul r9.xyz, r7.xyz, v1.x
    mad r10.xyz, r8.xyz, r2.x, r9.xyz

    # Calculation squared normal length
    dp3 r11.w, r6.xyz, r6.xyz

    # Calculate output position (xy)
    dph oPos.y, r10, c22
    dph oPos.x, r10, c21

    # Turn squared normal length into length: r1.w = 1.0 / length(normal)
    +rsq r1.w, r11.w

    # Calculate output position (z)
    dph oPos.z, r10, c23

    # Multiply normal by it's inverse length: r0.xyz = normalize(normal)
    mul r0.xyz, r6.xyz, r1.w

    # Calculate output position (w)
    dph oPos.w, r10, c24

    # Generate fog depth (same as oPos.w - somehow not optimized?)
    dph oFog.x, r10, c24

    # Dead code?
    mov r2, c58

    # Something with normal.xy
    dp3 r3.x, r0, -c56
    dp3 r3.y, r0, -c57

    # Forward texture U
    dph oT0.x, v9, c9

    # Something to do with lighting
    max r4.xy, r3.xy, c32.x
    mad r5, r4.x, c59, r2
    mad r6, r4.y, c60, r5

    # Forward texture V
    dph oT0.y, v9, c10

    # Calculate diffuse color
    mul oD0, r6, c90

    # Viewport or something? (D3D boilerplate)
    mul oPos.xyz, r12, c-38
    +rcc r1.x, r12.w
    mad oPos.xyz, r12, r1.x, c-37

### wiggle.xvu

    xvs.1.1
    dph r2.x, v0, c0
    dph r2.y, v0, c1
    dph r2.z, v0, c2
    mov r3.xy, c48.x
    +mov oT0.xy, v9
    dp3 r4.x, r2, c72
    dp3 r4.y, r2, c73
    mad r5.xy, r3.xy, c74.xy, r4.xy
    dp3 r6.x, v2, c0
    mov r7.z, c32.x
    +exp r1.y, r5
    exp r8.y, r5.y
    mul r1.x, r1.y, r1.y
    mul r8.x, r8.y, r8.y
    +mov oD0, c32.z
    mul r1.z, r1.x, r1.y
    mul r8.z, r8.x, r8.y
    dp3 r7.x, r1.xyz, c36
    dp3 r7.y, r8.xyz, c36
    dp3 r6.y, v2, c1
    dp3 r6.z, v2, c2
    dp3 r9.x, r7.xyz, c74.zw
    mul r10.xyz, r6, v3.x
    mad r11.xyz, r10, r9.x, r2
    dph oPos.x, r11, c21
    dph oPos.y, r11, c22
    dph oPos.z, r11, c23
    dph oPos.w, r11, c24
    dph oFog.x, r11, c24
    mul oPos.xyz, r12, c-38
    +rcc r1.x, r12.w
    mad oPos.xyz, r12, r1.x, c-37

Demo
----

There was a demo released for ??? on ???.

### Rendering in the Demo

This is a rundown of how the scene is being rendered in the THPS2X Demo
version Rendering happens in this order:

1.  World
2.  Skater
    -   Shader: cloth.xvu
3.  HUD
4.  Shadows
    1.  Skater
        -   Skater shadows are rendered into a 256x256 texture after the
            entire scene has been rendered
        -   Shader: blackskin.xvu

