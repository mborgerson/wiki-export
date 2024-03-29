---
title: Network
permalink: wiki/Network/
layout: wiki
---

The Xbox contains an Ethernet module and one RJ45 connector.
Additionally, separate modem and wireless accessories were considered
when developing the console. Eventually an official wireless adapter was
released based of a “D-Link 108AG Gaming Adapter” in the end of 2003.

The XDK provides a TCP/IP protocol stack complete with a DNS PPTP, DHCP
clients. The IANA registered port 3074 (UDP / TCP) is reserved for Xbox
communications (See [System Link](/wiki/System_Link "wikilink") and [Xbox
Live](/wiki/Xbox_Live "wikilink")).

Integrated network adapter
--------------------------

Integrated in the Nvidia Southbridge MCPX chip which is similar to the
nForce chips.

The Xbox MAC address is stored in the [EEPROM](/wiki/EEPROM "wikilink"). The
network driver, including the protocol stack is contained in the XDK.
The kernel only contains a small number of exports to reset and get the
state of the NIC.

The Xbox Linux team used the binary drivers from Nvidia.

#### Heartbeat

`   Ethernet II, Src: Microsof_f2:00:00 (00:50:f2:f2:00:00), Dst: Broadcast (ff:ff:ff:ff:ff:ff)`  
`   MS Network Load Balancing`  
`       Signature: Unknown (0x584f4258)`  
`       Version: 1.1`  
`       Unique Host ID: 3118682055`  
`       Cluster IP: 167.102.81.132 (167.102.81.132)`  
`       Host IP: 4.89.169.109 (4.89.169.109)`  
`       Signature Data - Unknown (1481589336)`

Wireless adapter
----------------

based on the “D-Link 108AG Gaming Adapter”, the Xbox MN-740 Wireless
Bridge bundled with a Xbox setup disc (wich would update the dashboard
if necessary). It was also [described on Micosofts
website](https://web.archive.org/web/20040508051958/http://www.xbox.com/en-US/live/connect/msmn740.htm).

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

There were at least 2 firmware updates for download:

-   [MN740
    1.01](https://web.archive.org/web/20031210155952/http://www.microsoft.com/hardware/broadbandnetworking/readme/readme_mn740_101.htm)
-   [MN740
    1.02](https://web.archive.org/web/20040602231929/http://www.microsoft.com/hardware/broadbandnetworking/readme/readme_mn740_102.htm)

Judging by the firmware filenames above, there should also be a MN740
1.00 and MN740 1.03.

###### WPA2 support

The shipped firmware does not support WPA or WPA2. A “firmware” hack
based on the D-Link firmware adds WPA support, rendering Dashboard
support unfunctional and changing settings require connecting to the LAN
port using a PC (or webbrowser capable application).

#### Software (Xbox setup disc)

The setup disc is a CD[1](http://redump.org/disc/53586/). It contains an
XISO filesystem that contains only a “default.xbe” which contains a
dashboard updater.

References and links
--------------------

-   [<https://xboxlivehacking.blogspot.de/>](https://xboxlivehacking.blogspot.de/)
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

