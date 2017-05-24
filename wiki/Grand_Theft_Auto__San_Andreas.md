---
title: Grand Theft Auto: San Andreas
permalink: wiki/Grand_Theft_Auto:_San_Andreas/
layout: wiki
---

See <https://github.com/aap/skygfx> for a re-implementation of the Xbox
graphics features for PC.

This game shares some technical aspects with [Grand Theft Auto
III](/wiki/Grand_Theft_Auto_III "wikilink") and [Grand Theft Auto: Vice
City](/wiki/Grand_Theft_Auto:_Vice_City "wikilink").

At least some parts of the games source code have been leaked into the
public. The source code was illegally offered for sale in the past.

Reverse engineering by aap
--------------------------

aap has reverse engineered a lot for skygfx. Some of these files might
be for [Grand Theft Auto III](/wiki/Grand_Theft_Auto_III "wikilink") or [Grand
Theft Auto: Vice City](/wiki/Grand_Theft_Auto:_Vice_City "wikilink"). They
come from a folder for all GTA research

### Shader used for cars

#### Shader 1

    ;   c14     0.0 0.5 1.0 2.0
    ;   c26     specular
    ;   c27     spec, fresnel, 0.6, power

     dp4    r3.x, v0, c4
     dp4    r3.y, v0, c5
     dp4    r3.z, v0, c6
     add    r2.xyz, c12.xyzz, -r3.xyzz
     dp3    r0.x, v1.xyzz, c4.xyzz
     dp3    r2.w, r2.xyzz, r2.xyzz
     +mov   oD0.w, c14.z
     dp3    r0.y, v1.xyzz, c5.xyzz
     dp3    r0.z, v1.xyzz, c6.xyzz
     +rsq   r1.w, r2.w
     mov    r5.w, c14.w
     mul    r2.xyz, r2.xyzz, r1.w       ; r2: normalized view vector
     dp3    r0.w, r0.xyzz, r0.xyzz
     add    r4.xyz, r2.xyzz, -c13.xyzz
     mov    r11.w, c27.w
     +rsq   r1.w, r0.w
     dp3    r4.w, r4.xyzz, r4.xyzz
     mul    r0.xyz, r0.xyzz, r1.w       ; r0: normalized world normal
     add    r10.xyz, r2.xyzz, -c18.xyzz
     mul    r11.z, c27.w, r5.w
     +rsq   r1.w, r4.w
     dp3    r4.w, r10.xyzz, r10.xyzz
     add    r9.yzw, r2.yyzx, -c19.yyzx
     mul    r4.xyz, r4.xyzz, r1.w
     dp3    r4.w, r9.wyzz, r9.wyzz
     +rsq   r1.w, r4.w
     add    r8.xzw, r2.yyzx, -c20.yyzx
     dp3    r10.w, r4.xyzz, r0.xyzz
     mul    r4.xyz, r10.xyzz, r1.w
     +rsq   r1.x, r4.w
     dp3    r4.w, r8.wxzz, r8.wxzz
     add    r10.xyz, r2.xyzz, -c21.xyzz
     max    r11.x, r10.w, c14.x
     +rsq   r1.w, r4.w
     dp3    r10.w, r4.xyzz, r0.xyzz
     mul    r4.xyz, r9.wyzz, r1.x
     +lit   r1.z, r11.xxxw
     dp3    r4.w, r10.xyzz, r10.xyzz
     max    r11.y, r10.w, c14.x
     dp3    r11.x, r4.xyzz, r0.xyzz
     mul    r4.xyz, r8.wxzz, r1.w
     +rsq   r1.x, r4.w
     mul    r8.xyz, r1.z, c26.xyzz          ; direct light
     mul    r11.w, c27.w, r5.w
     +lit   r1.z, r11.yyyz
     max    r11.y, r11.x, c14.x
     dp3    r11.x, r4.xyzz, r0.xyzz
     mul    r4.xyz, r10.xyzz, r1.x
     dp4    r6.y, v0, c0
     dp4    r6.z, v0, c1
     dp4    r6.x, v0, c2
     +dp4   oFog.x, v0, c2
     mad    r8.xyz, r1.z, c22.xyzz, r8.xyzz     ; add extra 1
     mul    r11.w, c27.w, r5.w
     +lit   r1.z, r11.yyyw
     max    r11.y, r11.x, c14.x
     +mov   oPos.xyz, r6.yzxw
     dp3    r11.x, r4.xyzz, r0.xyzz
     dp4    oPos.w, v0, c3
     mad    r8.xyz, r1.z, c23.xyzz, r8.xyzz     ; add extra 2
     mul    r11.w, c27.w, r5.w
     +lit   r1.z, r11.yyyw
     max    r11.y, r11.x, c14.x
    +rcc    r1.x, r12.w
     mad    r8.xyz, r1.z, c24.xyzz, r8.xyzz     ; add extra 3
    mul oPos.xyz, r12.xyzz, c-38.xyzz
     +lit   r1.z, r11.yyyw
     mad    oD0.xyz, r1.z, c25.xyzz, r8.xyzz    ; add extra 4
    mad oPos.xyz, r12.xyzz, r1.x, c-37.xyzz

