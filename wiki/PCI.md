---
title: PCI
permalink: wiki/PCI/
layout: wiki
---

Overview
--------

Most Xbox devices are ultimately exposed to the CPU via a PCI bus.

PCI works by allowing individual devices to be interrogated and remapped
through the physical address space (both memory and IO space). This
interrogation and configuration occurs in a special address space known
as PCI Config Space. Typically an operating system scans the entire PCI
Config Space at initilization time in order to map the relevant devices
and load drivers. Like a lot of single purpose embedded systems, the
Xbox was shipped at the point of being a minimum viable product (ie. the
PCI bus is horribly broken in remarkably crazy ways).

PCI Config Addresses consist of three or four hex numbers, typically in
the form of BB.DD:F or BB.DD:F.RR where

-   BB - Bus number - 00 through FF
-   DD - Device Number - 00 through 1F
-   F - Function Number - 0 through 7
-   RR - Register In PCI Config Space - 00 through FC (register access
    is aligned on 4 byte words)

Devices
-------

All dumps are from a DVT4 completely booted to the official 5558 debug
ROM

### 00.00:0 - Host Bridge Adapter

XCodes write 7ffffff to config reg 84 pretty soon in boot (looks like
part of DRAM init).

`00.00:0.00: 02a510de`  
`00.00:0.04: 00100006`  
`00.00:0.08: 060000a1`  
`00.00:0.0c: 00800000`  
`00.00:0.10: 40000008`  
`00.00:0.14: 00000000`  
`00.00:0.18: 00000000`  
`00.00:0.1c: 00000000`  
`00.00:0.20: 00000000`  
`00.00:0.24: 00000000`  
`00.00:0.28: 00000000`  
`00.00:0.2c: 00000000`  
`00.00:0.30: 00000000`  
`00.00:0.34: 00000040`  
`00.00:0.38: 00000000`  
`00.00:0.3c: 00000000`  
`00.00:0.40: 00206002`  
`00.00:0.44: 1f000217`  
`00.00:0.48: 00000014`  
`00.00:0.4c: 00000001`  
`00.00:0.50: 00000000`  
`00.00:0.54: 10000000`  
`00.00:0.58: ffffffff`  
`00.00:0.5c: ffffffff`  
`00.00:0.60: 20010008`  
`00.00:0.64: 88880120`  
`00.00:0.68: 00000010`  
`00.00:0.6c: 0f0f0f21`  
`00.00:0.70: ffffffff`  
`00.00:0.74: ffffffff`  
`00.00:0.78: ffffffff`  
`00.00:0.7c: ffffffff`  
`00.00:0.80: 00000100`  
`00.00:0.84: 07ffffff`  
`00.00:0.88: 00000001`  
`00.00:0.8c: 3ff6f417`  
`00.00:0.90: 00000000`  
`00.00:0.94: f9feffff`  
`00.00:0.98: 00000000`  
`00.00:0.9c: 00000000`  
`00.00:0.a0: 00000000`  
`00.00:0.a4: 00002001`  
`00.00:0.a8: 00000000`  
`00.00:0.ac: 00000000`  
`00.00:0.b0: 00000001`  
`00.00:0.b4: 00000000`  
`00.00:0.b8: 00000000`  
`00.00:0.bc: 00000000`  
`00.00:0.c0: 00033333`  
`00.00:0.c4: 00033333`  
`00.00:0.c8: 00000013`  
`00.00:0.cc: 00000000`  
`00.00:0.d0: 00000000`  
`00.00:0.d4: 00000001`  
`00.00:0.d8: 07f0ffff`  
`00.00:0.dc: 00000000`  
`00.00:0.e0: 00400006`  
`00.00:0.e4: 01ff75b7`  
`00.00:0.e8: 00000000`  
`00.00:0.ec: 00000000`  
`00.00:0.f0: f0000001`  
`00.00:0.f4: 00000000`  
`00.00:0.f8: 00000000`  
`00.00:0.fc: 00000000`

`BAR0: 40000008 - c0000008 - 1073741824 bytes`  
`BAR1:        0 -        0 - 0 bytes`  
`BAR2:        0 -        0 - 0 bytes`  
`BAR3:        0 -        0 - 0 bytes`  
`BAR4:        0 -        0 - 0 bytes`  
`BAR5:        0 -        0 - 0 bytes`

### 00.00:1 & 00.00:2 - Broken Devices

Reading PCI Config Space for these devices will freeze the console.
Looking at the similar nForce chipsets, it is likely that these are DRAM
controllers that aren't complete.

### 00.00:3 - DRAM Controller

