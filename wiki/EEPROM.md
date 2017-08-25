---
title: EEPROM
permalink: wiki/EEPROM/
layout: wiki
---

The Xbox EEPROM is a 256 byte non-volatile storage device which contains
device-specific information. It is connected via I²C and located on
address 0x54. Parts of the EEPROM are encrypted using
[Kernel/XboxEEPROMKey](/wiki/Kernel/XboxEEPROMKey "wikilink").

Chip Models
-----------

| Xbox Model    | Manufacturer | Model      |
|---------------|--------------|------------|
| 1.4 (Others?) | Catalyst     | CAT24WC02J |

Contents
--------

| Start | End  | Notes                                                                            |
|-------|------|----------------------------------------------------------------------------------|
| 0x00  | 0x13 | HMAC\_SHA1 Hash                                                                  |
| 0x14  | 0x1B | RC4 Encrypted Confounder ??                                                      |
| 0x1C  | 0x2B | RC4 Encrypted HDD key                                                            |
| 0x2C  | 0x2F | RC4 Encrypted Region code (0x01 North America, 0x02 Japan, 0x04 Europe)          |
| 0x30  | 0x33 | Checksum2 - Checksum of next 44 bytes                                            |
| 0x34  | 0x3F | Xbox serial number                                                               |
| 0x40  | 0x45 | Ethernet MAC address                                                             |
| 0x46  | 0x47 | Unknown Padding ?                                                                |
| 0x48  | 0x57 | Online Key ?                                                                     |
| 0x58  | 0x5B | -   -   Video Standard 0x00400100 = NTSC, 0x00400200 = NTSC-J, 0x00800300 = PAL  |
| 0x5C  | 0x5F | Unknown Padding ?                                                                |
| 0x60  | 0x63 | Checksum3 - Checksum of the next 156 bytes                                       |
| 0x64  | 0x67 | Zone Bias?                                                                       |
| 0x68  | 0x6B | Standard timezone                                                                |
| 0x6C  | 0x6F | Daylight timezone                                                                |
| 0x70  | 0x77 | Unknown Padding ?                                                                |
| 0x78  | 0x7B | 10-05-00-02 (Month-Day-DayOfWeek-Hour)                                           |
| 0x7C  | 0x7F | 04-01-00-02 (Month-Day-DayOfWeek-Hour)                                           |
| 0x80  | 0x87 | Unknown Padding ?                                                                |
| 0x88  | 0x8B | Standard Bias?                                                                   |
| 0x8C  | 0x8F | Daylight Bias?                                                                   |
| 0x90  | 0x93 | Language ID                                                                      |
| 0x94  | 0x97 | Video Settings                                                                   |
| 0x98  | 0x9B | Audio Settings                                                                   |
| 0x9C  | 0x9F | Parental Control Games (0=MAX rating)                                            |
| 0xA0  | 0xA3 | Parental Control Passcode - 4 button sequence - 7=X, 8=Y, B=LTrigger, C=RTrigger |
| 0xA4  | 0xA7 | Parental Control Movies (0=Max rating)                                           |
| 0xA8  | 0xAB | XBOX Live IP Address..                                                           |
| 0xAC  | 0xAF | XBOX Live DNS Server..                                                           |
| 0xB0  | 0xB3 | XBOX Live Gateway Address..                                                      |
| 0xB4  | 0xB7 | XBOX Live Subnet Mask..                                                          |
| 0xB8  | 0xBB | Other XBLive settings ?                                                          |
| 0xBC  | 0xBF | DVD Playback Kit Zone                                                            |
| 0xC0  | 0xFF | Unknown Codes / History ?                                                        |
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
hardware. You have several options: use an I2C host adapter (see
[here](http://dangerousprototypes.com/blog/bus-pirate-manual/) or
[here](https://www.totalphase.com/products/aardvark-i2cspi/)), build an
[I2C-Serial cable](https://www.youtube.com/watch?v=UcK6nKyKGVQ), or use
a device like a RaspberryPi which has an I2C interface. Connect
SDA/SCL/ground to the LPC pinout on the board. See
[here](https://github.com/grimdoomer/PiPROM) for pinout information.
Then use the corresponding software to read/write the EEPROM.

The HMAC HDD Key
----------------

The HMAC HDD Key is generated out of the first 48 bytes. This section
has been identified clearly.

The Region Code
---------------

This DWORD is encrypted. It corresponds to the region code in the XBE
header:

|            |                     |
|------------|---------------------|
| 1          | North America       |
| 2          | Japan               |
| 4          | Europe/Australia    |
| 0x80000000 | Manufacturing plant |

The Serial Number
-----------------

         1166356 20903
         ||    | |||||__
         ||    | ||||___ factory number
         ||    | |||____
         ||    | ||_____ week of year (starting Mondays)
         ||    | |______ last digit of year
         ||    |________
         ||_____________ number of Xbox within week and factory
         |______________ production line within factory 
       

The MAC address
---------------

This is the MAC address of the Ethernet hardware, which has been [issued
by the
IEEE](https://web.archive.org/web/20100617020733/http://standards.ieee.org/regauth/oui/oui_public.txt).

DVD Region
----------

This DWORD corresponds to the region code for playback of DVD movies:

|      |          |
|------|----------|
| 0x00 | None     |
| 0x01 | Region 1 |
| ...  | ...      |
| 0x06 | Region 6 |

Checksum Algorithm
------------------

Checksum2 and Checksum3 values can be calculated by running the
following code snippet over the area the checksum covers:

     /* The EepromCRC algorithm was obtained from the XKUtils 0.2 source released by
      * TeamAssembly under the GNU GPL.
      * Specifically, from XKCRC.cpp
      *
      * Rewritten to ANSI C by David Pye (dmp@davidmpye.dyndns.org)
      *
      * Thanks! */
     void EepromCRC(unsigned char *crc, unsigned char *data, long dataLen) {
             unsigned char* CRC_Data = (unsigned char *)malloc(dataLen+4);
             int pos=0;
             memset(crc,0x00,4);
     
             memset(CRC_Data,0x00, dataLen+4);
             //Circle shift input data one byte right
             memcpy(CRC_Data + 0x01 , data, dataLen-1);
             memcpy(CRC_Data, data + dataLen-1, 0x01);
     
             for (pos=0; pos&lt;4; ++pos) {
                     unsigned short CRCPosVal = 0xFFFF;
                     unsigned long l;
                     for (l=pos; l&lt;dataLen; l+=4) {
                             CRCPosVal -= *(unsigned short*)(&amp;CRC_Data[l]);
                     }
                     CRCPosVal &amp;= 0xFF00;
                     crc[pos] = (unsigned char) (CRCPosVal &gt;&gt; 8);
             }
             free(CRC_Data);
     }

Further Reading
---------------

-   [Information about EEPROM
    contents](https://web.archive.org/web/20040604013125/http://console-dev.com:80/eeprom.htm)
-   [Read/Write an original Xbox EEPROM chip with a Raspberry
    Pi](https://github.com/grimdoomer/PiPROM)
-   [How To Make Xbox EEPROM Reader / Write
    (Video)](https://www.youtube.com/watch?v=UcK6nKyKGVQ)
-   [How To Extract Xbox EEPROM Easy
    (Video)](https://www.youtube.com/watch?v=uzrljlHDr9w)

