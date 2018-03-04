---
title: EEPROM
permalink: wiki/EEPROM/
layout: wiki
---

The Xbox EEPROM is a 256 byte non-volatile storage device which contains
device-specific information. It is connected via I²C and located on
address 0x54. Parts of the EEPROM are encrypted using
[Kernel/XboxEEPROMKey](/wiki/Kernel/XboxEEPROMKey "wikilink").

Contents
--------

| Start | End  | Notes                                                                                                                                                                     |
|-------|------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| 0x00  | 0x13 | HMAC\_SHA1 Hash                                                                                                                                                           |
| 0x14  | 0x1B | RC4 Encrypted Confounder ??                                                                                                                                               |
| 0x1C  | 0x2B | RC4 Encrypted HDD key                                                                                                                                                     |
| 0x2C  | 0x2F | RC4 Encrypted Region code                                                                                                                                                 
                                                                                                                                                                              
   -   0x01 = North America                                                                                                                                                   
   -   0x02 = Japan                                                                                                                                                           
   -   0x04 = Europe                                                                                                                                                          |
| 0x30  | 0x33 | Checksum2 - Checksum of next 44 (0x2C) bytes (0x34 - 0x5F)<sup>\*</sup>                                                                                                   |
| 0x34  | 0x3F | Xbox serial number - (ASCII chars 0x30 - 0x39 to match each digit in SN)                                                                                                  |
| 0x40  | 0x45 | Ethernet MAC address                                                                                                                                                      |
| 0x46  | 0x47 | Unknown Padding ?                                                                                                                                                         |
| 0x48  | 0x57 | Online Key ?                                                                                                                                                              |
| 0x58  | 0x5B | Video Standard                                                                                                                                                            
                                                                                                                                                                              
   -   0x00400100 = NTSC-M                                                                                                                                                    
   -   0x00400200 = NTSC-J                                                                                                                                                    
   -   0x00800300 = PAL                                                                                                                                                       |
| 0x5C  | 0x5F | Unknown Padding ?                                                                                                                                                         |
| 0x60  | 0x63 | Checksum3 - Checksum of the next 92 (0x5C) bytes (0x64 - 0xBF)<sup>\*</sup>                                                                                               |
| 0x64  | 0x67 | Zone Bias - Offset in \# minutes to subtract from GMT time (e.g., for GMT-06 Central; 6hr = 360min = 0x00000168)                                                          |
| 0x68  | 0x6B | Standard Timezone Name: 4 characters, NULL fill remainder if shorter (e.g., CST\\0, ACST)                                                                                 |
| 0x6C  | 0x6F | Daylight Timezone Name: 4 characters, NULL fill remainder if shorter (e.g., CDT\\0, ACDT)                                                                                 |
| 0x70  | 0x77 | Unknown Padding ?                                                                                                                                                         |
| 0x78  | 0x7B | Standard Time Starts 10-05-00-02 (Month-Day-DayOfWeek-Hour)                                                                                                               |
| 0x7C  | 0x7F | Daylight Savings Time Starts 04-01-00-02 (Month-Day-DayOfWeek-Hour)                                                                                                       |
| 0x80  | 0x87 | Unknown Padding ?                                                                                                                                                         |
| 0x88  | 0x8B | Standard Timezone Bias; if not DST, 0 (0x00000000) minute time adjust                                                                                                     |
| 0x8C  | 0x8F | Daylight Savings Time Bias; if DST, -60 (0xFFFFFFC4) minute time adjust                                                                                                   |
| 0x90  | 0x93 | Language ID                                                                                                                                                               |
| 0x94  | 0x97 | Video Settings                                                                                                                                                            
                                                                                                                                                                              
   Offset 0x96:                                                                                                                                                               
                                                                                                                                                                              
   -   0x??=Normal                                                                                                                                                            
   -   0xB0=Widescreen                                                                                                                                                        
   -   0xB4=Letterbox                                                                                                                                                         |
| 0x98  | 0x9B | Audio Settings                                                                                                                                                            |
| 0x9C  | 0x9F | Games Parental Control (0 = Max rating) 1 byte at offset 0x9C data 4-byte aligned adds the 3 remaining bytes of 0x00                                                      
                                                                                                                                                                              
   | Xbox Game Rating          | EEPROM Value @ 0x9C |                                                                                                                        
   |---------------------------|---------------------|                                                                                                                        
   | (RP) Rating Pending (Max) | 0x00                |                                                                                                                        
   | (AO) Adults Only          | 0x01                |                                                                                                                        
   | (M) Mature                | 0x02                |                                                                                                                        
   | (T) Teen                  | 0x03                |                                                                                                                        
   | (E) Everyone              | 0x04                |                                                                                                                        
   | (K-A) Kids to Adults      | 0x05                |                                                                                                                        
   | (EC) Early Childhood      | 0x06                |                                                                                                                        |