`00.00:3.00: 02a610de`  
`00.00:3.04: 00200000`  
`00.00:3.08: 050000a1`  
`00.00:3.0c: 00800000`  
`00.00:3.10: 00000000`  
`00.00:3.14: 00000000`  
`00.00:3.18: 00000000`  
`00.00:3.1c: 00000000`  
`00.00:3.20: 00000000`  
`00.00:3.24: 00000000`  
`00.00:3.28: 00000000`  
`00.00:3.2c: 00000000`  
`00.00:3.30: 00000000`  
`00.00:3.34: 00000000`  
`00.00:3.38: 00000000`  
`00.00:3.3c: 00000000`  
`00.00:3.40: f0f0c0c0`  
`00.00:3.44: 00c00000`  
`00.00:3.48: 00000000`  
`00.00:3.4c: 00000000`  
`00.00:3.50: 00000000`  
`00.00:3.54: 00000000`  
`00.00:3.58: 00000000`  
`00.00:3.5c: 04070000`  
`00.00:3.60: 00000000`  
`00.00:3.64: 01208001`  
`00.00:3.68: 0000006c`  
`00.00:3.6c: 01230801`  
`00.00:3.70: 00000000`  
`00.00:3.74: 00000000`  
`00.00:3.78: 00000000`  
`00.00:3.7c: 00000000`  
`00.00:3.80: 00000000`  
`00.00:3.84: 00000000`  
`00.00:3.88: 00000000`  
`00.00:3.8c: 00000000`  
`00.00:3.90: 00000000`  
`00.00:3.94: 00000000`  
`00.00:3.98: 00000000`  
`00.00:3.9c: 00000000`  
`00.00:3.a0: 00000000`  
`00.00:3.a4: 00000000`  
`00.00:3.a8: 00000000`  
`00.00:3.ac: 00000000`  
`00.00:3.b0: 00000000`  
`00.00:3.b4: 00000000`  
`00.00:3.b8: 00000000`  
`00.00:3.bc: 00000000`  
`00.00:3.c0: 00000000`  
`00.00:3.c4: 00000000`  
`00.00:3.c8: 00000000`  
`00.00:3.cc: 00000000`  
`00.00:3.d0: 00000000`  
`00.00:3.d4: 00000000`  
`00.00:3.d8: 00000000`  
`00.00:3.dc: 00000000`  
`00.00:3.e0: 00000000`  
`00.00:3.e4: 00000000`  
`00.00:3.e8: 00000000`  
`00.00:3.ec: 00000000`  
`00.00:3.f0: 00000000`  
`00.00:3.f4: 00000000`  
`00.00:3.f8: 00000000`  
`00.00:3.fc: 00000000`

`BAR0:        0 -        0 - 0 bytes`  
`BAR1:        0 -        0 - 0 bytes`  
`BAR2:        0 -        0 - 0 bytes`  
`BAR3:        0 -        0 - 0 bytes`  
`BAR4:        0 -        0 - 0 bytes`  
`BAR5:        0 -        0 - 0 bytes`

### 00.01:0 - ISA Bridge

This is probably the LPC bus controller (LPC is just a serial version of
the classic ISA bus). A lot of random system init stuff gets written
here on boot, both to it's IO space, and it's config space.

`00.01:0.00: 01b210de`  
`00.01:0.04: 00b0000f`  
`00.01:0.08: 060100b2`  
`00.01:0.0c: 00800000`  
`00.01:0.10: 00008001`  
`00.01:0.14: 00000000`  
`00.01:0.18: 00000000`  
`00.01:0.1c: 00000000`  
`00.01:0.20: 00000000`  
`00.01:0.24: 00000000`  
`00.01:0.28: 00000000`  
`00.01:0.2c: 00000000`  
`00.01:0.30: 00000000`  
`00.01:0.34: 00000050`  
`00.01:0.38: 00000000`  
`00.01:0.3c: 00000000`  
`00.01:0.40: 00000000`  
`00.01:0.44: 20000000`  
`00.01:0.48: 00000440`  
`00.01:0.4c: 0000fdde`  
`00.01:0.50: 01410008`  
`00.01:0.54: 88880070`  
`00.01:0.58: 000000a0`  
`00.01:0.5c: 00000000`  
`00.01:0.60: 00000400`  
`00.01:0.64: 00000b0c`  
`00.01:0.68: 00031000`  
`00.01:0.6c: 0e065491`  
`00.01:0.70: 00000000`  
`00.01:0.74: 00000000`  
`00.01:0.78: 00000000`  
`00.01:0.7c: 00000000`  
`00.01:0.80: 04001800`  
`00.01:0.84: 00000000`  
`00.01:0.88: 0000000f`  
`00.01:0.8c: 48000000`  
`00.01:0.90: 00000080`  
`00.01:0.94: 00000000`  
`00.01:0.98: 811ced04`  
`00.01:0.9c: 871cc707`  
`00.01:0.a0: 00000000`  
`00.01:0.a4: 01500044`  
`00.01:0.a8: 00150004`  
`00.01:0.ac: 00070001`  
`00.01:0.b0: 02010000`  
`00.01:0.b4: 00010f12`  
`00.01:0.b8: 00000000`  
`00.01:0.bc: 01008001`  
`00.01:0.c0: 00000000`  
`00.01:0.c4: 00000000`  
`00.01:0.c8: 00000000`  
`00.01:0.cc: 00000000`  
`00.01:0.d0: 607f7e7c`  
`00.01:0.d4: 7c7e7c78`  
`00.01:0.d8: 00007e7f`  
`00.01:0.dc: 00000000`  
`00.01:0.e0: 00000000`  
`00.01:0.e4: 00000000`  
`00.01:0.e8: 00000000`  
`00.01:0.ec: 00000000`  
`00.01:0.f0: 00000f00`  
`00.01:0.f4: 00000000`  
`00.01:0.f8: 00000000`  
`00.01:0.fc: 00000000`