#### Shader 2

    ;   v0  vertex
    ;   v1  normal
    ;   v2  tex coord
    ;   c0 c1 c2 c3 worldviewproj matrix
    ;   c4 c5 c6    world matrix
    ;   c8 c9       texture matrix?
    ;   c12     eye position
    ;   c13     direct direction
    ;   c14     0.0 0.5 1.0 2.0
    ;   c15     ambient
    ;   c16     color
    ;   c17     direct color
    ;   c18 c19 c20 c21 extra directions
    ;   c22 c23 c24 c25 extra colors
    ;   c26     specular
    ;   c27     spec, fresnel, 0.6, power

     dp3    r0.x, v1.xyzz, c4.xyzz
     dp3    r0.y, v1.xyzz, c5.xyzz
     dp3    r0.z, v1.xyzz, c6.xyzz
     dp4    r3.x, v0, c4
     dp3    r0.w, r0.xyzz, r0.xyzz
     +mov   oT0.xy, v2.xyyy
     dp4    r3.y, v0, c5
     dp4    r3.z, v0, c6
     +rsq   r1.w, r0.w
     add    r2.xyz, c12.xyzz, -r3.xyzz
     mul    r0.xyz, r0.xyzz, r1.w       ; r0.xyz: normalized world-normal
     dp3    r2.w, r2.xyzz, r2.xyzz
     dp3    r0.w, r0.xyzz, -c13.xyzz
     dp3    r11.w, r0.xyzz, -c18.xyzz
     +rsq   r1.w, r2.w
     max    r0.w, r0.w, c14.x
     mul    r2.xyz, r2.xyzz, r1.w       ; r2: normalized world-view-vector
    dp3 r7.x, r2.xyzz, r0.xyzz
     mul    r8.xyz, r0.w, c16.xyzz      ; direct color
    max r5.x, r7.x, c14.x
     mov    r11.xyz, c16
     +mov   oD0.w, c16
     max    r0.w, r11.w, c14.x
     dp3    r11.w, r0.xyzz, -c19.xyzz
    mul r6.x, r7.x, c14.w
    add r5.x, c14.z, -r5.x
     mad    r8.xyz, c15.xyzz, r11.xyzz, r8.xyzz ; add ambient color
     mul    r6.yzw, r0.w, c22.yyzx
     max    r0.w, r11.w, c14.x
     dp3    r11.w, r0.xyzz, -c20.xyzz
    mad r11.xyz, r6.x, r0.xyzz, -r2.xyzz
    mul r6.x, r5.x, r5.x
     dp4    r10.y, v0, c0
     dp4    r10.z, v0, c1
     dp4    r10.x, v0, c2
     +dp4   oFog.x, v0, c2
     mad    r8.xyz, r6.wyzz, c16.xyzz, r8.xyzz  ; add extra light 1
     mul    r6.yzw, r0.w, c23.yyzx
     +mov   oPos.xyz, r10.yzxw
     max    r0.w, r11.w, c14.x
     dp3    r0.x, r0.xyzz, -c21.xyzz
    dp3 r11.x, r11.xyzz, c8.xyzz
    mul r6.x, r6.x, r6.x
    mov r11.w, c27.y
     dp4    oPos.w, v0, c3
     mad    r8.xyz, r6.wyzz, c16.xyzz, r8.xyzz  ; add extra light 2
     mul    r6.yzw, r0.w, c24.yyzx
     +rcc   r1.x, r12.w
     max    r0.w, r0.x, c14.x
    dp3 r11.y, r11.xyzz, c9.xyzz
    mul r5.x, r5.x, r6.x
    add r7.x, c14.z, -r11.w
     mad    r8.xyz, r6.wyzz, c16.xyzz, r8.xyzz  ; add extra light 3
     mul    r6.xyz, r0.w, c25.xyzz
    mul r11.yw, r11.yyyx, c14.y
    mad r5.x, r7.x, r5.x, c27.y
    mul oPos.xyz, r12.xyzz, c-38.xyzz
     mad    oD0.xyz, r6.xyzz, c16.xyzz, r8.xyzz ; add extra light 4
    add oT1.xy, r11.wyyy, c14.y
    mul oD1, r5.x, c27.x
    mad oPos.xyz, r12.xyzz, r1.x, c-37.xyzz

