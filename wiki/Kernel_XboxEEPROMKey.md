---
title: Kernel/XboxEEPROMKey
permalink: wiki/Kernel/XboxEEPROMKey/
layout: wiki
---

The 16 byte RC4 key used to decrypt parts of the
[EEPROM](/wiki/EEPROM "wikilink").

The key is passed to kernel from from 2BL. It is only initialized on
cold-boot.

If an XBE is loaded which is not allowed to run in the manufacturing
region, the Microsoft Kernels will set the EEPROM key to all zeros. As
the key is not re-initialized, this means running the first
non-manufacturing XBE (such as a game or
[Dashboard](/wiki/Dashboard "wikilink")), will make the key unretrievable
until a reboot.

The Chihiro EEPROM key is
`7B 35 A8 B7 27 ED 43 7A A0 BA FB 8F A4 38 61 80`
