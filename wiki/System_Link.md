---
title: System Link
permalink: wiki/System_Link/
layout: wiki
---

### Secured traffic

Xbox network traffic is secured through
[IPSec](wikipedia:IPSec "wikilink"). The implementation appears to be
similar to [3498, Section
2.1](https://tools.ietf.org/html/rfc3948#section-2.1%7CRFC) from 2005
which was co-authored by Microsoft.

The protocol uses UDP port 3074 which is also registered with the IANA
for use in the
Xbox[1](https://www.iana.org/assignments/service-names-port-numbers/service-names-port-numbers.xhtml?search=3074).
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