`BAR0:     8001 - ffffff01 - 256 bytes`  
`BAR1:        0 -        0 - 0 bytes`  
`BAR2:        0 -        0 - 0 bytes`  
`BAR3:        0 -        0 - 0 bytes`  
`BAR4:        0 -        0 - 0 bytes`  
`BAR5:        0 -        0 - 0 bytes`

### 00.01:1 - SMBUS Controller

It's unknown what backs BAR0 and BAR2. BAR0 is never quite setup, and is
therefor pointed to the IO ports from 00 to 0F. Cromwell interestingly
writes to IO port 00 accidentally.

`00.01:1.00: 01b410de`  
`00.01:1.04: 00b00001`  
`00.01:1.08: 0c0500b1`  
`00.01:1.0c: 00800000`  
`00.01:1.10: 00000001`  
`00.01:1.14: 0000c001`  
`00.01:1.18: 0000c201`  
`00.01:1.1c: 00000000`  
`00.01:1.20: 00000000`  
`00.01:1.24: 00000000`  
`00.01:1.28: 00000000`  
`00.01:1.2c: 00000000`  
`00.01:1.30: 00000000`  
`00.01:1.34: 00000044`  
`00.01:1.38: 00000000`  
`00.01:1.3c: 01030100`  
`00.01:1.40: 00000000`  
`00.01:1.44: 00020001`  
`00.01:1.48: 00000000`  
`00.01:1.4c: 00000000`  
`00.01:1.50: 00000000`  
`00.01:1.54: 00000000`  
`00.01:1.58: 00000000`  
`00.01:1.5c: 00000000`  
`00.01:1.60: 00000000`  
`00.01:1.64: 00000000`  
`00.01:1.68: 00000000`  
`00.01:1.6c: 00000000`  
`00.01:1.70: 00000000`  
`00.01:1.74: 00000000`  
`00.01:1.78: 00000000`  
`00.01:1.7c: 00000000`  
`00.01:1.80: 00000000`  
`00.01:1.84: 00000000`  
`00.01:1.88: 00000000`  
`00.01:1.8c: 00000000`  
`00.01:1.90: 00000000`  
`00.01:1.94: 00000000`  
`00.01:1.98: 00000000`  
`00.01:1.9c: 00000000`  
`00.01:1.a0: 00000000`  
`00.01:1.a4: 00000000`  
`00.01:1.a8: 00000000`  
`00.01:1.ac: 00000000`  
`00.01:1.b0: 00000000`  
`00.01:1.b4: 00000000`  
`00.01:1.b8: 00000000`  
`00.01:1.bc: 00000000`  
`00.01:1.c0: 00000000`  
`00.01:1.c4: 00000000`  
`00.01:1.c8: 00000000`  
`00.01:1.cc: 00000000`  
`00.01:1.d0: 00000000`  
`00.01:1.d4: 00000000`  
`00.01:1.d8: 00000000`  
`00.01:1.dc: 00000000`  
`00.01:1.e0: 00000000`  
`00.01:1.e4: 00000000`  
`00.01:1.e8: 00000000`  
`00.01:1.ec: 00000000`  
`00.01:1.f0: 00000000`  
`00.01:1.f4: 00000000`  
`00.01:1.f8: 00000000`  
`00.01:1.fc: 00000000`

`BAR0:        1 - fffffff1 - 16 bytes`  
`BAR1:     c001 - fffffff1 - 16 bytes`  
`BAR2:     c201 - ffffffe1 - 32 bytes`  
`BAR3:        0 -        0 - 0 bytes`  
`BAR4:        0 -        0 - 0 bytes`  
`BAR5:        0 -        0 - 0 bytes`

### 00.02:0 - First OHCI controller

`00.02:0.00: 01c210de`  
`00.02:0.04: 00b00006`  
`00.02:0.08: 0c0310b1`  
`00.02:0.0c: 00000000`  
`00.02:0.10: fed00000`  
`00.02:0.14: 00000000`  
`00.02:0.18: 00000000`  
`00.02:0.1c: 00000000`  
`00.02:0.20: 00000000`  
`00.02:0.24: 00000000`  
`00.02:0.28: 00000000`  
`00.02:0.2c: 00000000`  
`00.02:0.30: 00000000`  
`00.02:0.34: 00000044`  
`00.02:0.38: 00000000`  
`00.02:0.3c: 01030101`  
`00.02:0.40: 00000000`  
`00.02:0.44: da020001`  
`00.02:0.48: 00000000`  
`00.02:0.4c: 00000002`  
`00.02:0.50: 0000000f`  
`00.02:0.54: 00000000`  
`00.02:0.58: 00000000`  
`00.02:0.5c: 00000000`  
`00.02:0.60: 00000000`  
`00.02:0.64: 00000000`  
`00.02:0.68: 00000000`  
`00.02:0.6c: 00000000`  
`00.02:0.70: 00000000`  
`00.02:0.74: 00000000`  
`00.02:0.78: 00000000`  
`00.02:0.7c: 00000000`  
`00.02:0.80: 00000000`  
`00.02:0.84: 00000000`  
`00.02:0.88: 00000000`  
`00.02:0.8c: 00000000`  
`00.02:0.90: 00000000`  
`00.02:0.94: 00000000`  
`00.02:0.98: 00000000`  
`00.02:0.9c: 00000000`  
`00.02:0.a0: 00000000`  
`00.02:0.a4: 00000000`  
`00.02:0.a8: 00000000`  
`00.02:0.ac: 00000000`  
`00.02:0.b0: 00000000`  
`00.02:0.b4: 00000000`  
`00.02:0.b8: 00000000`  
`00.02:0.bc: 00000000`  
`00.02:0.c0: 00000000`  
`00.02:0.c4: 00000000`  
`00.02:0.c8: 00000000`  
`00.02:0.cc: 00000000`  
`00.02:0.d0: 00000000`  
`00.02:0.d4: 00000000`  
`00.02:0.d8: 00000000`  
`00.02:0.dc: 00000000`  
`00.02:0.e0: 00000000`  
`00.02:0.e4: 00000000`  
`00.02:0.e8: 00000000`  
`00.02:0.ec: 00000000`  
`00.02:0.f0: 00000000`  
`00.02:0.f4: 00000000`  
`00.02:0.f8: 00000000`  
`00.02:0.fc: 00000000`