| 0xA0  | 0xA3 | Parental Control Passcode; 4 button sequence (each key stored in a nibble)                                                                                                
                                                                                                                                                                              
   -   0x1 = or                                                                                                                                                               
   -   0x2 = or                                                                                                                                                               
   -   0x3 = or                                                                                                                                                               
   -   0x4 = or                                                                                                                                                               
   -   0x7 =                                                                                                                                                                  
   -   0x8 =                                                                                                                                                                  
   -   0xB =                                                                                                                                                                  
   -   0xC =                                                                                                                                                                  
   -   0 = Disabled                                                                                                                                                           
                                                                                                                                                                              
   Note:                                                                                                                                                                      
                                                                                                                                                                              
   -   EEPROM offset 0xA0: `23 14 00 00`                                                                                                                                      
   -   Little Endian value 0x00001423.                                                                                                                                        
   -   The pass code is D-pad directions up (0x1), right (0x4), down (0x2), left (0x3).                                                                                       
   -   Pass code is only 2 bytes not 4, each button is stored as a nibble in the word. First button in the most significant nibble and last in the least significant nibble.  
   -   Data in the EEPROM is aligned to double word (4-byte) boundaries. Thus, the two extra bytes at 0xA2 and 0xA3 of 0x00.                                                  |
| 0xA4  | 0xA7 | Movies Parental Control (0 = Max rating) only 1 byte necessary, the 3 remaining bytes for multiple of 4-byte data alignment                                               
                                                                                                                                                                              
   | Xbox Movie Rating | EEPROM Value @ 0xA4 |                                                                                                                                
   |-------------------|---------------------|                                                                                                                                
   | 8 (Max)           | 0x00                |                                                                                                                                
   | 7 (NC-17)         | 0x01                |                                                                                                                                
   | 6 (R)             | 0x02                |                                                                                                                                
   | 5                 | 0x03                |                                                                                                                                
   | 4 (PG-13)         | 0x04                |                                                                                                                                
   | 3 (PG)            | 0x05                |                                                                                                                                
   | 2                 | 0x06                |                                                                                                                                
   | 1 (G)             | 0x07                |                                                                                                                                |
| 0xA8  | 0xAB | XBOX Live IP Address..                                                                                                                                                    |
| 0xAC  | 0xAF | XBOX Live DNS Server..                                                                                                                                                    |
| 0xB0  | 0xB3 | XBOX Live Gateway Address..                                                                                                                                               |
| 0xB4  | 0xB7 | XBOX Live Subnet Mask..                                                                                                                                                   |
| 0xB8  | 0xBB | Other XBLive settings ?                                                                                                                                                   |
| 0xBC  | 0xBF | DVD Playback Kit Zone                                                                                                                                                     |
| 0xC0  | 0xFF | Unknown Codes / History ? do not change any values in this region                                                                                                         |
||

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
| 0x00000001 | North America       |
| 0x00000002 | Japan               |
| 0x00000004 | Europe / Australia  |
| 0x80000000 | Manufacturing plant |

The MAC address
---------------

This is the MAC address of the Ethernet hardware, which has been [issued
by the
IEEE](https://web.archive.org/web/20100617020733/http://standards.ieee.org/regauth/oui/oui_public.txt).

DVD Region
----------

This DWORD corresponds to the region code for playback of DVD movies:

|            |          |
|------------|----------|
| 0x00000000 | None     |
| 0x00000001 | Region 1 |
| ...        | ...      |
| 0x00000006 | Region 6 |

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

Read Checksum Algorithm
-----------------------

When the Xbox reads from the FACTORY\_SETTINGS or the USER\_SETTINGS
section of the EEPROM, this algorithm is ran over the entire section
accessed (including the CRC checksum mentioned above) to ensure that the
data is valid. If the result of the checksum algorithm does not equal
0xFFFFFFFF, STATUS\_DEVICE\_DATA\_ERROR is returned from the Kernel.

    static uint32_t eeprom_section_checksum(
        const uint32_t* section_data,
        uint32_t section_data_length
    )
    {
        const uint32_t bitmask = 0xFFFFFFFF;
        const uint32_t num_dwords = section_data_length >> 2;
        uint64_t checksum = 0;
        uint32_t carry_count = 0;

        for(uint32_t loop_count = num_dwords; loop_count > 0; loop_count--) {
            checksum += *section_data;
            if(checksum > bitmask) {
                carry_count++;
                checksum &= bitmask;
            }
            section_data++;
        }
        checksum += carry_count;
        if(checksum > bitmask) {
            checksum += 1;
        }
        return (uint32_t)(checksum & bitmask);
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