### Gloss shader

    ; v0    vertex pos
    ; v2    tex coord
    ; c0-c3 matrix
    ; c4-c7 world matrix
    ; c12   eye position
    ; c13   light direction
    ; c26   pipeline data...
    ; c27   0, 0, 0, 8.0
    ; c28   0, 0, 1, 1
    ; c29   0.5, 0.5, 0.5, 0.5

     dp4    r3.x, v0, c4
     dp4    r3.y, v0, c5
     dp4    r3.z, v0, c6
     add    r2.xyz, c12.xyzz, -r3.xyzz
     dp4    r6.x, v0, c0
     dp3    r2.w, r2.xyzz, r2.xyzz
     +mov   oT0, v2
     dp4    r6.y, v0, c1
     dp4    r6.z, v0, c2
     +rsq   r1.w, r2.w
     dp4    oPos.w, v0, c3
     mul    r2.xyz, r2.xyzz, r1.w   ; r2: view vector
     +mov   oPos.xyz, r6
    mov r5.xyz, c28.xyzz
    +rcc    r1.x, r12.w
     add    r4.xyz, r2.xyzz, c13.xyzz
     mul    oPos.xyz, r12.xyzz, c-38.xyzz
     dp3    r4.w, r4.xyzz, r4.xyzz
    +mov    oFog.xyz, c26.xyzz
    mad oD0.xyz, r5.xyzz, c29.xyzz, c29.xyzz
     rsq    r4.w, r4.w
     mad    oPos.xyz, r12.xyzz, r1.x, c-37.xyzz
     mul    r5.xyz, r4.xyzz, r4.w
     mad    oD1.xyz, r5.xyzz, c29.xyzz, c29.xyzz

    typedef struct _D3DPixelShaderDef
    {
       DWORD    PSAlphaInputs[8];          // Alpha inputs for each stage
        00000000
        00000000
        00000000
        cdc81010
       DWORD    PSFinalCombinerInputsABCD; // Final combiner inputs
        130f0000
       DWORD    PSFinalCombinerInputsEFG;  // Final combiner inputs (continued)
        0d081c80
       DWORD    PSConstant0[8];            // C0 for each stage
       DWORD    PSConstant1[8];            // C1 for each stage
      DWORD    PSAlphaOutputs[8];         // Alpha output for each stage
        00000000
        00000000
        00000000
        000000c0
       DWORD    PSRGBInputs[8];            // RGB inputs for each stage
        44450000
            PS_REGISTER_V0
            PS_INPUTMAPPING_EXPAND_NORMAL

            PS_REGISTER_V1
            PS_INPUTMAPPING_EXPAND_NORMAL
        cdcd0000
            PS_REGISTER_R1
            PS_INPUTMAPPING_SIGNED_IDENTITY

            PS_REGISTER_R1
            PS_INPUTMAPPING_SIGNED_IDENTITY
        cdcd0000
        cdcd0000
       DWORD    PSCompareMode;             // Compare modes for clipplane texture mode
       DWORD    PSFinalCombinerConstant0;  // C0 in final combiner
       DWORD    PSFinalCombinerConstant1;  // C1 in final combiner
       DWORD    PSRGBOutputs[8];           // Stage 0 RGB outputs
        000020d0
            PS_REGISTER_R1
            PS_COMBINEROUTPUT_AB_DOT_PRODUCT

            PS_REGISTER_ZERO
        000000d0
            PS_REGISTER_R1
            PS_COMBINEROUTPUT_AB_MULTIPLY
        000000d0
        000000d0
       DWORD    PSCombinerCount;           // Active combiner count (Stages 0-7)
        00011104
            4
            PS_COMBINERCOUNT_MUX_MSB
            PS_COMBINERCOUNT_UNIQUE_C0
            PS_COMBINERCOUNT_UNIQUE_C1
       DWORD    PSTextureModes;            // Texture addressing modes
        00000001
            PS_TEXTUREMODES_PROJECT2D
       DWORD    PSDotMapping;              // Input mapping for dot product modes
       DWORD    PSInputTexture;            // Texture source for some texture modes
       DWORD    PSC0Mapping;               // Mapping of c0 regs to D3D constants
       DWORD    PSC1Mapping;               // Mapping of c1 regs to D3D constants
       DWORD    PSFinalCombinerConstants;  // Final combiner constant mapping
    } D3DPIXELSHADERDEF;

