---
title: Network
permalink: wiki/Network/
layout: wiki
---

Xbox Live infrastructure
------------------------

Kerberos Authentication Server: macs.xboxlive.com

### Kerberos

| padata-type | description                                              |
|-------------|----------------------------------------------------------|
| 131         | ?                                                        |
| 204         | ?                                                        |
| 206         | Information about Xbox Version, Title, and Title version |

Heartbeat
---------

`   Ethernet II, Src: Microsof_f2:00:00 (00:50:f2:f2:00:00), Dst: Broadcast (ff:ff:ff:ff:ff:ff)`  
`   MS Network Load Balancing`  
`       Signature: Unknown (0x584f4258)`  
`       Version: 1.1`  
`       Unique Host ID: 3118682055`  
`       Cluster IP: 167.102.81.132 (167.102.81.132)`  
`       Host IP: 4.89.169.109 (4.89.169.109)`  
`       Signature Data - Unknown (1481589336)`

Xbox Live Functions
-------------------

| function                                         | description                                                  |
|--------------------------------------------------|--------------------------------------------------------------|
| XNetCreateKey(&xnkid, &xnkey)                    |                                                              |
| XNetRegisterKey(&xnkid, &xnkey)                  | Register the session key                                     |
| XNetXnAddrToInAddr( pxnaddr, pxnkid, &pseudoIP ) | Convert the address to a winsock usable format               |
| XNetUnregisterKey( &xbc.SessionID )              |                                                              |
| XNetGetTitleXnAddr( &hostAddr )                  | Gets your XNADDR. Used by syslink, and lots of other people. |
| XNetGetEthernetLinkStatus()                      |                                                              |
|                                                  |                                                              |
|                                                  |                                                              |
|                                                  |                                                              |
|                                                  |                                                              |
|                                                  |                                                              |
|                                                  |                                                              |
|                                                  |                                                              |

| function                                                                                                                    | description |
|-----------------------------------------------------------------------------------------------------------------------------|-------------|
| XOnlineGetUsers( XBLAccountusers, &numOfXBLiveAccounts )                                                                    |             |
| XOnlineTaskClose(XONLINETASK\_HANDLE logonHandle)                                                                           |             |
| XOnlineStartup( XONLINE\_STARTUP\_PARAMS\* )                                                                                |             |
| XOnlineLogon(XONLINE\_USER\* XBLLoggedOnUsers, DWORD\* XBLservices, SERVICE\_COUNT, NULL, XONLINETASK\_HANDLE &logonHandle) |             |
| XOnlineTaskContinue(XONLINETASK\_HANDLE logonHandle)                                                                        |             |
| XOnlineLogonTaskGetResults(XONLINETASK\_HANDLE logonHandle)                                                                 |             |
| XOnlineGetLogonUsers()                                                                                                      |             |
|                                                                                                                             |             |
|                                                                                                                             |             |
|                                                                                                                             |             |
|                                                                                                                             |             |

References and links
--------------------

-   [<https://xboxlivehacking.blogspot.de/>](https://xboxlivehacking.blogspot.de/)
-   [<https://github.com/grayj/Jedi-Academy/blob/master/codemp/xbox/XBLive.cpp>](https://github.com/grayj/Jedi-Academy/blob/master/codemp/xbox/XBLive.cpp)
-   [<http://discerning.com/pdfbox/test/input/authentication.pdf>](http://discerning.com/pdfbox/test/input/authentication.pdf)

