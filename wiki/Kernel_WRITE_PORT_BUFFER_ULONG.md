---
title: Kernel/WRITE PORT BUFFER ULONG
permalink: wiki/Kernel/WRITE_PORT_BUFFER_ULONG/
layout: wiki
---

Also see
<https://msdn.microsoft.com/en-us/library/windows/hardware/ff566384(v=vs.85).aspx>

Disassembly
-----------

    mov  eax, edi
    mov  edx, [esp+4]
    mov  edi, [esp+8]
    mov  ecx, [esp+12]
    rep outsd
    mov  edi, eax
    ret 12