### San Andreas Car Shaders

These shaders are picked depending on the following flags:

    1       default
    2       refl && !spec && lit
    3       env && !lit
            env && !spec && lit
    4       spec && lit

    5       refl && spec && lit
    6       env && spec && lit

#### Shader 1

    ; c0-3  matrix
    ; c4-6  world/normal matrix
    ; c13   direct color
    ; c14   0.7 0.7 0.7 1.0
    ; c15   ambient color
    ; c16   light direction
    ; c20   diff mat
    ; c21   amb mat
    ; c22   spec mat
    ; c32   0, 1, 0.5, 2
    ; c33   0.3 0.7 0.1 0.01

     dp4    oPos.x, v0, c0
     dp4    oPos.y, v0, c1
     dp4    oPos.z, v0, c2
     dp4    oPos.w, v0, c3
     dp3    r0.y, v1, c5
     dp3    r0.x, v1, c4
     +mov   oFog.x, r12.w
     dp3    r0.z, v1, c6
    mov r0.w, c20
     +mov   oT0, v2
     mov    r2, c15
     dp3    r0.xyz, r0, -c16
     mul    oPos.xyz, r12, c-38
    +rcc    r1.x, r12.w
     max    r0.xyz, r0.xyzz, c32.x
     min    r0.xyz, r0.xyzz, c32.y
     mul    r0.xyz, r0, c13
     mul    r0.xyz, r0, c20
     mad    oD0, r2, c21, r0
     mad    oPos.xyz, r12, r1.x, c-37

#### Shader 2

    ; v0    position
    ; v1    normal
    ; v2    tex coord
    ; c0-3  matrix
    ; c4-6  world/normal matrix
    ; c12   eye
    ; c13   direct color
    ; c14   0.7 0.7 0.7 1.0
    ; c15   ambient color
    ; c16   light direction
    ; c20   diff mat
    ; c21   amb mat
    ; c22   spec mat
    ; c32   0, 1, 0.5, 2
    ; c33   0.3 0.7 0.1 0.01

     dp3    r0.x, v1.xyzz, c4.xyzz
     dp3    r0.y, v1.xyzz, c5.xyzz
     dp3    r0.z, v1.xyzz, c6.xyzz
    dp4 oPos.w, v0, c3
     dp3    r11.x, r0.xyzz, c8.xyzz
    +mov    oT0, v2
     dp3    r11.y, r0.xyzz, c9.xyzz
    +rcc    r1.x, r12.w
     dp3    r11.z, r0.xyzz, c10.xyzz
    +mov    oFog.x, r12.w
    dp3 r0.w, r0.xyzz, -c16.xyzz
     dp3    r11.w, r11.xyzz, r11.xyzz
    max r0.x, r0.w, c32.x
    min r0.x, r0.x, c32.y
     +rsq   r1.w, r11.w
     mov    r10.y, c32.y
     mul    r10.zw, r11.yyyx, r1.w
    dp4 oPos.x, v0, c0
    dp4 oPos.y, v0, c1
    dp4 oPos.z, v0, c2
    mul r0.xyz, r0.x, c13.xyzz
    mul r0.xyz, r0.xyzz, c20.xyzz
    mov r0.w, c20.w
    mov r2, c15
     dp3    r11.x, r10.wzyy, c24.xyzz
     dp3    r11.y, r10.wzyy, c25.xyzz
    mov r11.z, c32.y
    mul oPos.xyz, r12.xyzz, c-38.xyzz
    mad oD0, r2, c21, r0
    mov oT1, r11.xyzz
    mad oPos.xyz, r12.xyzz, r1.x, c-37.xyzz