`BAR0: fed00000 - fffff000 - 4096 bytes`  
`BAR1:        0 -        0 - 0 bytes`  
`BAR2:        0 -        0 - 0 bytes`  
`BAR3:        0 -        0 - 0 bytes`  
`BAR4:        0 -        0 - 0 bytes`  
`BAR5:        0 -        0 - 0 bytes`

### 00.03:0 - Second OHCI Controller

`00.03:0.00: 01c210de`  
`00.03:0.04: 00b00006`  
`00.03:0.08: 0c0310b1`  
`00.03:0.0c: 00000000`  
`00.03:0.10: fed08000`  
`00.03:0.14: 00000000`  
`00.03:0.18: 00000000`  
`00.03:0.1c: 00000000`  
`00.03:0.20: 00000000`  
`00.03:0.24: 00000000`  
`00.03:0.28: 00000000`  
`00.03:0.2c: 00000000`  
`00.03:0.30: 00000000`  
`00.03:0.34: 00000044`  
`00.03:0.38: 00000000`  
`00.03:0.3c: 01030109`  
`00.03:0.40: 00000000`  
`00.03:0.44: da020001`  
`00.03:0.48: 00000000`  
`00.03:0.4c: 00000003`  
`00.03:0.50: 00000030`  
`00.03:0.54: 00000000`  
`00.03:0.58: 00000000`  
`00.03:0.5c: 00000000`  
`00.03:0.60: 00000000`  
`00.03:0.64: 00000000`  
`00.03:0.68: 00000000`  
`00.03:0.6c: 00000000`  
`00.03:0.70: 00000000`  
`00.03:0.74: 00000000`  
`00.03:0.78: 00000000`  
`00.03:0.7c: 00000000`  
`00.03:0.80: 00000000`  
`00.03:0.84: 00000000`  
`00.03:0.88: 00000000`  
`00.03:0.8c: 00000000`  
`00.03:0.90: 00000000`  
`00.03:0.94: 00000000`  
`00.03:0.98: 00000000`  
`00.03:0.9c: 00000000`  
`00.03:0.a0: 00000000`  
`00.03:0.a4: 00000000`  
`00.03:0.a8: 00000000`  
`00.03:0.ac: 00000000`  
`00.03:0.b0: 00000000`  
`00.03:0.b4: 00000000`  
`00.03:0.b8: 00000000`  
`00.03:0.bc: 00000000`  
`00.03:0.c0: 00000000`  
`00.03:0.c4: 00000000`  
`00.03:0.c8: 00000000`  
`00.03:0.cc: 00000000`  
`00.03:0.d0: 00000000`  
`00.03:0.d4: 00000000`  
`00.03:0.d8: 00000000`  
`00.03:0.dc: 00000000`  
`00.03:0.e0: 00000000`  
`00.03:0.e4: 00000000`  
`00.03:0.e8: 00000000`  
`00.03:0.ec: 00000000`  
`00.03:0.f0: 00000000`  
`00.03:0.f4: 00000000`  
`00.03:0.f8: 00000000`  
`00.03:0.fc: 00000000`

`BAR0: fed08000 - fffff000 - 4096 bytes`  
`BAR1:        0 -        0 - 0 bytes`  
`BAR2:        0 -        0 - 0 bytes`  
`BAR3:        0 -        0 - 0 bytes`  
`BAR4:        0 -        0 - 0 bytes`  
`BAR5:        0 -        0 - 0 bytes`

### 00.04:0 - Network Adapter

