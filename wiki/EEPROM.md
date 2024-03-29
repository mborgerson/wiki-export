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

| Start | End  | Notes                                                                                                                                                                                                                                             |
|-------|------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| 0x00  | 0x13 | HMAC\_SHA1 Hash                                                                                                                                                                                                                                   |
| 0x14  | 0x1B | RC4 Encrypted Confounder ??                                                                                                                                                                                                                       |
| 0x1C  | 0x2B | RC4 Encrypted HDD key                                                                                                                                                                                                                             |
| 0x2C  | 0x2F | RC4 Encrypted Region code                                                                                                                                                                                                                         
                                                                                                                                                                                                                                                      
   -   0x00000001 = North America                                                                                                                                                                                                                     
   -   0x00000002 = Japan                                                                                                                                                                                                                             
   -   0x00000004 = Europe & Australia                                                                                                                                                                                                                
   -   0x80000000 = Manufacturing plant                                                                                                                                                                                                               |
| 0x30  | 0x33 | Checksum2 - Checksum of next 44 (0x2C) bytes (0x34 - 0x5F)<sup>\*</sup>                                                                                                                                                                           |
| 0x34  | 0x3F | Xbox serial number - (ASCII chars 0x30 - 0x39 to match each digit in SN)                                                                                                                                                                          |
| 0x40  | 0x45 | Ethernet MAC address (Microsoft Xbox - 00:50:F2:xx:xx:xx) This is the MAC address of the Ethernet hardware, which has been [issued by the IEEE](https://web.archive.org/web/20100617020733/http://standards.ieee.org/regauth/oui/oui_public.txt). |
| 0x46  | 0x47 | Unknown Padding ?                                                                                                                                                                                                                                 |
| 0x48  | 0x57 | Online Key ?                                                                                                                                                                                                                                      |
| 0x58  | 0x5B | Video Standard                                                                                                                                                                                                                                    
                                                                                                                                                                                                                                                      
   -   0x00000000 = not set (INVALID)                                                                                                                                                                                                                 
   -   0x00400100 = NTSC-M                                                                                                                                                                                                                            
   -   0x00400200 = NTSC-J                                                                                                                                                                                                                            
   -   0x00800300 = PAL-I                                                                                                                                                                                                                             
   -   0x00400400 = PAL-M                                                                                                                                                                                                                             |
| 0x5C  | 0x5F | Unknown Padding ?                                                                                                                                                                                                                                 |
| 0x60  | 0x63 | Checksum3 - Checksum of the next 92 (0x5C) bytes (0x64 - 0xBF)<sup>\*</sup>                                                                                                                                                                       |
| 0x64  | 0x67 | Zone Bias - Offset in \# minutes to subtract from GMT time (e.g., for GMT-06 Central; 6hr = 360min = 0x00000168)                                                                                                                                  |
| 0x68  | 0x6B | Standard Timezone Name: 4 characters, NULL fill remainder if shorter (e.g., CST\\0, ACST)                                                                                                                                                         |
| 0x6C  | 0x6F | Daylight Timezone Name: 4 characters, NULL fill remainder if shorter (e.g., CDT\\0, ACDT)                                                                                                                                                         |
| 0x70  | 0x77 | Unknown Padding ?                                                                                                                                                                                                                                 |
| 0x78  | 0x7B | Standard Time Starts 10-05-00-02 (Month-Day-DayOfWeek-Hour)                                                                                                                                                                                       |
| 0x7C  | 0x7F | Daylight Savings Time Starts 04-01-00-02 (Month-Day-DayOfWeek-Hour)                                                                                                                                                                               |
| 0x80  | 0x87 | Unknown Padding ?                                                                                                                                                                                                                                 |
| 0x88  | 0x8B | Standard Timezone Bias; if not DST, 0 (0x00000000) minute time adjust                                                                                                                                                                             |
| 0x8C  | 0x8F | Daylight Savings Time Bias; if DST, -60 (0xFFFFFFC4) minute time adjust                                                                                                                                                                           |
| 0x90  | 0x93 | Language ID (0 = not set)                                                                                                                                                                                                                         
                                                                                                                                                                                                                                                      
   -   0x00000001 = English                                                                                                                                                                                                                           
   -   0x00000002 = Japanese                                                                                                                                                                                                                          
   -   0x00000003 = German                                                                                                                                                                                                                            
   -   0x00000004 = French                                                                                                                                                                                                                            
   -   0x00000005 = Spanish                                                                                                                                                                                                                           
   -   0x00000006 = Italian                                                                                                                                                                                                                           
   -   0x00000007 = Korean                                                                                                                                                                                                                            
   -   0x00000008 = Chinese                                                                                                                                                                                                                           
   -   0x00000009 = Portuguese                                                                                                                                                                                                                        |
| 0x94  | 0x97 | Video Settings                                                                                                                                                                                                                                    
                                                                                                                                                                                                                                                      
   -   0x00080000 = 480p                                                                                                                                                                                                                              
   -   0x00020000 = 720p                                                                                                                                                                                                                              
   -   0x00040000 = 1080i                                                                                                                                                                                                                             
                                                                                                                                                                                                                                                      
   <!-- -->                                                                                                                                                                                                                                           
                                                                                                                                                                                                                                                      
   -   0x00010000 = Widescreen                                                                                                                                                                                                                        
   -   0x00100000 = Letterbox                                                                                                                                                                                                                         
                                                                                                                                                                                                                                                      
   <!-- -->                                                                                                                                                                                                                                           
                                                                                                                                                                                                                                                      
   -   0x00400000 = 60Hz                                                                                                                                                                                                                              
   -   0x00800000 = 50Hz                                                                                                                                                                                                                              |
| 0x98  | 0x9B | Audio Settings                                                                                                                                                                                                                                    
                                                                                                                                                                                                                                                      
   -   0x00000000 = Stereo                                                                                                                                                                                                                            
   -   0x00000001 = Mono                                                                                                                                                                                                                              
   -   0x00000002 = Surround                                                                                                                                                                                                                          
   -   0x00010000 = Enable AC3                                                                                                                                                                                                                        
   -   0x00020000 = Enable DTS                                                                                                                                                                                                                        |
| 0x9C  | 0x9F | Games Parental Control (0 = Max rating)                                                                                                                                                                                                           
                                                                                                                                                                                                                                                      
   -   0x00000000 = Rating Pending (RP)                                                                                                                                                                                                               
   -   0x00000001 = Adults Only (AO)                                                                                                                                                                                                                  
   -   0x00000002 = Mature (M)                                                                                                                                                                                                                        
   -   0x00000003 = Teen (T)                                                                                                                                                                                                                          
   -   0x00000004 = Everyone (E)                                                                                                                                                                                                                      
   -   0x00000005 = Kids to Adults (K-A)                                                                                                                                                                                                              
   -   0x00000006 = Early Childhood (EC)                                                                                                                                                                                                              |
| 0xA0  | 0xA3 | Parental Control Passcode; 4 button sequence (each key stored in a nibble)                                                                                                                                                                        
                                                                                                                                                                                                                                                      
   -   0x1 = or                                                                                                                                                                                                                                       
   -   0x2 = or                                                                                                                                                                                                                                       
   -   0x3 = or                                                                                                                                                                                                                                       
   -   0x4 = or                                                                                                                                                                                                                                       
   -   0x5 =                                                                                                                                                                                                                                          
   -   0x6 =                                                                                                                                                                                                                                          
   -   0x7 =                                                                                                                                                                                                                                          
   -   0x8 =                                                                                                                                                                                                                                          
   -   0xB =                                                                                                                                                                                                                                          
   -   0xC =                                                                                                                                                                                                                                          
   -   0x0 = Disabled                                                                                                                                                                                                                                 
                                                                                                                                                                                                                                                      
   *Note*:                                                                                                                                                                                                                                            
                                                                                                                                                                                                                                                      
   -   A passcode 0x00001423 is D-pad directions up (0x1), right (0x4), down (0x2), left (0x3).                                                                                                                                                       
   -   Pass code only uses the lower 16 bits; each button is stored as a nibble in the word. First button in the most significant nibble and last in the least significant nibble.                                                                    |
| 0xA4  | 0xA7 | Movies Parental Control (0 = Max rating)                                                                                                                                                                                                          
                                                                                                                                                                                                                                                      
   -   0x00000001 = Adults Only (NC-17)                                                                                                                                                                                                               
   -   0x00000002 = Restricted (R)                                                                                                                                                                                                                    
   -   0x00000004 = Parents Strongly Cautioned (PG-13)                                                                                                                                                                                                
   -   0x00000005 = Parental Guidance Suggested (PG)                                                                                                                                                                                                  
   -   0x00000007 = General Audiences (G)                                                                                                                                                                                                             |
| 0xA8  | 0xAB | XBOX Live IP Address..                                                                                                                                                                                                                            |
| 0xAC  | 0xAF | XBOX Live DNS Server..                                                                                                                                                                                                                            |
| 0xB0  | 0xB3 | XBOX Live Gateway Address..                                                                                                                                                                                                                       |
| 0xB4  | 0xB7 | XBOX Live Subnet Mask..                                                                                                                                                                                                                           |
| 0xB8  | 0xBB | Other XBLive settings ?                                                                                                                                                                                                                           |
| 0xBC  | 0xBF | DVD Playback Kit Zone                                                                                                                                                                                                                             
                                                                                                                                                                                                                                                      
   -   0x00000000 = None                                                                                                                                                                                                                              
   -   0x00000001 = Region 1                                                                                                                                                                                                                          
   -   ...                                                                                                                                                                                                                                            
   -   0x00000006 = Region 6                                                                                                                                                                                                                          |
| 0xC0  | 0xFF | Unknown Codes / History ? do not change any values in this range                                                                                                                                                                                  |
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
      * Adapted for XboxDevWiki
      */
     uint32_t EepromCRC(unsigned char *data, long dataLen) {

             // Initialize result to zero
             uint8_t crc[4] = { 0x00, 0x00, 0x00, 0x00 };
     
             //Circle shift input data one byte right
             unsigned char* CRC_Data = (unsigned char *)malloc(dataLen + 4);
             memset(CRC_Data, 0x00, dataLen + 4);
             memcpy(CRC_Data + 0x01 , data, dataLen - 1);
             memcpy(CRC_Data, data + dataLen - 1, 0x01);
     
             // Calculate checksum
             for (unsigned int i = 0; i < 4; i++) {
                     unsigned short CRCPosVal = 0xFFFF;
                     for (unsigned long l = i; l < dataLen; l += 4) {
                             CRCPosVal -= *(unsigned short*)(&amp;CRC_Data[l]);
                     }
                     CRCPosVal &= 0xFF00;
                     crc[i] = (unsigned char) (CRCPosVal << 8);
             }

             free(CRC_Data);

             return *(uint32_t*)&checksum;
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
        uint64_t checksum = 0;
        uint32_t carry_count = 0;

        // Process the data in 32 bit steps
        for(unsigned int i = 0; i < section_data_length / 4; i++) {
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