#### Shader 3

    ; v0    position
    ; v1    normal
    ; v2    tex coord
    ; c0-3  matrix
    ; c4-6  world/normal matrix
    ; c12   eye
    ; c13   direct color
    ; c14   0.7 0.7 0.7 1.0
    ; c15   ambient color
    ; c16   light direction
    ; c20   diff mat
    ; c21   amb mat
    ; c22   spec mat
    ; c32   0, 1, 0.5, 2
    ; c33   0.3 0.7 0.1 0.01

     dp3    r0.x, v1.xyzz, c4.xyzz
     dp3    r0.y, v1.xyzz, c5.xyzz
     dp3    r0.z, v1.xyzz, c6.xyzz
     dp4    oPos.w, v0, c3
     dp3    r0.w, r0.xyzz, -c16.xyzz
     +mov   oT0, v2
    mov r11.xz, v3.xxyy
    +rcc    r1.x, r12.w
     max    r0.x, r0.w, c32.x
     +mov   oFog.x, r12.w
    mov r11.y, c32.y
     min    r0.x, r0.x, c32.y
     dp4    oPos.x, v0, c0
     dp4    oPos.y, v0, c1
     dp4    oPos.z, v0, c2
     mul    r0.xyz, r0.x, c13.xyzz
     mul    r0.xyz, r0.xyzz, c20.xyzz
     mov    r0.w, c20.w
     mov    r2, c15
    dp3 r10.w, r11.xzyy, c24.xyzz
    dp3 r10.y, r11.xzyy, c25.xyzz
    mov r10.z, v3.w
     mul    oPos.xyz, r12.xyzz, c-38.xyzz
     mad    oD0, r2, c21, r0
    mov oT1, r10.wyzz
     mad    oPos.xyz, r12.xyzz, r1.x, c-37.xyzz

#### Shader 4

    ; v0    position
    ; v1    normal
    ; v2    tex coord
    ; c0-3  matrix
    ; c4-6  world/normal matrix
    ; c12   eye
    ; c13   direct color
    ; c14   0.7 0.7 0.7 1.0
    ; c15   ambient color
    ; c16   light direction
    ; c20   diff mat
    ; c21   amb mat
    ; c22   spec mat
    ; c32   0, 1, 0.5, 2
    ; c33   0.3 0.7 0.1 0.01

     dp4    r10.x, v0, c4
     dp4    r10.y, v0, c5
     dp4    r10.z, v0, c6
     dp3    r11.x, v1.xyzz, c4.xyzz
     add    r0.xyz, -r10.xyzz, c12.xyzz
     dp3    r11.y, v1.xyzz, c5.xyzz
     dp3    r11.w, r0.xyzz, r0.xyzz
     +mov   oD0.w, c20.w
     dp3    r11.z, v1.xyzz, c6.xyzz     ; r11.xyz world normal
     dp4    oPos.w, v0, c3
     +rsq   r1.x, r11.w
     dp3    r4.x, r11.xyzz, -c16.xyzz   ; diffuse factor
     +mov   oT0, v2
     mul    r0.xyz, r0.xyzz, r1.x       ; r0.xyz view vector
    +rcc    r1.y, r12.w
     mul    r4.xyz, r4.x, c13.xyzz
     +mov   oFog.x, r12.w
     add    r2.xyz, r0.xyzz, -c16.xyzz
     mul    r4.xyz, r4.xyzz, c20.xyzz
     dp3    r11.w, r2.xyzz, r2.xyzz
     mul    r4.xyz, r4.xyzz, c32.z
     dp4    oPos.x, v0, c0
     +rsq   r1.x, r11.w
     dp4    oPos.y, v0, c1
     mul    r2.xyz, r2.xyzz, r1.x       ; r2.xyz spec vector
     dp3    r3.x, r11.xyzz, r2.xyzz
     dp4    oPos.z, v0, c2
     mul    r3.x, r3.x, r3.x
     mul    r3.x, r3.x, r3.x
     mul    r3.x, r3.x, r3.x
     mul    r3.x, r3.x, r3.x
     mul    r3.xyz, r3.x, c14.xyzz
     mul    r3.xyz, r3.xyzz, c22.xyzz
     mul    r3.xyz, r3.xyzz, c32.z
     add    r0.xyz, r4.xyzz, r3.xyzz
     mov    r2.xyz, c15.xyzz
     mad    r0.xyz, r2.xyzz, c21.xyzz, r0.xyzz
     mul    oPos.xyz, r12.xyzz, c-38.xyzz
     mov    oD0.xyz, r0
     mad    oPos.xyz, r12.xyzz, r1.y, c-37.xyzz