`00.04:0.00: 01c310de`  
`00.04:0.04: 00b00007`  
`00.04:0.08: 020000b1`  
`00.04:0.0c: 00000000`  
`00.04:0.10: fef00000`  
`00.04:0.14: 0000e001`  
`00.04:0.18: 00000000`  
`00.04:0.1c: 00000000`  
`00.04:0.20: 00000000`  
`00.04:0.24: 00000000`  
`00.04:0.28: 00000000`  
`00.04:0.2c: 00000000`  
`00.04:0.30: 00000000`  
`00.04:0.34: 00000044`  
`00.04:0.38: 00000000`  
`00.04:0.3c: 14010104`  
`00.04:0.40: 00000000`  
`00.04:0.44: fe020001`  
`00.04:0.48: 00000000`  
`00.04:0.4c: 00000004`  
`00.04:0.50: 00000000`  
`00.04:0.54: 00000000`  
`00.04:0.58: 00000000`  
`00.04:0.5c: 00000000`  
`00.04:0.60: 00000000`  
`00.04:0.64: 00000000`  
`00.04:0.68: 00000000`  
`00.04:0.6c: 00000000`  
`00.04:0.70: 00000000`  
`00.04:0.74: 00000000`  
`00.04:0.78: 00000000`  
`00.04:0.7c: 00000000`  
`00.04:0.80: 00000000`  
`00.04:0.84: 00000000`  
`00.04:0.88: 00000000`  
`00.04:0.8c: 00000000`  
`00.04:0.90: 00000000`  
`00.04:0.94: 00000000`  
`00.04:0.98: 00000000`  
`00.04:0.9c: 00000000`  
`00.04:0.a0: 00000000`  
`00.04:0.a4: 00000000`  
`00.04:0.a8: 00000000`  
`00.04:0.ac: 00000000`  
`00.04:0.b0: 00000000`  
`00.04:0.b4: 00000000`  
`00.04:0.b8: 00000000`  
`00.04:0.bc: 00000000`  
`00.04:0.c0: 00000000`  
`00.04:0.c4: 00000000`  
`00.04:0.c8: 00000000`  
`00.04:0.cc: 00000000`  
`00.04:0.d0: 00000000`  
`00.04:0.d4: 00000000`  
`00.04:0.d8: 00000000`  
`00.04:0.dc: 00000000`  
`00.04:0.e0: 00000000`  
`00.04:0.e4: 00000000`  
`00.04:0.e8: 00000000`  
`00.04:0.ec: 00000000`  
`00.04:0.f0: 00000000`  
`00.04:0.f4: 00000000`  
`00.04:0.f8: 00000000`  
`00.04:0.fc: 00000000`

`BAR0: fef00000 - fffffc00 - 1024 bytes`  
`BAR1:     e001 - fffffff9 - 16 bytes`  
`BAR2:        0 -        0 - 0 bytes`  
`BAR3:        0 -        0 - 0 bytes`  
`BAR4:        0 -        0 - 0 bytes`  
`BAR5:        0 -        0 - 0 bytes`

### 00.05:0 - APU

`00.05:0.00: 01b010de`  
`00.05:0.04: 00b00006`  
`00.05:0.08: 040100b1`  
`00.05:0.0c: 00000000`  
`00.05:0.10: fe800000`  
`00.05:0.14: 00000000`  
`00.05:0.18: 00000000`  
`00.05:0.1c: 00000000`  
`00.05:0.20: 00000000`  
`00.05:0.24: 00000000`  
`00.05:0.28: 00000000`  
`00.05:0.2c: 00000000`  
`00.05:0.30: 00000000`  
`00.05:0.34: 00000044`  
`00.05:0.38: 00000000`  
`00.05:0.3c: 0c010105`  
`00.05:0.40: 00000000`  
`00.05:0.44: 00020001`  
`00.05:0.48: 00000000`  
`00.05:0.4c: 0000050a`  
`00.05:0.50: 00020001`  
`00.05:0.54: 00020001`  
`00.05:0.58: 00020001`  
`00.05:0.5c: 00020001`  
`00.05:0.60: 00020001`  
`00.05:0.64: 00020001`  
`00.05:0.68: 00020001`  
`00.05:0.6c: 00020001`  
`00.05:0.70: 00020001`  
`00.05:0.74: 00020001`  
`00.05:0.78: 00020001`  
`00.05:0.7c: 00020001`  
`00.05:0.80: 00020001`  
`00.05:0.84: 00020001`  
`00.05:0.88: 00020001`  
`00.05:0.8c: 00020001`  
`00.05:0.90: 00020001`  
`00.05:0.94: 00020001`  
`00.05:0.98: 00020001`  
`00.05:0.9c: 00020001`  
`00.05:0.a0: 00020001`  
`00.05:0.a4: 00020001`  
`00.05:0.a8: 00020001`  
`00.05:0.ac: 00020001`  
`00.05:0.b0: 00020001`  
`00.05:0.b4: 00020001`  
`00.05:0.b8: 00020001`  
`00.05:0.bc: 00020001`  
`00.05:0.c0: 00020001`  
`00.05:0.c4: 00020001`  
`00.05:0.c8: 00020001`  
`00.05:0.cc: 00020001`  
`00.05:0.d0: 00020001`  
`00.05:0.d4: 00020001`  
`00.05:0.d8: 00020001`  
`00.05:0.dc: 00020001`  
`00.05:0.e0: 00020001`  
`00.05:0.e4: 00020001`  
`00.05:0.e8: 00020001`  
`00.05:0.ec: 00020001`  
`00.05:0.f0: 00020001`  
`00.05:0.f4: 00020001`  
`00.05:0.f8: 00020001`  
`00.05:0.fc: 00020001`

