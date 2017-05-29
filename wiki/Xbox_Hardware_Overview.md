---
title: Xbox Hardware Overview
permalink: wiki/Xbox_Hardware_Overview/
layout: wiki
---

Retrieved from
[1](http://www.xbox-linux.org/wiki/Xbox_Hardware_Overview)

The Xbox is a standard PC, based on standard components, but extended
with a some security solutions. This document gives an overview of the
components the Xbox is made of.

Overview
========

Viewed by connection (similar to the Windows 2000 Device Manager view),
the Xbox system hardware looks like this:

Diagram modified by Fahad

    Xbox V1.0-1.5
    |
    |-- Mobile Intel Pentium III 733mHz w/128kb Cache
    |
    |-- Flash ROM (V1.0/1.1 -
    |
    \-- PCI bus
        |
        |-- Programmable Interrupt Controller
        |
        |-- System Timer
        |
        |-- DMA Controller
        |
        |-- PCI Bridge Device - Host Bridge                                 (0:0:0, 0x2a5)
        |
        |-- Memory Controller - SDRAM - Samsung K4D263238M-QC50 133mHz 16mb         (0:0:3, 0x2a6)
        |   |
        |   |-- Top of Motherboard 
        |   |   |
        |   |   |-- Pad 1 (closest to power connector) (empty)
        |   |   |
        |   |   |-- Pad 2 (left of Pad 1)
        |   |   |
        |   |   |-- Pad 3 (below Pad 4)
        |   |   |
        |   |   \-- Pad 4 (closest to AV connector) (empty)
        |   |
        |   \-- Bottom of Motherboard
        |       |
        |       |-- Pad 5 (paralell to Pad 1) (empty)
        |       |
        |       |-- Pad 6 (paralell to Pad 2)
        |       |
        |       |-- Pad 7 (paralell to Pad 3)
        |       |
        |       \-- Pad 8 (paralell to Pad 4) (empty)
        |
        |-- HUB Interface - ISA Bridge                                  (0:1:0, 0x1b2) [nForce]
        |
        |-- SMBus Controller                                            (0:1:1, 0x1b4) [nForce]
        |   |
        |   |-- PIC16LC
        |   |
        |   |-- Conexant 25871/Focus 454/Xcalibur Video Encoder
        |   |
        |   |-- ADM1032 System Temperature Monitor
        |   |
        |   \-- Serial EEPROM
        |
        |-- OHCI USB Controller                                             (0:2:0, 0x1c2) [nForce]
        |   |
        |   \-- USB HUB (Controller Ports 1 &amp; 2) 
        |
        |-- OHCI USB Controller                                             (0:3:0, 0x1c2) [nForce]
        |   |
        |   \-- USB HUB (Controller Ports 3 &amp; 4)
        |
        |-- MCP-X V3 Southbridge 
        |   |
        |   |-- MCP Networking Adapter                                  (0:4:0, 0x1c3) [nForce]
        |   |
        |   \-- MCP APU                                                 (0:5:0, 0x1b0) [nForce]
        |
        |-- AC`97 Audio Codec Interface                                     (0:6:0, 0x1b1) [nForce]
        |
        |-- Simple Communication Controller - Generic Modem     (0:6:1, 0x1c1) [???]
        |
        |-- IDE Controller                                          (0:9:0, 0x1bc) [nForce]
        |   |
        |   |-- Primary IDE Channel
        |   |   |
        |   |   |-- Seagate ST3210014ACE 10.2gb (8gb used)/Custom Western Digital WD80EB 8gb hard drive
        |   |   |
        |   |   \-- Custom Samsung/Phillips/Thompson/LG DVD Drive
        |   |
        |   \--- Secondary IDE Channel
        |
        |-- PCI Bridge                                              (0:8:0, 0x1b8) [nForce]
        |
        \-- AGP Host to PCI Bridge                                      (0:30:0, 0x1b7) [nForce]
            |
            \-- NV2A GeForce 3MX Integrated GPU/Northbridge 233mHz                  (1:0:0, 0x2a0)

  
PCI devices have their Bus:Dev:Func numbers as well as the DevID printed
next to them. PCI devices with “\[nForce\]” have the same DevIDs as
their counterparts of the nForce chipset.

A derivative of the Xbox chipset is also available as the PC chipset
“nVidia nForce”, which consists of a northbridge with the memory
interface and the GPU and the southbridge with the PCI bus and its
devices, just like the Xbox chipset. Many of the southbridge PCI devices
are the same on both versions, i.e. they have the same DevID.

The Devices in Detail
=====================

Intel Pentium III CPU
---------------------

The CPU is a standard Pentium III Coppermine mobile at 733 MHz. This CPU
has been found to be a modified Pentium III, as it has many of the
features a Celeron lacks. Therefore, it can be classified as a Pentium
III with a smaller cache. (Regular has 256kb or more, XBox has 128kb)

Flash ROM
---------

The Flash ROM is mapped at 0xFFF00000 (if the ROM is 1 MB; 0xFFFC0000,
if the ROM is 256 KB) into the but the Xbox kernel.

PCI bus, Host Bridge, ISA Bridge, AGP Host to PCI Bridge, Memory Controller
---------------------------------------------------------------------------

These devices are identical to the corresponding devices contained in
the nForce chipset. The PCI bus can be accessed like on any PC.

Programmable Interrupt Controller, System Timer, DMA Controller
---------------------------------------------------------------

These legacy PC/AT system devices are fully PC-compatible, but the timer
frequency is 6% higher than in a PC.

SMBus Controller
----------------

Some low-speed devices are connected through the SMBus, so the
southbridge contains an SMBus controller, compatible to the SMBus
controller in the nVidia nForce chipset. Since nForce is based on AMD's
original chipset for Athlon CPUs, the SMBus controller is AMD-756
compatible.

PIC16LC
-------

The PIC16LC is a small 8 bit processor running at 20 MHz with its own
ROM, RAM and I/O lines. It controls:

-   Power button
-   Eject button
-   Power/error LED
-   DVD tray status (through extra pins to DVD drive)
-   Video cable type (3 pins)

The PIC is always running, even if the Xbox is turned off. When the
power cable is unplugged, it gets its energy from a capacitor for some
hours.

Video Encoder
-------------

The Xbox does not use an ordinary
[RAMDAC](https://web.archive.org/web/20100617022211/http://en.wikipedia.org/wiki/RAMDAC)
for video output. Instead, it employs a <em>video encoder</em>.

Video encoder is a chip that converts a digital pixel data stream
(coming from the nVidia NV2A graphics processor) into analog video
signal, just like a RAMDAC would. An ordinary RAMDAC, however, can only
output VGA-style RGB signal. The video encoder used in the Xbox is more
flexible, and can generate several different types of signals that
adhere to various video standards and color formats. These include, but
are not necessarily limited to:

-   VGA-style &gt;31 kHz RGB, though only with Sync-on-Green sync
    signals. (If needed, separate HSYNC and VSYNC signals can be
    obtained from the motherboard, or by building a special video cable
    with active electronics for stripping and separating the
    Sync-on-Green sync signal. In any case, separate HSYNC and VSYNC are
    not available directly through the AV connector.)
-   TV-compatible 15 kHz RGB (with composite sync) – suitable for
    European-style SCART RGB output (are progressive 625/50 signals
    supported?)
-   Component (Y'PbPr) signal, both in SDTV and HDTV resolutions;
    suitable for American-style “component” output
-   PAL color signal with typical PAL timings (including PAL60), in both
    composite (CVBS) and s-video (Y/C) formats
-   SECAM color signal with typical SECAM timings, in both composite
    (CVBS) and s-video (Y/C) formats
-   NTSC color signal with typical NTSC timings, in both composite
    (CVBS) and s-video (Y/C) formats
-   Black and white composite video signal without a color carrier

The video encoder is also capable of PALplus style Line 23 Wide Screen
Signalling (WSS), and the Xbox PIC is rigged with the capability of
controlling Scart pin 8 (the *function switching pin*, which is used as
an alternative method of Wide Screen Signalling) and pin 16 (the *fast
switching pin*.)

The make and model of the video encoder has varied through the times –
three different video encoders have been used this far. All three are
very similar in their features; they support various modes and are
flexible enough to be able to output a VGA compatible signal (which is
not supported by the Xbox kernel.) They are, however, not
register-compatible.

Two of the video encoders (namely, Conexant CX25871 and Focus FS454)
also have extensive scaling and filtering functionality, which allows
for [overscan
compensation](https://web.archive.org/web/20100617022211/http://scanline.ca/overscan/)
in desktop-style “TV out” usage. (This means that the GPU can output
ordinary VGA resolutions with VGA timings and the video encoder can
convert them to SDTV resolutions with TV-style timings on the fly,
adding borders around the image so that a projection of the VGA
framebuffer image falls within the “safe area” of the video signal.) The
capabilities of the Xcalibur chip, however, remain a mystery in this
regard: it is not known whether it has a scaler.

All video encoders are connected to (and controlled via)
<a href="/web/20100617022211/http://www.xbox-linux.org/wiki/SMBus_Controller" title="SMBus Controller">IÃÂÃÂÃÂÃÂ²C/SMBus</a>.

### Conexant CX25871

[Conexant
CX25871](https://web.archive.org/web/20100617022211/http://www.conexant.com:9000/cgi-bin/query?mss=srchprod&pg=q&i=IDXPRODSRCH&q=cx25870)
is a close relative of the Brooktree BT868/BT869. There is also a sister
model (CX25870) without the Macrovision capability. This chip was used
in Xbox versions v1.0 through v1.3. If you follow the link, you will
find a product brief and a complete data sheet, with register-level
programming information.

### Focus FS454

[Focus
FS454](https://web.archive.org/web/20100617022211/http://www.focusinfo.com/solutions/catalog.asp?id=30)
was used in v1.4 (and possibly v1.5) Xboxes. There is also a sister
model (FS453) without the Macrovision capability. The data sheet
containing the necessary programming information is available from the
manufacturer by separate request. Copies of it have also been seen
floating around the net.

### Xcalibur

The “Xcalibur” video encoder is a custom chip manufactured for
Microsoft. It was first used in the Xbox hardware revision 1.6.

The Xbox-Linux support the Xcalibur video encoders is very limited at
present (see
<a href="/web/20100617022211/http://www.xbox-linux.org/wiki/Xbox_v1.6_Issues" title="Xbox v1.6 Issues">Xbox
v1.6 Issues</a>). The reason for this is that there is, at least
currently, no official programming documentation available for this
chip. It seems the only way to find out more is through
reverse-engineering techniques.

Xboxes that contain the Xcalibur encoder have the firmware ROM and the
PIC physically integrated into another MS-specific chip, named Xyclops.

ADM1032 System Temperature Monitor
----------------------------------

The Analog Devices ADM1032 is an SMBus device that does temperature
measurement.

Serial EEPROM
-------------

The Serial EEPROM holds some settings, such as time zone, language, IP
address etc., as well as the hard disk key and the Xbox serial number.

OHCI USB Controller, IDE Controller
-----------------------------------

These devices are compatible to standard PC devices.

MCP Networking Adapter, MCP APU, Audio Codec Interface
------------------------------------------------------

These are identical with the nForce devices.

Simple Communication Controller - Generic Modem
-----------------------------------------------

Nothing is known about the purpose of this device. It never gets used by
the Xbox system software. It is probably there, because the nForce has a
PCI softmodem, but in the Xbox, the corresponding lines of the
Southbridge don't seem to be connected.

NV2A GeForce3 Integrated GPU
----------------------------

The Xbox graphics hardware is VGA compatible and very similar to
existing nVidia graphics cards.

Further Information
===================

Information about all chips and links to technical documentation can be
found at [Pixel8's
site](https://web.archive.org/web/20100617022211/http://www.console-dev.com/xbox.htm)
somehting