#### Shader 5

    dp3 r11.x, v1.xyzz, c4.xyzz
    dp3 r11.y, v1.xyzz, c5.xyzz
    dp3 r11.z, v1.xyzz, c6.xyzz
    dp4 r10.x, v0, c4
    dp4 r10.y, v0, c5
    dp4 r10.z, v0, c6
    dp3 r4.x, r11.xyzz, -c16.xyzz
    +mov    oT0, v2
    add r0.xyz, -r10.xyzz, c12.xyzz
    dp3 r10.w, r11.xyzz, c8.xyzz
    dp3 r11.w, r0.xyzz, r0.xyzz
    +mov    oD0.w, c20.w
    dp3 r10.y, r11.xyzz, c9.xyzz
    mul r4.xyz, r4.x, c13.xyzz
    +rsq    r1.x, r11.w
    dp3 r10.z, r11.xyzz, c10.xyzz
    mul r0.xyz, r0.xyzz, r1.x
    add r2.xyz, r0.xyzz, -c16.xyzz
    dp3 r11.w, r10.wyzz, r10.wyzz
    dp3 r10.x, r2.xyzz, r2.xyzz
    mul r4.xyz, r4.xyzz, c20.xyzz
    +rsq    r1.w, r11.w
    dp4 oPos.w, v0, c3
    +rsq    r1.x, r10.x
    mul r4.xyz, r4.xyzz, c32.z
    mul r2.xyz, r2.xyzz, r1.x
    +rcc    r1.y, r12.w
    mul r10.zw, r10.yyyw, r1.w
    +mov    oFog.x, r12.w
    dp3 r3.x, r11.xyzz, r2.xyzz
    mov r10.y, c32.y
    mul r3.x, r3.x, r3.x
    mul r3.x, r3.x, r3.x
    mul r3.x, r3.x, r3.x
    mul r3.x, r3.x, r3.x
    mul r3.xyz, r3.x, c14.xyzz
    mul r3.xyz, r3.xyzz, c22.xyzz
    mul r3.xyz, r3.xyzz, c32.z
    dp4 oPos.x, v0, c0
    dp4 oPos.y, v0, c1
    dp4 oPos.z, v0, c2
    add r0.xyz, r4.xyzz, r3.xyzz
    mov r2.xyz, c15.xyzz
    mad r0.xyz, r2.xyzz, c21.xyzz, r0.xyzz
    dp3 r11.w, r10.wzyy, c24.xyzz
    dp3 r11.y, r10.wzyy, c25.xyzz
    +mov    oD0.xyz, r0
    mov r11.z, c32.y
    mul oPos.xyz, r12.xyzz, c-38.xyzz
    mov oT1, r11.wyzz
    mad oPos.xyz, r12.xyzz, r1.y, c-37.xyzz

