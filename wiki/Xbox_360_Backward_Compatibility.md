---
title: Xbox 360 Backward Compatibility
permalink: wiki/Xbox_360_Backward_Compatibility/
layout: wiki
---

Xbox 360 Backward Compatibility is Microsofts original Xbox emulator for
the Xbox 360.

The emulator binary is called xefu.xex. The first resource is xb1krnl
which is a modified version of [xboxkrnl.exe](/wiki/Kernel "wikilink").

### Modifications to xboxkrnl.exe

The IDEXPRDT section has been dropped, additionally the extra data from
the MS-DOS header is gone.

#### Guest to host communication

The entrypoint of the kernel looks like:

    80030878:   56                      push   %esi
    80030879:   57                      push   %edi
    8003087a:   8d 05 4c ac 02 80       lea    0x8002ac4c,%eax
    80030880:   0f 3f                   (bad)  
    80030882:   04 20
    80030884:   8d 05 6c ac 02 80       lea    0x8002ac6c,%eax
    8003088a:   0f 3f                   (bad)  
    8003088c:   04 20
    8003088e:   8d 05 8c ac 02 80       lea    0x8002ac8c,%eax
    80030894:   0f 3f                   (bad)  
    80030896:   04 21
    80030898:   8d 05 70 94 01 80       lea    0x80019470,%eax
    ...

According to [this document by
symantec](https://www.symantec.com/avcenter/reference/Virtual_Machine_Threats.pdf)
(Page 5, Left-hand-side) the patterns `0F 3F x1 x2` and `0F C7 C8 y1 y2`
are used for communication with the host.

| x1                       | x2   | Notes                                                                                                                              |
|--------------------------|------|------------------------------------------------------------------------------------------------------------------------------------|
| 0x04                     | 0x20 | Seems to use eax (address) as parameter? eax points to a zero terminated list of pointers into the kernel memory \[7 elements\]    |
| 0x04                     | 0x21 | Seems to use eax (address) as parameter? " \[4 elements\]                                                                          |
| 0x04                     | 0x22 | Seems to use eax (address) as parameter? Seems to be some call to that address?!                                                   |
| 0x04                     | 0x23 | Seems to use eax (address) as parameter?                                                                                           |
| 0x04                     | 0x24 | Seems to use eax (address) as parameter?                                                                                           |
| 0x04                     | 0x35 | Seems to use eax (address) as parameter?                                                                                           |
| 0x04                     | 0x50 | Seems to use eax (address) as parameter? " \[3 elements\]                                                                          |
| Cleaner list starts here |
| 0x04                     | 0x20 |                                                                                                                                    |
| 0x04                     | 0x20 |                                                                                                                                    |
| 0x04                     | 0x21 |                                                                                                                                    |
| 0x04                     | 0x22 |                                                                                                                                    |
| 0x04                     | 0x23 |                                                                                                                                    |
| 0x04                     | 0x24 |                                                                                                                                    |
| 0x04                     | 0x35 |                                                                                                                                    |
| 0x04                     | 0x50 |                                                                                                                                    |
| 0x06                     | 0x00 | Seems to use eax (address) and ecx (size) as parameter? Memory is 0x00 filled before. location is 0x8002b420, size would be 0x3000 |
| 0x06                     | 0x02 |                                                                                                                                    |
| 0x06                     | 0x20 | Some call or callback registration to the address pointed to by eax                                                                |
| 0x06                     | 0x21 |                                                                                                                                    |
| 0x06                     | 0x22 |                                                                                                                                    |
| 0x06                     | 0x23 | Some call or callback registration to the address pointed to by eax                                                                |
| 0x06                     | 0x24 |                                                                                                                                    |
| 0x06                     | 0x25 |                                                                                                                                    |
| 0x06                     | 0x26 |                                                                                                                                    |
| 0x06                     | 0x27 |                                                                                                                                    |
| 0x06                     | 0x28 |                                                                                                                                    |
| 0x06                     | 0x29 |                                                                                                                                    |
| 0x06                     | 0x40 |                                                                                                                                    |

References and links
--------------------

-   [Official compatibility list by
    Microsoft](http://support.xbox.com/en-US/legacy-devices/original-console/play-original-games)
-   [Michael Brundages page about the original Xbox emulator in the Xbox
    360](http://michaelbrundage.com/project/xbox-360-emulator/)
    -   [More information about the original Xbox emulator in the Xbox
        360](http://michaelbrundage.com/note/2005/05/15/xbox-360-emulator/)

