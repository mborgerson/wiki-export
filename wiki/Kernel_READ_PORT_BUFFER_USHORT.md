---
title: Kernel/READ PORT BUFFER USHORT
permalink: wiki/Kernel/READ_PORT_BUFFER_USHORT/
layout: wiki
---

Also see
<https://msdn.microsoft.com/en-us/library/windows/hardware/ff560794(v=vs.85).aspx>

Disassembly
-----------

    mov  eax, edi
    mov  edx, [esp+4]
    mov  edi, [esp+8]
    mov  ecx, [esp+12]
    rep insw
    mov  edi, eax
    ret 12