#### Shader 6

    ; v0    position
    ; v1    normal
    ; v2    tex coord 1
    ; v3    tex coord 2
    ; c0-3  matrix
    ; c4-6  world/normal matrix
    ; c12   eye
    ; c13   direct color
    ; c14   0.7 0.7 0.7 1.0
    ; c15   ambient color
    ; c16   light direction
    ; c20   diff mat
    ; c21   amb mat
    ; c22   spec mat
    ; c32   0, 1, 0.5, 2
    ; c33   0.3 0.7 0.1 0.01

     dp4    r10.x, v0, c4
     dp4    r10.y, v0, c5
     dp4    r10.z, v0, c6
     dp3    r11.x, v1.xyzz, c4.xyzz
     add    r0.xyz, -r10.xyzz, c12.xyzz
     dp3    r11.y, v1.xyzz, c5.xyzz
     dp3    r11.w, r0.xyzz, r0.xyzz
     +mov   oD0.w, c20.w
     dp3    r11.z, v1.xyzz, c6.xyzz     ; r11 normal
     dp4    oPos.w, v0, c3
     +rsq   r1.x, r11.w
     dp3    r4.x, r11.xyzz, -c16.xyzz
     +mov   oT0, v2
     mul    r0.xyz, r0.xyzz, r1.x       ; r0 view vector
     +rcc   r1.y, r12.w
     mul    r4.xyz, r4.x, c13.xyzz
     +mov   oFog.x, r12.w
     add    r2.xyz, r0.xyzz, -c16.xyzz
     mul    r4.xyz, r4.xyzz, c20.xyzz
     dp3    r11.w, r2.xyzz, r2.xyzz
     mul    r4.xyz, r4.xyzz, c32.z      ; r4 diffuse term
    mov r10.zw, v3.yyyx
     +rsq   r1.x, r11.w
    mov r10.y, c32.y
     mul    r2.xyz, r2.xyzz, r1.x
     dp3    r3.x, r11.xyzz, r2.xyzz
     dp4    oPos.x, v0, c0
     mul    r3.x, r3.x, r3.x
     mul    r3.x, r3.x, r3.x
     mul    r3.x, r3.x, r3.x
     mul    r3.x, r3.x, r3.x
     mul    r3.xyz, r3.x, c14.xyzz
     mul    r3.xyz, r3.xyzz, c22.xyzz
     mul    r3.xyz, r3.xyzz, c32.z      ; r3 specular term
     dp4    oPos.y, v0, c1
     dp4    oPos.z, v0, c2
     add    r0.xyz, r4.xyzz, r3.xyzz
     mov    r2.xyz, c15.xyzz
     mad    r0.xyz, r2.xyzz, c21.xyzz, r0.xyzz
    dp3 r11.w, r10.wzyy, c24.xyzz
    dp3 r11.y, r10.wzyy, c25.xyzz
     +mov   oD0.xyz, r0
    mov r11.z, v3.w
     mul    oPos.xyz, r12.xyzz, c-38.xyzz
    mov oT1, r11.wyzz
     mad    oPos.xyz, r12.xyzz, r1.y, c-37.xyzz

### Shader used for grass

    dp4 oPos.w, v0, c3
    dp4 oPos.x, v0, c0
    dp4 oPos.y, v0, c1
    +rcc    r1.x, r12.w
    dp4 oPos.z, v0, c2
    mov oFog.x, r12.w
    mul oPos.xyz, r12.xyzz, c-38.xyzz
    mov oT0, v1
    mov oD0, c20
    mad oPos.xyz, r12.xyzz, r1.x, c-37.xyzz

