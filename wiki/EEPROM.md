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

<table>
<thead>
<tr class="header">
<th><p>Start</p></th>
<th><p>End</p></th>
<th><p>Notes</p></th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><p>0x00</p></td>
<td><p>0x13</p></td>
<td><p>HMAC_SHA1 Hash</p></td>
</tr>
<tr class="even">
<td><p>0x14</p></td>
<td><p>0x1B</p></td>
<td><p>RC4 Encrypted Confounder ??</p></td>
</tr>
<tr class="odd">
<td><p>0x1C</p></td>
<td><p>0x2B</p></td>
<td><p>RC4 Encrypted HDD key</p></td>
</tr>
<tr class="even">
<td><p>0x2C</p></td>
<td><p>0x2F</p></td>
<td><p>RC4 Encrypted Region code</p>
<ul>
<li>0x01 = North America</li>
<li>0x02 = Japan</li>
<li>0x04 = Europe</li>
</ul></td>
</tr>
<tr class="odd">
<td><p>0x30</p></td>
<td><p>0x33</p></td>
<td><p>Checksum2 - Checksum of next 44 (0x2C) bytes (0x34 - 0x5F)<sup>*</sup></p></td>
</tr>
<tr class="even">
<td><p>0x34</p></td>
<td><p>0x3F</p></td>
<td><p>Xbox serial number - (ASCII chars 0x30 - 0x39 to match each digit in SN)</p></td>
</tr>
<tr class="odd">
<td><p>0x40</p></td>
<td><p>0x45</p></td>
<td><p>Ethernet MAC address</p></td>
</tr>
<tr class="even">
<td><p>0x46</p></td>
<td><p>0x47</p></td>
<td><p>Unknown Padding ?</p></td>
</tr>
<tr class="odd">
<td><p>0x48</p></td>
<td><p>0x57</p></td>
<td><p>Online Key ?</p></td>
</tr>
<tr class="even">
<td><p>0x58</p></td>
<td><p>0x5B</p></td>
<td><p>Video Standard</p>
<ul>
<li>0x00400100 = NTSC-M</li>
<li>0x00400200 = NTSC-J</li>
<li>0x00800300 = PAL</li>
</ul></td>
</tr>
<tr class="odd">
<td><p>0x5C</p></td>
<td><p>0x5F</p></td>
<td><p>Unknown Padding ?</p></td>
</tr>
<tr class="even">
<td><p>0x60</p></td>
<td><p>0x63</p></td>
<td><p>Checksum3 - Checksum of the next 92 (0x5C) bytes (0x64 - 0xBF)<sup>*</sup></p></td>
</tr>
<tr class="odd">
<td><p>0x64</p></td>
<td><p>0x67</p></td>
<td><p>Zone Bias - Offset in # minutes to subtract from GMT time (e.g., for GMT-06 Central; 6hr = 360min = 0x0000168)</p></td>
</tr>
<tr class="even">
<td><p>0x68</p></td>
<td><p>0x6B</p></td>
<td><p>Standard Timezone Name: 4 characters, NULL fill remainder if shorter (e.g., CST\0, ACST)</p></td>
</tr>
<tr class="odd">
<td><p>0x6C</p></td>
<td><p>0x6F</p></td>
<td><p>Daylight Timezone Name: 4 characters, NULL fill remainder if shorter (e.g., CDT\0, ACDT)</p></td>
</tr>
<tr class="even">
<td><p>0x70</p></td>
<td><p>0x77</p></td>
<td><p>Unknown Padding ?</p></td>
</tr>
<tr class="odd">
<td><p>0x78</p></td>
<td><p>0x7B</p></td>
<td><p>Standard Time Starts 10-05-00-02 (Month-Day-DayOfWeek-Hour)</p></td>
</tr>
<tr class="even">
<td><p>0x7C</p></td>
<td><p>0x7F</p></td>
<td><p>Daylight Savings Time Starts 04-01-00-02 (Month-Day-DayOfWeek-Hour)</p></td>
</tr>
<tr class="odd">
<td><p>0x80</p></td>
<td><p>0x87</p></td>
<td><p>Unknown Padding ?</p></td>
</tr>
<tr class="even">
<td><p>0x88</p></td>
<td><p>0x8B</p></td>
<td><p>Standard Timezone Bias; if not DST, 0 (0x00000000) minute time adjust</p></td>
</tr>
<tr class="odd">
<td><p>0x8C</p></td>
<td><p>0x8F</p></td>
<td><p>Daylight Savings Time Bias; if DST, -60 (0xFFFFFFC4) minute time adjust</p></td>
</tr>
<tr class="even">
<td><p>0x90</p></td>
<td><p>0x93</p></td>
<td><p>Language ID</p></td>
</tr>
<tr class="odd">
<td><p>0x94</p></td>
<td><p>0x97</p></td>
<td><p>Video Settings</p>
<p>Offset 0x96:</p>
<ul>
<li>0x??=Normal</li>
<li>0xB0=Widescreen</li>
<li>0xB4=Letterbox</li>
</ul></td>
</tr>
<tr class="even">
<td><p>0x98</p></td>
<td><p>0x9B</p></td>
<td><p>Audio Settings</p></td>
</tr>
<tr class="odd">
<td><p>0x9C</p></td>
<td><p>0x9F</p></td>
<td><p>Games Parental Control (0 = Max rating) 1 byte at offset 0x9C data 4-byte aligned adds the 3 remaining bytes of 0x00</p>
<table>
<thead>
<tr class="header">
<th><p>Xbox Game Rating</p></th>
<th><p>EEPROM Value @ 0x9C</p></th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><p>(RP) Rating Pending (Max)</p></td>
<td><p>0x00</p></td>
</tr>
<tr class="even">
<td><p>(AO) Adults Only</p></td>
<td><p>0x01</p></td>
</tr>
<tr class="odd">
<td><p>(M) Mature</p></td>
<td><p>0x02</p></td>
</tr>
<tr class="even">
<td><p>(T) Teen</p></td>
<td><p>0x03</p></td>
</tr>
<tr class="odd">
<td><p>(E) Everyone</p></td>
<td><p>0x04</p></td>
</tr>
<tr class="even">
<td><p>(K-A) Kids to Adults</p></td>
<td><p>0x05</p></td>
</tr>
<tr class="odd">
<td><p>(EC) Early Childhood</p></td>
<td><p>0x06</p></td>
</tr>
</tbody>
</table></td>
</tr>
<tr class="even">
<td><p>0xA0</p></td>
<td><p>0xA3</p></td>
<td><p>Parental Control Passcode; 4 button sequence (each key stored in a nibble)</p>
<ul>
<li>0x1 =  or </li>
<li>0x2 =  or </li>
<li>0x3 =  or </li>
<li>0x4 =  or </li>
<li>0x7 = </li>
<li>0x8 = </li>
<li>0xB = </li>
<li>0xC = </li>
<li>0 = Disabled</li>
</ul>
<p><code>Note:</code><br />
<code>* EEPROM offset 0xA0: 23 14 00 00 </code><br />
<code>* Little Endian value 0x00001423.</code><br />
<code>* The pass code is D-pad directions up (0x01), right (0x04), down (0x02), left (0x03). </code><br />
<code>* Pass code is only 2 bytes not 4, each button is stored as a nibble in the word.  First button in the most significant nibble and last in the least significant nibble.  </code><br />
<code>* Data in the EEPROM is aligned to double word (4-byte) boundaries. Thus, the two extra bytes at 0xA2 and 0xA3 of 0x00.</code></p></td>
</tr>
<tr class="odd">
<td><p>0xA4</p></td>
<td><p>0xA7</p></td>
<td><p>Movies Parental Control (0 = Max rating) only 1 byte necessary, the 3 remaining bytes for multiple of 4-byte data alignment</p>
<table>
<thead>
<tr class="header">
<th><p>Xbox Movie Rating</p></th>
<th><p>EEPROM Value @ 0xA4</p></th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><p>8 (Max)</p></td>
<td><p>0x00</p></td>
</tr>
<tr class="even">
<td><p>7 (NC-17)</p></td>
<td><p>0x01</p></td>
</tr>
<tr class="odd">
<td><p>6 (R)</p></td>
<td><p>0x02</p></td>
</tr>
<tr class="even">
<td><p>5</p></td>
<td><p>0x03</p></td>
</tr>
<tr class="odd">
<td><p>4 (PG-13)</p></td>
<td><p>0x04</p></td>
</tr>
<tr class="even">
<td><p>3 (PG)</p></td>
<td><p>0x05</p></td>
</tr>
<tr class="odd">
<td><p>2</p></td>
<td><p>0x06</p></td>
</tr>
<tr class="even">
<td><p>1 (G)</p></td>
<td><p>0x07</p></td>
</tr>
</tbody>
</table></td>
</tr>
<tr class="even">
<td><p>0xA8</p></td>
<td><p>0xAB</p></td>
<td><p>XBOX Live IP Address..</p></td>
</tr>
<tr class="odd">
<td><p>0xAC</p></td>
<td><p>0xAF</p></td>
<td><p>XBOX Live DNS Server..</p></td>
</tr>
<tr class="even">
<td><p>0xB0</p></td>
<td><p>0xB3</p></td>
<td><p>XBOX Live Gateway Address..</p></td>
</tr>
<tr class="odd">
<td><p>0xB4</p></td>
<td><p>0xB7</p></td>
<td><p>XBOX Live Subnet Mask..</p></td>
</tr>
<tr class="even">
<td><p>0xB8</p></td>
<td><p>0xBB</p></td>
<td><p>Other XBLive settings ?</p></td>
</tr>
<tr class="odd">
<td><p>0xBC</p></td>
<td><p>0xBF</p></td>
<td><p>DVD Playback Kit Zone</p></td>
</tr>
<tr class="even">
<td><p>0xC0</p></td>
<td><p>0xFF</p></td>
<td><p>Unknown Codes / History ? do not change any values in this region</p></td>
</tr>
<tr class="odd">
</tr>
</tbody>
</table>