`BAR0: fe800000 - fff80000 - 524288 bytes`  
`BAR1:        0 -        0 - 0 bytes`  
`BAR2:        0 -        0 - 0 bytes`  
`BAR3:        0 -        0 - 0 bytes`  
`BAR4:        0 -        0 - 0 bytes`  
`BAR5:        0 -        0 - 0 bytes`

### 00.06:0 - AC97-esque Controller

`00.06:0.00: 01b110de`  
`00.06:0.04: 00b00007`  
`00.06:0.08: 040100b1`  
`00.06:0.0c: 00800000`  
`00.06:0.10: 0000d001`  
`00.06:0.14: 0000d201`  
`00.06:0.18: fec00000`  
`00.06:0.1c: 00000000`  
`00.06:0.20: 00000000`  
`00.06:0.24: 00000000`  
`00.06:0.28: 00000000`  
`00.06:0.2c: 00000000`  
`00.06:0.30: 00000000`  
`00.06:0.34: 00000044`  
`00.06:0.38: 00000000`  
`00.06:0.3c: 05020106`  
`00.06:0.40: 00000000`  
`00.06:0.44: 00020001`  
`00.06:0.48: 00000000`  
`00.06:0.4c: 01010106`  
`00.06:0.50: 00000000`  
`00.06:0.54: 00000000`  
`00.06:0.58: 00000000`  
`00.06:0.5c: 00000000`  
`00.06:0.60: 00000000`  
`00.06:0.64: 00000000`  
`00.06:0.68: 00000000`  
`00.06:0.6c: 00000000`  
`00.06:0.70: 00000000`  
`00.06:0.74: 00000000`  
`00.06:0.78: 00000000`  
`00.06:0.7c: 00000000`  
`00.06:0.80: 00000000`  
`00.06:0.84: 00000000`  
`00.06:0.88: 00000000`  
`00.06:0.8c: 00000000`  
`00.06:0.90: 00000000`  
`00.06:0.94: 00000000`  
`00.06:0.98: 00000000`  
`00.06:0.9c: 00000000`  
`00.06:0.a0: 00000000`  
`00.06:0.a4: 00000000`  
`00.06:0.a8: 00000000`  
`00.06:0.ac: 00000000`  
`00.06:0.b0: 00000000`  
`00.06:0.b4: 00000000`  
`00.06:0.b8: 00000000`  
`00.06:0.bc: 00000000`  
`00.06:0.c0: 00000000`  
`00.06:0.c4: 00000000`  
`00.06:0.c8: 00000000`  
`00.06:0.cc: 00000000`  
`00.06:0.d0: 00000000`  
`00.06:0.d4: 00000000`  
`00.06:0.d8: 00000000`  
`00.06:0.dc: 00000000`  
`00.06:0.e0: 00000000`  
`00.06:0.e4: 00000000`  
`00.06:0.e8: 00000000`  
`00.06:0.ec: 00000000`  
`00.06:0.f0: 00000000`  
`00.06:0.f4: 00000000`  
`00.06:0.f8: 00000000`  
`00.06:0.fc: 00000000`

`BAR0:     d001 - ffffff01 - 256 bytes`  
`BAR1:     d201 - ffffff81 - 128 bytes`  
`BAR2: fec00000 - fffff000 - 4096 bytes`  
`BAR3:        0 -        0 - 0 bytes`  
`BAR4:        0 -        0 - 0 bytes`  
`BAR5:        0 -        0 - 0 bytes`

### 00.08:0 - PCI-PCI Bridge

`00.08:0.00: 01b810de`  
`00.08:0.04: 00a00000`  
`00.08:0.08: 060400b1`  
`00.08:0.0c: 00010000`  
`00.08:0.10: 00000000`  
`00.08:0.14: 00000000`  
`00.08:0.18: 00000000`  
`00.08:0.1c: 02800000`  
`00.08:0.20: 00000000`  
`00.08:0.24: 00000000`  
`00.08:0.28: 00000000`  
`00.08:0.2c: 00000000`  
`00.08:0.30: 00000000`  
`00.08:0.34: 00000044`  
`00.08:0.38: 00000000`  
`00.08:0.3c: 00000000`  
`00.08:0.40: 00000000`  
`00.08:0.44: 00020001`  
`00.08:0.48: 00000000`  
`00.08:0.4c: 00000b08`  
`00.08:0.50: 00000d0c`  
`00.08:0.54: 00000f0e`  
`00.08:0.58: 00000000`  
`00.08:0.5c: 00000000`  
`00.08:0.60: 00000000`  
`00.08:0.64: 00000000`  
`00.08:0.68: 00000000`  
`00.08:0.6c: 00000000`  
`00.08:0.70: 00000000`  
`00.08:0.74: 00000000`  
`00.08:0.78: 00000000`  
`00.08:0.7c: 00000000`  
`00.08:0.80: 00000000`  
`00.08:0.84: 00000000`  
`00.08:0.88: 00000000`  
`00.08:0.8c: 00000000`  
`00.08:0.90: 00000000`  
`00.08:0.94: 00000000`  
`00.08:0.98: 00000000`  
`00.08:0.9c: 00000000`  
`00.08:0.a0: 00000000`  
`00.08:0.a4: 00000000`  
`00.08:0.a8: 00000000`  
`00.08:0.ac: 00000000`  
`00.08:0.b0: 00000000`  
`00.08:0.b4: 00000000`  
`00.08:0.b8: 00000000`  
`00.08:0.bc: 00000000`  
`00.08:0.c0: 00000000`  
`00.08:0.c4: 00000000`  
`00.08:0.c8: 00000000`  
`00.08:0.cc: 00000000`  
`00.08:0.d0: 00000000`  
`00.08:0.d4: 00000000`  
`00.08:0.d8: 00000000`  
`00.08:0.dc: 00000000`  
`00.08:0.e0: 00000000`  
`00.08:0.e4: 00000000`  
`00.08:0.e8: 00000000`  
`00.08:0.ec: 00000000`  
`00.08:0.f0: 00000000`  
`00.08:0.f4: 00000000`  
`00.08:0.f8: 00000000`  
`00.08:0.fc: 00000000`

