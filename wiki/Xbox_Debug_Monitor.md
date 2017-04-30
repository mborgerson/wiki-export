---
title: Xbox Debug Monitor
permalink: wiki/Xbox_Debug_Monitor/
layout: wiki
---

The **Xbox Debug Monitor** (**XBDM**) is a feature of Xbox Development
Kits that provides remote debugging, file management, console discovery,
and other services on TCP/UDP port 731. It is loaded by debug kernels at
startup from `C:\xbdm.dll` and its configuration is read from
`C:\xbdm.ini`. XBDM is distinct from KD and uses a different wire
protocol.

Name Answering Protocol
-----------------------

An Xbox Development Kit (XDK) can be assigned a *debug name* that
identifies it on the local network. XBDM provides the ability to resolve
a debug name to an IP address (forward lookup), resolve an IP address to
a debug name (reverse lookup), and discover all XDKs on the local
network using a very simple UDP-based protocol.

| *Offsets* | Octet | 0    | 1           |
|-----------|-------|------|-------------|
| Octet     | Bit   | 0    | 1           |
| 0         | 0     | Type | Name Length |
| 2         | 16+   | Name |

A NAP packet contains 3 fields, the last of which is variable-length.
The minimum length of a NAP packet is 2 bytes and the maximum is 257.
Invalid packets are silently dropped by XBDM.

Type  
This unsigned 8-bit field may contain the values 1 (lookup), 2 (reply),
or 3 (wildcard).

Name Length  
This unsigned 8-bit field specifies the length of the Name field and
should be a value from 0 to 255. For Type 3 packets, this field should
always be 0. For Type 1 and Type 2 packets, this field should never
be 0.

Name  
This variable-length field contains the ASCII-encoded debug name for
Type 1 and Type 2 packets. The number of bytes in this field is given by
the Length field. It should not contain any `NUL` characters.

### Forward Lookup

To resolve a debug name to an IP address, send a Type 1 NAP packet
containing the debug name to be resolved to UDP address
255.255.255.255:731. The XDK with that name will respond with a Type 2
NAP packet and its IP address can be retrieved from the UDP header.
There is no way to prevent multiple XDKs being assigned the same debug
name, so it's possible that the client may receive replies from multiple
IP addresses.

### Reverse Lookup

To resolve an IP address to a debug name, send a Type 3 NAP packet with
no name (length 0) to the IP address on UDP port 731. Assuming the
target is actually an XDK, it will respond with a Type 2 NAP packet
containing its name. This is very similar to the Console Discovery
process (below), except that by sending the wildcard packet to a single
IP address, only that XDK will respond.

### Console Discovery

To discover all XDKs on the local network, send a Type 3 NAP packet with
no name (length 0) to the UDP address 255.255.255.255:731. Each XDK will
respond with a Type 2 NAP packet containing its name. As with a forward
lookup, the client may receive multiple replies with the same name, but
different IP addresses.

Remote Debugging and Control Protocol
-------------------------------------

The Remote Debugging and Control Protocol (RDCP) is a text-based,
line-oriented, request-response protocol transmitted over a TCP
connection on port 731. RDCP resembles protocols like FTP and SMTP,
making it possible to communicate with XBDM using just a Telnet client.

### Authentication

TODO

### Commands

TODO

See Also
--------

-   [Xbox Neighborhood](/wiki/Xbox_Neighborhood "wikilink") – An XDK tool that
    utilizes the XBDM protocols

External Links
--------------

-   [Yelo
    Neighborhood](https://bitbucket.org/remnantmods/yelo-neighborhood/)
    – An open-source re-implementation of [Xbox
    Neighborhood](/wiki/Xbox_Neighborhood "wikilink") written in C\#.
-   [ViridiX](https://github.com/Ernegien/ViridiX) – An open-source
    collection of Xbox debugging libraries and tools written in C\#.