Note: Info in above table comes from XKUtils
[XKEEPROM.h](https://svn.exotica.org.uk:8443/xbmc4xbox/tags/3.5.3/xbmc/xbox/XKEEPROM.h).

<sup>\*</sup>Configmagic-FINAL-1.6 uses the wrong size when computing
Checksum2 (40 instead of 44 bytes) and Checksum3 (96 instead of 92
bytes). Checksum2 value computed was correct only because the extra 4
bytes not used in the CRC computation were all 0's which does not change
the CRC value. However, a similiar problem with computation of Checksum3
is present. The CRC computed for v1.6 Xbox's is incorrect as the 4 extra
bytes are not 0's as on earlier versions.

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
       

| 1   | 1   | 6   | 6   | 3   | 5   | 6   |     | 2   | 0   | 9   | 0   | 3   |     |                                                             |
|-----|-----|-----|-----|-----|-----|-----|-----|-----|-----|-----|-----|-----|-----|-------------------------------------------------------------|
| |   | \\  | \_  | \_  | \_  | \_  | /   |     | |   | |   | |   | \\  | \\  | \_  | Factory Number (02=Mexico, 03=Hungary, 05=China, 06=Taiwan) |
| |   |     |     | |   | |   |     |     |     | |   | |   | |   |     |     |     |                                                             |
| |   |     |     | |   | |   |     |     |     | |   | \\  | \\  | \_  | \_  | \_  | week of year (starting Mondays)                             |
| |   |     |     | |   | |   |     |     |     | |   |     |     |     |     |     |                                                             |
| |   |     |     | |   | |   |     |     |     | \\  | \_  | \_  | \_  | \_  | \_  | last digit of year (200Y)                                   |
| |   |     |     | |   | |   |     |     |     |     |     |     |     |     |     |                                                             |
| |   |     |     | \\  | \\  | \_  | \_  | \_  | \_  | \_  | \_  | \_  | \_  | \_  | number of Xbox within week and factory                      |
| |   |     |     |     |     |     |     |     |     |     |     |     |     |     |                                                             |
| \\  | \_  | \_  | \_  | \_  | \_  | \_  | \_  | \_  | \_  | \_  | \_  | \_  | \_  | production line within factory                              |

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

