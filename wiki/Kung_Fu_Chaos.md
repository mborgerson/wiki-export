---
title: Kung Fu Chaos
permalink: wiki/Kung_Fu_Chaos/
layout: wiki
---

Fullscreen blit shader
----------------------

<code>

` v0.xy {`  
`   {0,0}`  
`   {0,1}`  
`   {1,1}`  
`   {1,0}`  
` }`  
` `  
` surfaceSize = {640,480}`  
` clipRange = {0,16777215}`  
` `  
` c[58] = { 320, -240, 16777215, 0 }`  
` c[59] = { 320.53125, 240.53125, 0, 0}`  
` `  
` c[100] = {  640,    480, 0, 5.06639553e-05}`  
` c[101] = {    0,      0, 0, 1}`  
` c[102] = {1/320, -1/240, 1, 1}`  
` c[103] = {    1,     -1, 0, 0}`  
` c[104] = {    0,      0, 0, 0}`  
` c[105] = { 320, -240, 16777215, 0 }`  
` c[106] = { 320.53125, 240.53125, 0, 0} `  
` `  
` `  
` /* Slot 0: 0x00000000 0x002D001B 0x0C36106C 0x28200FF8 */`  
` `  
` // R2.x = 0.0;`  
` MOV(R2,x, c[104]);`  
` `  
` /* Slot 1: 0x00000000 0x002CC01B 0x0C36106C 0x2F900FF8 */`  
` `  
` //   R9 = {1/320, -1/240, 1, 1}`  
` MOV(R9,xyzw, c[102]);`  
` `  
` /* Slot 2: 0x00000000 0x024CE200 0x2636186C 0x2C50F81C */`  
` `  
` // R5.xy = R2.x * { -1, +1 }  // -> R5.xy = 0.0 .. FIXME: This makes no sense? R2.x was just set to zero?! [from C104.x !!]`  
` // oD0 = v1`  
` `  
` MUL(R5,xy, R2.x, -c[103]);`  
` MOV(oD0,xyzw, v1);`  
` `  
`   /* Slot 3: 0x00000000 0x006CC0AA 0x0C36146C 0x98300FF8 */`  
`   ADD(R3,x, c[102].z, -R2);  // -> R3.x = C[102].z = 1.0`  
` `  
`   /* Slot 4: 0x00000000 0x008CC000 0x242A1800 0xDC400FF8 */`  
`   MAD(R4,xy, R2.x, c[102].xy, R3.x); // -> R4.xy = R3.xx = {1,1}`  
` `  
`   /* Slot 5: 0x00000000 0x008D2000 0x242A1800 0xDC600FF8 */`  
`   MAD(R6,xy, R2.x, c[105].xy, R3.x); // -> R6.xy = R3.xx = {1,1}`  
` `  
`   /* Slot 6: 0x00000000 0x004D4000 0x242A186C 0x2C700FF8 */`  
`   MUL(R7,xy, R2.x, c[106].xy); // -> R7.xy = 0.0`  
` `  
`   /* Slot 7: 0x00000000 0x0080041B 0x0836886D 0x5C800FF8 */`  
`   MAD(R8,xy, v2, R4, R5); // -> R8 = v2 * R4 = v2`  
` `  
`   oT0.xy = R8 * R6 + R7 // -> v2`  
`   oT1.xy = R8 * R6 + R7 // -> v2`  
` `  
`   /* Slot 8: 0x00000000 0x004CA01A 0x0C37286C 0x2FA00FF8 */`  
`   MUL(R10,xyzw, c[101].xyz, R9);`  
` `  
`   /* Slot 11: 0x00000000 0x006CE01B 0xA436146C 0x3070F800 */`  
`   ADD(oPos,xyzw, R10, -c[103]);`  
` `  
`   /* Slot 12: 0x00000000 0x004C8015 0x942A186C 0x2CB00FF8 */`  
`   MUL(R11,xy, R9.xy, c[100].xy);`  
` `  
`   /* Slot 13: 0x00000000 0x00800015 0x082B6857 0x1070C800 */`  
`   MAD(oPos,xy, v0.xy, R11.xy, R12.xy);`  
` `  
` `  
` SET FOG:`  
` `  
` `  
`   /* Slot 14: 0x00000000 0x004C80FF 0x0DFF886C 0x20708828 */`  
`   MUL(oFog,x, c[100].w, R12.w);`  
` `  
` `  
` `  
` DO VIEWPORT TRANSFORM:`  
` `  
` `  
`   /* Slot 15: 0x00000000 0x0047401B 0xC436186C 0x2070C800 */`  
`   MUL(oPos,xy, R12, c[58]);`  
` `  
`   /* Slot 16: 0x00000000 0x0067601B 0xC436106C 0x3070C801 */`  
`   ADD(oPos,xy, R12, c[59]);`  
` `  
` `  
` `  
` SPACE CONVERSION [XQEMU only]:`  
` `  
` `  
`   if (oPos.w == 0.0 || isinf(oPos.w)) {`  
`     vtx.inv_w = 1.0;`  
`   } else {`  
`     vtx.inv_w = 1.0 / oPos.w;`  
`   }`  
`   oPos.x = 2.0 * (oPos.x - surfaceSize.x * 0.5) / surfaceSize.x;`  
`   oPos.y = -2.0 * (oPos.y - surfaceSize.y * 0.5) / surfaceSize.y;`  
`   if (clipRange.y != clipRange.x) {`  
`     oPos.z = (oPos.z - 0.5 * (clipRange.x + clipRange.y)) / (0.5 * (clipRange.y - clipRange.x));`  
`   }`  
`   if (oPos.w <= 0.0) {`  
`     oPos.xyz *= oPos.w;`  
`   } else {`  
`     oPos.w = 1.0;`  
`   }`  
` `  
` `  
` /* Make the world a happy place! */`  
` #if 1`  
` `  
` oFog.xyzw = vec4(1.0);`  
` `  
` oPos.xy = v0.xy;`  
` oPos.y *= -1.0;`  
` oPos.z = 0.5;`  
` oPos.w = 1.0;`  
` vtx.inv_w = 1.0;`  
` `  
` #endif`  
` `

</code>
