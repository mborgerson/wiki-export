---
title: Network
permalink: wiki/Network/
layout: wiki
---

The Xbox contains an Ethernet module and one RJ45 connector.
Additionally, separate modem and wireless accessories were considered
when developing the console.

The Xbox has a TCP/IP protocol stack complete with a DNS PPTP, DHCP
clients.

Port 3074 UDP/TCP is reserved for Xbox communications.

Hardware
--------

Integrated in the Nvidia Southbridge MCPX chip which is similar to the
nForce chips. The Xbox Linux team used the binary drivers from Nvidia.

System Link
-----------

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

Xbox Live
---------

### Matchmaking servers

### Game servers

### Authentication servers

Authentication and access to Xbox Live services is controlled using the
Kerberos protocol with a few proprietary customizations for the Xbox.

Kerberos Authentication Server: macs.xboxlive.com

| padata-type | description                                              |
|-------------|----------------------------------------------------------|
| 131         | ?                                                        |
| 204         | ?                                                        |
| 206         | Information about Xbox Version, Title, and Title version |

### Xbox Live Functions

| function                                                                                                                          | description                                                                                                                                                                                                                                                            |
|-----------------------------------------------------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| XOnlineGetUsers(XONLINE\_USER\* XBLAccountusers, DWORD\* numOfXBLiveAccounts)                                                     | The XOnlineGetUsers function will enumerate both the hard disk and any attached memory units looking for user accounts                                                                                                                                                 |
| XOnlineTaskClose(XONLINETASK\_HANDLE logonHandle)                                                                                 | Called to abort the authentication process.                                                                                                                                                                                                                            |
| XOnlineStartup( XONLINE\_STARTUP\_PARAMS\* )                                                                                      |                                                                                                                                                                                                                                                                        |
| XOnlineLogon(XONLINE\_USER\* XBLLoggedOnUsers, DWORD\* XBLservices, DWORD SERVICE\_COUNT, NULL, XONLINETASK\_HANDLE &logonHandle) | When a title calls XOnlineLogon to sign in, instead of blocking until the authentication completes, an asynchronous task handle is returned. As part of the authentication process a title must specify which services it will be using (XBLservices, SERVICE\_COUNT). |
| XOnlineTaskContinue(XONLINETASK\_HANDLE logonHandle)                                                                              | Called to check the status of XOnlineLogon.                                                                                                                                                                                                                            |
| XOnlineLogonTaskGetResults(XONLINETASK\_HANDLE logonHandle)                                                                       |                                                                                                                                                                                                                                                                        |
| XOnlineGetLogonUsers()                                                                                                            | This returns a pointer to an array of XONLINE USER structures. This array is similar the XONLINE USER array we populated and passed into XOnlineLogon, but is updated with error status and permission flags for each user.                                            |
| XOnlineSetUserGuestNumber(dwUserFlags , 1)                                                                                        |                                                                                                                                                                                                                                                                        |
| XOnlineTitleUpdate(DWORD)                                                                                                         | The XOnlineTitleUpdate function will boot into an updater application, which performs the actual update                                                                                                                                                                |
| XOnlineGetServiceInfo(Service, ?)                                                                                                 | XOnlineGetServiceInfo returns the connection status for a service                                                                                                                                                                                                      |
| XOnlineNotificationSetState                                                                                                       |                                                                                                                                                                                                                                                                        |
|                                                                                                                                   |                                                                                                                                                                                                                                                                        |
|                                                                                                                                   |                                                                                                                                                                                                                                                                        |
|                                                                                                                                   |                                                                                                                                                                                                                                                                        |
|                                                                                                                                   |                                                                                                                                                                                                                                                                        |
|                                                                                                                                   |                                                                                                                                                                                                                                                                        |
|                                                                                                                                   |                                                                                                                                                                                                                                                                        |
|                                                                                                                                   |                                                                                                                                                                                                                                                                        |

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

References and links
--------------------

-   [<https://xboxlivehacking.blogspot.de/>](https://xboxlivehacking.blogspot.de/)
-   [<https://github.com/grayj/Jedi-Academy/blob/master/codemp/xbox/XBLive.cpp>](https://github.com/grayj/Jedi-Academy/blob/master/codemp/xbox/XBLive.cpp)
-   [<http://discerning.com/pdfbox/test/input/authentication.pdf>](http://discerning.com/pdfbox/test/input/authentication.pdf)
-   [Patent: Managing access to
    content](https://www.google.de/patents/US20040009815)
-   [Patent: Network architecture for secure communications between two
    console-based gaming
    systems](https://www.google.de/patents/US20030093669)
-   [Patent: Architecture for manufacturing authenticatable gaming
    systems](https://www.google.de/patents/US20030093668)
-   [Patent: Discovery and distribution of game session
    information](https://www.google.de/patents/US7803052)
-   [Patent: Security gateway for online console-based
    gaming](https://www.google.de/patents/US20030229779)
-   [Patent: Presence and notification system for maintaining and
    communicating
    information](https://www.google.de/patents/US20030233537)
-   [Patent: Multiple user authentication for online console-based
    gaming](https://www.google.de/patents/US7218739)

