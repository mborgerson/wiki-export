---
title: Network
permalink: wiki/Network/
layout: wiki
---

The Xbox contains an Ethernet module and one RJ45 connector.
Additionally, separate modem and wireless accessories were considered
when developing the console. Eventualy an official wireless adapter was
released based of a “D-Link 108AG Gaming Adapter” in the end of 2003.

The Xbox has a TCP/IP protocol stack complete with a DNS PPTP, DHCP
clients.

Port 3074 UDP/TCP is reserved for Xbox communications.

Hardware
--------

Integrated in the Nvidia Southbridge MCPX chip which is similar to the
nForce chips. The Xbox Linux team used the binary drivers from Nvidia.

### Wireless adapter

based on the “D-Link 108AG Gaming Adapter”, the Xbox MN-740 Wireless
Bridge bundled with a Xbox setup disc (wich would update the dashboard
if necessary).

#### Hardware

-   AR5312 CPU (MIPS 4Kc core?)
-   AR5212 RoC (Radio on Chip) for 2.4 Ghz 802.11b/g.
-   KS8721B physical layer transciever
-   some Eeprom wich hold the MAC adress (based of FCC pictures and
    Firmware analysis)
-   IC42S16400 8Mb ram
-   SST39LF0?0A (1 or 2 Mb) (the FCC picture is unclear on the size part
    due to writing)

The onboard 3 leds are: Power, Wireless and Xbox(called Ethernet on the
PCB). The board seems to have Jtag and what apears to be Serial testpins
exposed.

##### Firmware

This wireless bridge runs a closed source version of the“ThreadX
JADE/Green Hills Version G4.0.4.0” RTOS. The firmware contains a
copyright string of: “Copyright (c) Microsoft Corporation All Rights
Reserved Device is Xbox Compatible”

latest firmware is seperated by a boot and runtime firmware :

-   MN740\_01.03.00.0005\_BOOT.bin, “Xbox Wireless Adapter (MN-740) boot
    firmware”
-   MN740\_01.00.02.0022\_RUNTIME.bin, “Xbox Wireless Adapter (MN-740)
    runtime firmware”

###### WPA2 support

The shipped firmware does not support WPA or WPA2. A “firmware” hack
based on the D-Link firmware adds WPA support, rendering Dashboard
support unfunctional and changing settings require connecting to the LAN
port using a PC (or webbrowser capable application).

#### Software (Xbox setup disc)

The setup disc is a CD[1](http://redump.org/disc/53586/). It contains an
XISO filesystem that contains only a “default.xbe” which contains a
dashboard updater.

System Link
-----------

### Secured traffic

Xbox network traffic is secured through
[IPSec](wikipedia:IPSec "wikilink"). The implementation appears to be
similar to [3498, Section
2.1](https://tools.ietf.org/html/rfc3948#section-2.1%7CRFC) from 2005
which was co-authored by Microsoft.

The protocol uses UDP port 3074 which is also registered with the IANA
for use in the
Xbox[2](https://www.iana.org/assignments/service-names-port-numbers/service-names-port-numbers.xhtml?search=3074).
Each Xbox uses the IP 0.0.0.1, so addressing relies on MAC-addresses.

The specific implementation in the Xbox uses TripleDES
([1851](https://tools.ietf.org/html/rfc1851%7CRFC)) for encryption, and
SHA1-96 as [HMAC](wikipedia:HMAC "wikilink").

#### Key derivation

The following keys are involved in generating the actual network
crypto-keys:

-   XboxLANKey (Kernel export)
-   Game specific LAN Key (XBE Certificate Header)

The algorithm to generate the final keys, is this:

    LAN-Hash_1 = HMAC(XboxLANKey, concatenate(0x00, XBE-LAN-Key))
    LAN-Hash_2 = HMAC(XboxLANKey, concatenate(0x01, XBE-LAN-Key))

    LAN-Hash = concatenate(LAN-Hash_1, LAN-Hash_2)

    LAN-SHA = LAN-Hash_0_to_15
    LAN-DES = XcDESKeyParity(LAN-Hash_16_to_39)

[XcDESKeyParity](/wiki/Kernel/XcDESKeyParity "wikilink") is the same as the
respective function in the Xbox kernel.

#### Broadcast messages

Because no security association exists for broadcast messages, these are
handled differently. A common use case for broadcast messages is a
server announce request / response.

Broadcast messages are send to 255.255.255.255 (MAC-address:
FF:FF:FF:FF:FF:FF) using SPI 0xFFFFFFFF and Sequence Number 0xFFFFFFFF.
A random IV is chosen, but nothing prevents re-using an IV.

#### Security association

Most messages require an SA between devices.

### XDK API

| function                                         | description                                                  |
|--------------------------------------------------|--------------------------------------------------------------|
| XNetCreateKey(&xnkid, &xnkey)                    |                                                              |
| XNetRegisterKey(&xnkid, &xnkey)                  | Register the session key                                     |
| XNetXnAddrToInAddr( pxnaddr, pxnkid, &pseudoIP ) | Convert the address to a winsock usable format               |
| XNetUnregisterKey( &xbc.SessionID )              |                                                              |
| XNetGetTitleXnAddr( &hostAddr )                  | Gets your XNADDR. Used by syslink, and lots of other people. |
| XNetGetEthernetLinkStatus()                      |                                                              |

Xbox Live
---------

Xbox Live is an online multiplayer gaming and digital media delivery
service created and operated by Microsoft. It was first made available
to the Xbox system in November 2002.
([Wikipedia](https://en.wikipedia.org/wiki/Xbox_Live)) Xbox Live support
for the original Xbox ended in April 15, 2010.

The Xbox Live architecture consists of authentication servers,
matchmaking servers, and game servers.

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
    content](https://www.google.com/patents/US20040009815)
-   [Patent: Network architecture for secure communications between two
    console-based gaming
    systems](https://www.google.com/patents/US20030093669)
-   [Patent: Architecture for manufacturing authenticatable gaming
    systems](https://www.google.com/patents/US20030093668)
-   [Patent: Discovery and distribution of game session
    information](https://www.google.com/patents/US7803052)
-   [Patent: Security gateway for online console-based
    gaming](https://www.google.com/patents/US20030229779)
-   [Patent: Presence and notification system for maintaining and
    communicating
    information](https://www.google.com/patents/US20030233537)
-   [Patent: Multiple user authentication for online console-based
    gaming](https://www.google.com/patents/US7218739)
-   [Xbox Wireless adapter
    manual](https://web.archive.org/web/20040831091347/http://www.xbox.com:80/assets/en-us/HardwareManuals/Xnewt.pdf)
-   [Flashing the Firmware of an Xbox MN-740 Wireless Adapter to a
    D-Link 108AG to support WPA
    Security](https://www.hanselman.com/blog/FlashingTheFirmwareOfAnXboxMN740WirelessAdapterToADLink108AGToSupportWPASecurity.aspx)
-   [FCC entry of Xbox MN-740 Wireless Adapter (with IC writing
    intact)](https://fccid.io/C3KMN740/Internal-Photos/Internal-Photos-360373.iframe)

