---
title: Kernel/WRITE PORT BUFFER USHORT
permalink: wiki/Kernel/WRITE_PORT_BUFFER_USHORT/
layout: wiki
---

Also see
<https://msdn.microsoft.com/en-us/library/windows/hardware/ff566385(v=vs.85).aspx>

Disassembly
-----------

    mov  eax, edi
    mov  edx, [esp+4]
    mov  edi, [esp+8]
    mov  ecx, [esp+12]
    rep outsw
    mov  edi, eax
    ret 12