### Pixelshader used for world objects (?)

    typedef struct _D3DPixelShaderDef
    {
       DWORD    PSAlphaInputs[8];          // Alpha inputs for each stage
        14181010
            PS_REGISTER_V0
            PS_INPUTMAPPING_UNSIGNED_IDENTITY

            PS_REGISTER_T0
            PS_INPUTMAPPING_UNSIGNED_IDENTITY

            PS_REGISTER_ZERO
            PS_INPUTMAPPING_UNSIGNED_IDENTITY

            PS_REGISTER_ZERO
            PS_INPUTMAPPING_UNSIGNED_IDENTITY
        dcd11010
            PS_REGISTER_R0
            PS_INPUTMAPPING_SIGNED_IDENTITY

            PS_REGISTER_C0
            PS_INPUTMAPPING_SIGNED_IDENTITY

            PS_REGISTER_ZERO
            PS_INPUTMAPPING_UNSIGNED_IDENTITY

            PS_REGISTER_ZERO
            PS_INPUTMAPPING_UNSIGNED_IDENTITY
       DWORD    PSFinalCombinerInputsABCD; // Final combiner inputs
        130c0300
            PS_REGISTER_FOG
            PS_CHANNEL_ALPHA

            PS_REGISTER_R0

            PS_REGISTER_FOG
       DWORD    PSFinalCombinerInputsEFG;  // Final combiner inputs (continued)
        00001c80
            PS_REGISTER_R0
            PS_CHANNEL_ALPHA

            PS_FINALCOMBINERSETTING_CLAMP_SUM
       DWORD    PSConstant0[8];            // C0 for each stage
        ffffffff
        ffffffff
       DWORD    PSConstant1[8];            // C1 for each stage
       DWORD    PSAlphaOutputs[8];         // Alpha output for each stage
        000000c0
            PS_REGISTER_DISCARD

            PS_REGISTER_R0
        000000c0
            PS_REGISTER_DISCARD

            PS_REGISTER_R0
       DWORD    PSRGBInputs[8];            // RGB inputs for each stage
        0804c149
            PS_REGISTER_T0
            PS_INPUTMAPPING_UNSIGNED_IDENTITY

            PS_REGISTER_V0
            PS_INPUTMAPPING_UNSIGNED_IDENTITY

            PS_REGISTER_C0
            PS_INPUTMAPPING_SIGNED_IDENTITY

            PS_REGISTER_T1
            PS_INPUTMAPPING_EXPAND_NORMAL
        cdcc20cc
            PS_REGISTER_R1
            PS_INPUTMAPPING_SIGNED_IDENTITY

            PS_REGISTER_R0
            PS_INPUTMAPPING_SIGNED_IDENTITY

            PS_REGISTER_ZERO
            PS_INPUTMAPPING_UNSIGNED_INVERT

            PS_REGISTER_R0
            PS_INPUTMAPPING_SIGNED_IDENTITY
       DWORD    PSCompareMode;             // Compare modes for clipplane texture mode
       DWORD    PSFinalCombinerConstant0;  // C0 in final combiner
       DWORD    PSFinalCombinerConstant1;  // C1 in final combiner
       DWORD    PSRGBOutputs[8];           // Stage 0 RGB outputs
        000000cd
            PS_REGISTER_R1
            PS_COMBINEROUTPUT_CD_MULTIPLY

            PS_REGISTER_R0
            PS_COMBINEROUTPUT_AB_MULTIPLY
        00000c00
            PS_REGISTER_R0
            PS_COMBINEROUTPUT_AB_CD_SUM
       DWORD    PSCombinerCount;           // Active combiner count (Stages 0-7)
        00011102
            2
            PS_COMBINERCOUNT_MUX_MSB
            PS_COMBINERCOUNT_UNIQUE_C0
            PS_COMBINERCOUNT_UNIQUE_C1
       DWORD    PSTextureModes;            // Texture addressing modes
        00000021
            PS_TEXTUREMODES_PROJECT2D
            PS_TEXTUREMODES_PROJECT2D
       DWORD    PSDotMapping;              // Input mapping for dot product modes
       DWORD    PSInputTexture;            // Texture source for some texture modes
       DWORD    PSC0Mapping;               // Mapping of c0 regs to D3D constants
        ffffff00
       DWORD    PSC1Mapping;               // Mapping of c1 regs to D3D constants
        ffffffff
       DWORD    PSFinalCombinerConstants;  // Final combiner constant mapping
        000001ff
    } D3DPIXELSHADERDEF;

### Xbox vertex formats

    0000XXXC 44443333 2222111 NNNNVVVV

    V N:
        1   0x34    D3DVSDT_PBYTE3
        2   0x35    D3DVSDT_SHORT3
        3   0x31    D3DVSDT_NORMSHORT3
        4   0x16    D3DVSDT_NORMPACKED3     default normal
        5   0x32    D3DVSDT_FLOAT3          default position

    C:
            0x40    D3DVSDT_D3DCOLOR
    T1-4:
        1   0x24    D3DVSDT_PBYTE2
        2   0x25    D3DVSDT_SHORT2
        3   0x21    D3DVSDT_NORMSHORT2
        4   0x16    D3DVSDT_NORMPACKED3
        5   0x22    D3DVSDT_FLOAT2          default

    X:  tangent? XXX is index of tex coord set - 1 that has the data
        anything    0x16    D3DVSDT_NORMPACKED3
                    0x16    D3DVSDT_NORMPACKED3

    default:
        0   position
        2   normal
        3   color
        9-C tex coords

    skin:
        stream 0
        0   position
        3   normal
        4   color
        5-8 tex coords
        stream 1
        1   weights     D3DVSDT_PBYTEn
        2   indices     D3DVSDT_SHORTn

    normmap:
        40320000    ; vertex
        40160001    ; normal
        40220002    ; tex coords
        40160003    ; ?
        40160004    ; ?

        40320000    ; vertex
        40160001    ; normal
        40400002    ; color
        40220003    ; tex coords
        40160004    ; ?
        40160005    ; ?

    20000000h
    40320000h ; vertex      5
    40160001h ; normal      4
    40220002h ; tex coord
    40400004h ; color       5
    0FFFFFFFFh
