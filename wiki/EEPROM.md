---
title: EEPROM
permalink: wiki/EEPROM/
layout: wiki
---

The Xbox EEPROM is a 256 byte non-volatile storage device which contains
device-specific information. It is connected via I²C and located on
address 0x54.

The Chihiro EEPROM key is
`7B 35 A8 B7 27 ED 43 7A A0 BA FB 8F A4 38 61 80`

Contents
--------

| Start | End  | Notes                                                                   |
|-------|------|-------------------------------------------------------------------------|
| 0x00  | 0x13 | HMAC\_SHA1 Hash                                                         |
| 0x14  | 0x1B | RC4 Encrypted Confounder ??                                             |
| 0x1C  | 0x2B | RC4 Encrypted HDD key                                                   |
| 0x2C  | 0x2F | RC4 Encrypted Region code (0x01 North America, 0x02 Japan, 0x04 Europe) |
| 0x30  | 0x33 | Checksum of next 44 bytes                                               |
| 0x34  | 0x3F | Xbox serial number                                                      |
| 0x40  | 0x45 | Ethernet MAC address                                                    |
| 0x46  | 0x47 | Unknown Padding ?                                                       |
| 0x48  | 0x57 | Online Key ?                                                            |
| 0x58  | 0x5B | -   -   0x00014000 = NTSC, 0x00038000 = PAL                             |
| 0x5C  | 0x5F | Unknown Padding ?                                                       |
| 0x60  | 0x63 | other Checksum of next                                                  |
| 0x64  | 0x67 | Zone Bias?                                                              |
| 0x68  | 0x6B | Standard timezone                                                       |
| 0x5C  | 0x6F | Daylight timezone                                                       |
| 0x70  | 0x77 | Unknown Padding ?                                                       |
| 0x78  | 0x7B | 10-05-00-02 (Month-Day-DayOfWeek-Hour)                                  |
| 0x7C  | 0x7F | 04-01-00-02 (Month-Day-DayOfWeek-Hour)                                  |
| 0x80  | 0x87 | Unknown Padding ?                                                       |
| 0x88  | 0x8B | Standard Bias?                                                          |
| 0x8C  | 0x8F | Daylight Bias?                                                          |
| 0x90  | 0x93 | Language ID                                                             |
| 0x94  | 0x97 | Video Settings                                                          |
| 0x98  | 0x9B | Audio Settings                                                          |
| 0x9C  | 0x9F | 0=MAX rating                                                            |
| 0xA0  | 0xA3 | 7=X, 8=Y, B=LTrigger, C=RTrigger                                        |
| 0xA4  | 0xA7 | 0=Max rating                                                            |
| 0xA8  | 0xAB | XBOX Live IP Address..                                                  |
| 0xAC  | 0xAF | XBOX Live DNS Server..                                                  |
| 0xB0  | 0xB3 | XBOX Live Gateway Address..                                             |
| 0xB4  | 0xB7 | XBOX Live Subnet Mask..                                                 |
| 0xA8  | 0xBB | Other XBLive settings ?                                                 |
| 0xBC  | 0xBF | DVD Playback Kit Zone                                                   |
| 0xC0  | 0xFF | Unknown Codes / History ?                                               |
||

Note: Info in above table comes from XKUtils
[XKEEPROM.h](https://svn.exotica.org.uk:8443/xbmc4xbox/tags/3.5.3/xbmc/xbox/XKEEPROM.h).

Reading/Writing the EEPROM
--------------------------

### Software Method

This is the easiest way to dump an Xbox EEPROM. Use your alternative
dashboard to dump the EEPROM to a file and download it over FTP.

### Hardware Method

If you cannot dump the EEPROM using software, you can dump it using
hardware. You have several options: use an [I2C/SPI USB host
adapter](https://www.totalphase.com/products/aardvark-i2cspi/), build an
[I2C-Serial cable](https://www.youtube.com/watch?v=UcK6nKyKGVQ), or use
a device like a RaspberryPi which has an I2C interface already. Connect
SDA/SCL/ground to the LPC pinout on the board. See
[here](https://github.com/grimdoomer/PiPROM) for pinout information.

Further Reading
---------------

-   [Read/Write an original Xbox EEPROM chip with a Raspberry
    Pi](https://github.com/grimdoomer/PiPROM)
-   [How To Make Xbox EEPROM Reader / Write
    (Video)](https://www.youtube.com/watch?v=UcK6nKyKGVQ)
-   [How To Extract Xbox EEPROM Easy
    (Video)](https://www.youtube.com/watch?v=uzrljlHDr9w)