### 00.09:0 - IDE Controller

`00.09:0.00: 01bc10de`  
`00.09:0.04: 00b00005`  
`00.09:0.08: 01018ab1`  
`00.09:0.0c: 00000000`  
`00.09:0.10: 00000000`  
`00.09:0.14: 00000000`  
`00.09:0.18: 00000000`  
`00.09:0.1c: 00000000`  
`00.09:0.20: 0000ff61`  
`00.09:0.24: 00000000`  
`00.09:0.28: 00000000`  
`00.09:0.2c: 00000000`  
`00.09:0.30: 00000000`  
`00.09:0.34: 00000044`  
`00.09:0.38: 00000000`  
`00.09:0.3c: 01030000`  
`00.09:0.40: 00000000`  
`00.09:0.44: 00020001`  
`00.09:0.48: 00000000`  
`00.09:0.4c: 00000900`  
`00.09:0.50: 00000002`  
`00.09:0.54: 00000000`  
`00.09:0.58: 20202020`  
`00.09:0.5c: ffff00ff`  
`00.09:0.60: c0c0c0c0`  
`00.09:0.64: 00000000`  
`00.09:0.68: 00000000`  
`00.09:0.6c: 00000000`  
`00.09:0.70: 00000000`  
`00.09:0.74: 00000000`  
`00.09:0.78: 00000000`  
`00.09:0.7c: 00000000`  
`00.09:0.80: 00000000`  
`00.09:0.84: 00000000`  
`00.09:0.88: 00000000`  
`00.09:0.8c: 00000000`  
`00.09:0.90: 00000000`  
`00.09:0.94: 00000000`  
`00.09:0.98: 00000000`  
`00.09:0.9c: 00000000`  
`00.09:0.a0: 00000000`  
`00.09:0.a4: 00000000`  
`00.09:0.a8: 00000000`  
`00.09:0.ac: 00000000`  
`00.09:0.b0: 00000000`  
`00.09:0.b4: 00000000`  
`00.09:0.b8: 00000000`  
`00.09:0.bc: 00000000`  
`00.09:0.c0: 00000000`  
`00.09:0.c4: 00000000`  
`00.09:0.c8: 00000000`  
`00.09:0.cc: 00000000`  
`00.09:0.d0: 00000000`  
`00.09:0.d4: 00000000`  
`00.09:0.d8: 00000000`  
`00.09:0.dc: 00000000`  
`00.09:0.e0: 00000000`  
`00.09:0.e4: 00000000`  
`00.09:0.e8: 00000000`  
`00.09:0.ec: 00000000`  
`00.09:0.f0: 00000000`  
`00.09:0.f4: 00000000`  
`00.09:0.f8: 00000000`  
`00.09:0.fc: 00000000`

`BAR0:        0 -        0 - 0 bytes`  
`BAR1:        0 -        0 - 0 bytes`  
`BAR2:        0 -        0 - 0 bytes`  
`BAR3:        0 -        0 - 0 bytes`  
`BAR4:     ff61 - fffffff1 - 16 bytes`  
`BAR5:        0 -        0 - 0 bytes`

### 00.1e:0 - AGP Bridge

