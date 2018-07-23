---
title: DSP
permalink: wiki/DSP/
layout: wiki
---

The DSPs in the APU are probably “Parthus MediaStream” DSP core
(DSP24210/DSP2420?).

Those are similar to Motorola DSP56362 (DSP56300 Family). If so, the
datasheet can be found at
<http://www.nxp.com/docs/en/data-sheet/DSP56362.pdf> (Also see
“Documentation” section in said datasheet for the related documentation)

Memory Size
-----------

|     | Program RAM Size | X Data RAM Size | Y Data RAM Size |
|-----|------------------|-----------------|-----------------|
| GP  | 4096 x 24-bit    | 4096 x 24-bit   | 2048 x 24-bit   |
| EP  | 4096 x 24-bit    | 3072 x 24-bit   | 256 x 24-bit    |

Other datasheets for similar DSPs suggest that the memory sizes might be
different if instruction cache or switch mode are toggled. It is
currently unknown if the DSPs in the Xbox APU support a similar feature.

DMA
---

*This section is very incomplete and not much was tested on hardware
either*

DMA is controlled using peripheral registers:

-   0xFFFFD4: Memory address of next command block
-   0xFFFFD5: DMA\_START\_BLOCK
-   0xFFFFD6: DMA\_CONTROL
-   0xFFFFD7: DMA\_CONFIGURATION

Additionally, bit 7 in the interrupt register at 0xFFFFC5 is set if a
DMA End-Of-List has been encountered.

### Command blocks

DSP command blocks are loaded from X-Memory.

| Word | Meaning                    | Notes                                                                       |
|------|----------------------------|-----------------------------------------------------------------------------|
| 0    | Next command block address | Memory address of next command block. Bit 14 is used as End-Of-List marker. |
| 1    | Transfer control word      | Controls the DMA transfer:                                                  
                                                                                
   -   Direction (Bit-offset 1; 1-bit).                                         
       -   0 = Buffer to DSP                                                    
       -   1 = DSP to buffer                                                    
                                                                                
   <!-- -->                                                                     
                                                                                
   -   Buffer (Bit-offset 5; 4-bits).                                           
       -   0x0 = FIFO0                                                          
       -   0x1 = FIFO1                                                          
       -   0x2 = FIFO2                                                          
       -   0x3 = FIFO3                                                          
       -   0x4                                                                  
       -   0x5                                                                  
       -   0x6                                                                  
       -   0x7                                                                  
       -   0x8                                                                  
       -   0x9                                                                  
       -   0xA                                                                  
       -   0xB                                                                  
       -   0xC                                                                  
       -   0xD                                                                  
       -   0xE = Circular                                                       
       -   0xF                                                                  
                                                                                
   <!-- -->                                                                     
                                                                                
   -   Sample format (Bit-offset 10; 3-bits).                                   
       -   0x0 = 8 bit                                                          
       -   0x1 = 16 bit                                                         
       -   0x2 = 24 bit in MSB                                                  
       -   0x3 = 32 bit                                                         
       -   0x4                                                                  
       -   0x5                                                                  
       -   0x6 = 24 bit in LSB (also endianess switched?)                       
       -   0x7                                                                  |
| 2    | Transfer sample count      | The number of samples to transfer.                                          |
| 3    | DSP address                | This is the address in the DSP:                                             
                                                                                
   -   0x0000 - 0x17FF = X-Memory                                               
   -   0x1800 - 0x27FF = Y-Memory                                               
   -   0x2800 - 0x37FF = P-Memory                                               |
| 4    | Buffer offset              | This is the address within the buffer where the first sample is accessed.   |
| 5    | Buffer base                | The start of the buffer.                                                    |
| 6    | Buffer limit               | The end of the buffer. For 0x1000 bytes, this has to be 0xFFF.              |

Related links
-------------

-   [Modernized fork of a56, open-source assembler for the similar 56000
    architecture](https://github.com/XboxDev/a56)

