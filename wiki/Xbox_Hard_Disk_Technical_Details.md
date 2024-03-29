---
title: Xbox Hard Disk Technical Details
permalink: wiki/Xbox_Hard_Disk_Technical_Details/
layout: wiki
---

by Michael Steil (original version: 1 May 2002, updated 26 September
2002)

Seagate (10 GB)
---------------

### Harddisk information as reported by hdparm -i

    Model=ST310211A, FwRev=6.55, SerialNo=6DB2WQW2
    Config={ HardSect NotMFM HdSw&gt;15uSec Fixed DTR&gt;10Mbs RotSpdTol&gt;.5% }
    RawCHS=16383/16/63, TrkSize=0, SectSize=0, ECCbytes=0
    BuffType=unknown, BuffSize=512kB, MaxMultSect=16, MultSect=16
    CurCHS=16383/16/63, CurSects=16514064, LBA=yes, LBAsects=19541088
    IORDY=on/off, tPIO={min:240,w/IORDY:120}, tDMA={min:120,rec:120}
    PIO modes: pio0 pio1 pio2 pio3 pio4 
    DMA modes: mdma0 mdma1 mdma2 udma0 udma1 *udma2 
    AdvancedPM=no WriteCache=enabled
    Drive Supports : Reserved : ATA-1 ATA-2 ATA-3 ATA-4 

### Detailed information as reported by hdparm -I

    non-removable ATA device, with non-removable media
            Model Number:           ST310211A                          
        
            Serial Number:          6DB2WQW2            
            Firmware Revision:      6.55    
    Standards:
            Supported: 1 2 3 4 
            Likely used: 5
    Configuration:
            Logical         max     current
            cylinders       16383   16383
            heads           16      16
            sectors/track   63      63
            bytes/track:    0               (obsolete)
            bytes/sector:   0               (obsolete)
            current sector capacity: 16514064
            LBA user addressable sectors = 19541088
    Capabilities:
            LBA, IORDY(can be disabled)
            Buffer size: 512.0kB    Queue depth: 1
            Standby timer values: spec'd by standard
            r/w multiple sector transfer: Max = 16  Current = 16
            DMA: mdma0 mdma1 mdma2 udma0 udma1 *udma2 udma3 udma4 udma5

                 Cycle time: min=120ns recommended=120ns
            PIO: pio0 pio1 pio2 pio3 pio4 
                 Cycle time: no flow control=240ns  IORDY flow control=120ns
    Commands/features:
            Enabled Supported:
               *    READ BUFFER cmd
               *    WRITE BUFFER cmd
               *    Host Protected Area feature set
               *    look-ahead
               *    write cache
               *    Power Management feature set
               *    Security Mode feature set
                    SMART feature set
                    SET MAX security extension
               *    DOWNLOAD MICROCODE cmd
    Security: 
                    supported
                    enabled
            not     locked
            not     frozen
            not     expired: security count
            not     supported: enhanced erase
            Security level high
    HW reset results:
            CBLID- below Vih
            Device num = 1
    Checksum: correct

### Hard disk performance measured by hdparm -t

    Timing buffered disk reads:  64 MB in  3.51 seconds = 18.23 MB/sec
    Timing buffer-cache reads:   128 MB in  0.85 seconds =150.59 MB/sec

Western Digital (8 GB)
----------------------

### Harddisk information as reported by hdparm -i

    Model=WDC WD80EB-28CGH1, FwRev=24.84G24, SerialNo=***************
    Config={ HardSect NotMFM HdSw&gt;15uSec SpinMotCtl Fixed DTR&gt;5Mbs FmtGapReq }
    RawCHS=15509/16/63, TrkSize=57600, SectSize=600, ECCbytes=40
    BuffType=DualPortCache, BuffSize=768kB, MaxMultSect=16, MultSect=off
    CurCHS=15509/16/63, CurSects=15633072, LBA=yes, LBAsects=15633072
    IORDY=on/off, tPIO={min:120,w/IORDY:120}, tDMA={min:120,rec:120}
    PIO modes:  pio0 pio1 pio2 pio3 pio4 
    DMA modes:  mdma0 mdma1 *mdma2 
    UDMA modes: udma0 udma1 udma2 udma3 udma4 udma5 
    AdvancedPM=no WriteCache=enabled
    Drive conforms to: device does not report version:  1 2 3 4 5

### Detailed information as reported by hdparm -I

    ATA device, with non-removable media
            Model Number:       WDC WD80EB-28CGH1                      

            Serial Number:      *************** 
            Firmware Revision:  24.84G24
    Standards:
            Supported: 5 4 3 2 
            Likely used: 6
    Configuration:
            Logical         max     current
            cylinders       15509   15509
            heads           16      16
            sectors/track   63      63
            --
            CHS current addressable sectors:   15633072
            LBA    user addressable sectors:   15633072
            device size with M = 1024*1024:        7633 MBytes
            device size with M = 1000*1000:        8004 MBytes (8 GB)
    Capabilities:
            LBA, IORDY(can be disabled)
            bytes avail on r/w long: 40     Queue depth: 1
            Standby timer values: spec'd by Standard, with device specific minimum
            R/W multiple sector transfer: Max = 16  Current = 0
            Recommended acoustic management value: 128, current value: 254
            DMA: mdma0 mdma1 *mdma2 udma0 udma1 udma2 udma3 udma4 udma5

                 Cycle time: min=120ns recommended=120ns
            PIO: pio0 pio1 pio2 pio3 pio4 
                 Cycle time: no flow control=120ns  IORDY flow control=120ns
    Commands/features:
            Enabled Supported:
               *    READ BUFFER cmd
               *    WRITE BUFFER cmd
               *    Host Protected Area feature set
               *    Look-ahead
               *    Write cache
               *    Power Management feature set
               *    Security Mode feature set
               *    SMART feature set
                    Automatic Acoustic Management feature set 
                    SET MAX security extension
               *    DOWNLOAD MICROCODE cmd
    Security: 
                    supported
                    enabled
            not     locked
            not     frozen
            not     expired: security count
            not     supported: enhanced erase
            Security level high
    HW reset results:
            CBLID- above Vih
            Device num = 0 determined by CSEL
    Checksum: correct

### Hard disk performance measured by hdparm -t

    Timing buffered disk reads:  64 MB in  6.58 seconds =  9.73 MB/sec
    Timing buffer-cache reads:   128 MB in  3.28 seconds = 39.02 MB/sec