`00.1e:0.00: 01b710de`  
`00.1e:0.04: 02200007`  
`00.1e:0.08: 060400a1`  
`00.1e:0.0c: 5f016df7`  
`00.1e:0.10: bf7e3fff`  
`00.1e:0.14: bbfffbff`  
`00.1e:0.18: 00010100`  
`00.1e:0.1c: 02a000f0`  
`00.1e:0.20: fe70fd00`  
`00.1e:0.24: f3f0f000`  
`00.1e:0.28: ff3fbfff`  
`00.1e:0.2c: afff7fff`  
`00.1e:0.30: 3efff5fd`  
`00.1e:0.34: 00000000`  
`00.1e:0.38: 3bdac4f7`  
`00.1e:0.3c: 0080e7f7`  
`00.1e:0.40: 00000001`  
`00.1e:0.44: 80000000`  
`00.1e:0.48: 00000014`  
`00.1e:0.4c: 00000001`  
`00.1e:0.50: 00000000`  
`00.1e:0.54: 10000000`  
`00.1e:0.58: ffffffff`  
`00.1e:0.5c: ffffffff`  
`00.1e:0.60: ffffffff`  
`00.1e:0.64: ffffffff`  
`00.1e:0.68: ffffffff`  
`00.1e:0.6c: ffffffff`  
`00.1e:0.70: ffffffff`  
`00.1e:0.74: ffffffff`  
`00.1e:0.78: ffffffff`  
`00.1e:0.7c: ffffffff`  
`00.1e:0.80: ffffffff`  
`00.1e:0.84: ffffffff`  
`00.1e:0.88: ffffffff`  
`00.1e:0.8c: ffffffff`  
`00.1e:0.90: ffffffff`  
`00.1e:0.94: ffffffff`  
`00.1e:0.98: ffffffff`  
`00.1e:0.9c: ffffffff`  
`00.1e:0.a0: ffffffff`  
`00.1e:0.a4: ffffffff`  
`00.1e:0.a8: ffffffff`  
`00.1e:0.ac: ffffffff`  
`00.1e:0.b0: ffffffff`  
`00.1e:0.b4: ffffffff`  
`00.1e:0.b8: ffffffff`  
`00.1e:0.bc: ffffffff`  
`00.1e:0.c0: ffffffff`  
`00.1e:0.c4: ffffffff`  
`00.1e:0.c8: ffffffff`  
`00.1e:0.cc: ffffffff`  
`00.1e:0.d0: ffffffff`  
`00.1e:0.d4: ffffffff`  
`00.1e:0.d8: ffffffff`  
`00.1e:0.dc: ffffffff`  
`00.1e:0.e0: ffffffff`  
`00.1e:0.e4: ffffffff`  
`00.1e:0.e8: ffffffff`  
`00.1e:0.ec: ffffffff`  
`00.1e:0.f0: ffffffff`  
`00.1e:0.f4: ffffffff`  
`00.1e:0.f8: ffffffff`  
`00.1e:0.fc: ffffffff`

### 01.00:0 - NV2A GPU

`01.00:0.00: 02a010de`  
`01.00:0.04: 02b00007`  
`01.00:0.08: 030000a1`  
`01.00:0.0c: 0000f800`  
`01.00:0.10: fd000000`  
`01.00:0.14: f0000008`  
`01.00:0.18: 00000008`  
`01.00:0.1c: 00000000`  
`01.00:0.20: 00000000`  
`01.00:0.24: 00000000`  
`01.00:0.28: 00000000`  
`01.00:0.2c: 00000000`  
`01.00:0.30: 00000000`  
`01.00:0.34: 00000060`  
`01.00:0.38: 00000000`  
`01.00:0.3c: 01050103`  
`01.00:0.40: 00000000`  
`01.00:0.44: 00200002`  
`01.00:0.48: 1f000017`  
`01.00:0.4c: 1f000114`  
`01.00:0.50: 00000000`  
`01.00:0.54: 00000001`  
`01.00:0.58: 0023d6ce`  
`01.00:0.5c: 0000000f`  
`01.00:0.60: 00024401`  
`01.00:0.64: 00000000`  
`01.00:0.68: 00000000`  
`01.00:0.6c: 00000000`  
`01.00:0.70: 00000000`  
`01.00:0.74: 00000000`  
`01.00:0.78: 00000000`  
`01.00:0.7c: 00000000`  
`01.00:0.80: 2b16d065`  
`01.00:0.84: 00000000`  
`01.00:0.88: 00000000`  
`01.00:0.8c: 00000000`  
`01.00:0.90: 00000000`  
`01.00:0.94: 00000000`  
`01.00:0.98: 00000000`  
`01.00:0.9c: 00000000`  
`01.00:0.a0: 00000000`  
`01.00:0.a4: 00000000`  
`01.00:0.a8: 00000000`  
`01.00:0.ac: 00000000`  
`01.00:0.b0: 00000000`  
`01.00:0.b4: 00000000`  
`01.00:0.b8: 00000000`  
`01.00:0.bc: 00000000`  
`01.00:0.c0: 00000000`  
`01.00:0.c4: 00000000`  
`01.00:0.c8: 00000000`  
`01.00:0.cc: 00000000`  
`01.00:0.d0: 00000000`  
`01.00:0.d4: 00000000`  
`01.00:0.d8: 00000000`  
`01.00:0.dc: 00000000`  
`01.00:0.e0: 00000000`  
`01.00:0.e4: 00000000`  
`01.00:0.e8: 00000000`  
`01.00:0.ec: 00000000`  
`01.00:0.f0: 00000000`  
`01.00:0.f4: 00000000`  
`01.00:0.f8: 00000000`  
`01.00:0.fc: 00000000`

`BAR0: fd000000 - ff000000 - 16777216 bytes`  
`BAR1: f0000008 - f8000008 - 134217728 bytes`  
`BAR2:        8 - fff80008 - 524288 bytes`  
`BAR3:        0 -        0 - 0 bytes`  
`BAR4:        0 -        0 - 0 bytes`  
`BAR5:        0 -        0 - 0 bytes`