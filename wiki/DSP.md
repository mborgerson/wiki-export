---
title: DSP
permalink: wiki/DSP/
layout: wiki
tags:
 - APU
---

The DSPs in the APU are probably “Parthus MediaStream” DSP cores
(probably the 24-bit DSP2420 “Mozart” core).

Those are similar to Motorola DSP56362 (DSP56300 Family). If so, the
datasheet can be found at
<http://www.nxp.com/docs/en/data-sheet/DSP56362.pdf> (Also see
“Documentation” section in said datasheet for the related documentation,
like [the family
manual](https://www.nxp.com/docs/en/reference-manual/DSP56300FM.pdf))

Memory Size
-----------

|     | Program RAM Size | X Data RAM Size | Y Data RAM Size | MIXBUF Size  |
|-----|------------------|-----------------|-----------------|--------------|
| GP  | 4096 x 24-bit    | 4096 x 24-bit   | 2048 x 24-bit   | 992 x 24-bit |
| EP  | 4096 x 24-bit    | 3072 x 24-bit   | 256 x 24-bit    | n/a          |

MIXBUF is accessible at X:$001400 in the GP.

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

| Word | Meaning                    | Notes                                                                                                                                     |
|------|----------------------------|-------------------------------------------------------------------------------------------------------------------------------------------|
| 0    | Next command block address | Memory address of next command block. Bit 14 is used as End-Of-List marker.                                                               |
| 1    | Transfer control word      | Controls the DMA transfer:                                                                                                                
                                                                                                                                              
   **DSP address interleave (Bit-offset 0; 1-bit):**                                                                                          
                                                                                                                                              
   -   0 = Addressing using:                                                                                                                  
           for (sample_index = 0; sample_index < sample_count; sample_index++) {                                                              
             transfer_dsp_word(dsp_address + sample_index * dsp_step_size)                                                                    
           }                                                                                                                                  
                                                                                                                                              
   -   1 = Addressing using:                                                                                                                  
           for (block_index = 0; block_index < block_count; block_index++) {                                                                  
             for (channel_index = 0; channel_index < channel_count; channel_index++) {                                                        
               transfer_dsp_word(dsp_address + block_index + channel_index * dsp_step_size)                                                   
             }                                                                                                                                
           }                                                                                                                                  
                                                                                                                                              
   **Direction (Bit-offset 1; 1-bit):**                                                                                                       
                                                                                                                                              
   -   0 = Buffer to DSP (In)                                                                                                                 
   -   1 = DSP to buffer (Out)                                                                                                                
                                                                                                                                              
   **Unknown (Bit-offset 2; 2-bits):**                                                                                                        
                                                                                                                                              
   -   0 = ?                                                                                                                                  
   -   1 = ?                                                                                                                                  
   -   2 = ?                                                                                                                                  
   -   3 = ?                                                                                                                                  
                                                                                                                                              
   **Buffer offset writeback (Bit-offset 4; 1-bit):** *Only used for scratch buffers, behaves like 0 otherwise.*                              
                                                                                                                                              
   -   0 = Don't update buffer offset (Word 4)                                                                                                
   -   1 = Update buffer offset (Word 4); this respects address wrapping of circular buffers                                                  
                                                                                                                                              
   **Buffer (Bit-offset 5; 4-bits):**                                                                                                         
                                                                                                                                              
   -   0x0 = FIFO0 (In / Out)                                                                                                                 
   -   0x1 = FIFO1 (In / Out)                                                                                                                 
   -   0x2 = FIFO2 (Out only; Buffer to DSP is ignored)                                                                                       
   -   0x3 = FIFO3 (Out only; Buffer to DSP is ignored)                                                                                       
   -   0x4 = ?                                                                                                                                
   -   0x5 = ?                                                                                                                                
   -   0x6 = ?                                                                                                                                
   -   0x7 = ?                                                                                                                                
   -   0x8 = ?                                                                                                                                
   -   0x9 = ?                                                                                                                                
   -   0xA = ?                                                                                                                                
   -   0xB = ?                                                                                                                                
   -   0xC = ?                                                                                                                                
   -   0xD = ?                                                                                                                                
   -   0xE = Scratch (Circular)                                                                                                               
   -   0xF = Scratch                                                                                                                          
                                                                                                                                              
   **Unknown (Bit-offset 9; 1-bit):**                                                                                                         
                                                                                                                                              
   -   0 = ?                                                                                                                                  
   -   1 = ?                                                                                                                                  
                                                                                                                                              
   **Sample format (Bit-offset 10; 3-bits):** **                                                                                              
                                                                                                                                              
   -   0x0 = 8 bit (1 DSP word / 1 byte); *sample-count must be multiple of 4, or transfer is skipped; rounded; byte MSB is flipped*          
       -   Buffer to DSP: bytes: `0x12,0x34,0x56,0x78` → words: `0x920000, 0xB40000, 0xD60000, 0xF80000`                                      
       -   Buffer to DSP: bytes: `0x92,0xB4,0xD6,0xF8` → words: `0x120000, 0x340000, 0x560000, 0x780000`                                      
       -   DSP to buffer: words: `0x927FFF, 0xB47FFF, 0xD67FFF, 0xF87FFF` → bytes: `0x12,0x34,0x56,0x78` *(Rounded down)*                     
       -   DSP to buffer: words: `0x928000, 0xB48000, 0xD68000, 0xF88000` → bytes: `0x13,0x35,0x57,0x79` *(Rounded up)*                       
       -   DSP to buffer: words: `0x800000, 0x7E7FFF, 0x7E8000, 0x7FFFFF` → bytes: `0x00,0xFE,0xFF,0xFF` *(Saturated)*                        
   -   0x1 = 16 bit (1 DSP word / 2 bytes) *sample-count must be multiple of 2, or transfer is skipped; truncated*                            
       -   Buffer to DSP: bytes: `0x34,0x12` → word: `0x123400`                                                                               
       -   DSP to buffer: word: `0x1234FF` → bytes: `0x34,0x12` *(Truncated)*                                                                 
       -   DSP to buffer: word: `0x123400` → bytes: `0x34,0x12` *(Truncated)*                                                                 
   -   0x2 = 24 bit in MSB (1 DSP word / 4 bytes); *padded*                                                                                   
       -   Buffer to DSP: bytes: `0x00,0x56,0x34,0x12` → word: `0x123456` *(Padding ignored)*                                                 
       -   Buffer to DSP: bytes: `0xFF,0x56,0x34,0x12` → word: `0x123456` *(Padding ignored)*                                                 
       -   DSP to buffer: word: `0x123456` → bytes: `0x00,0x56,0x34,0x12` *(Zero padding)*                                                    
   -   0x3 = 32 bit (2 DSP words / 4 bytes); *trunacted*                                                                                      
       -   Buffer to DSP: bytes: `0x12,0xBC,0x9A,0x78` → words: `0x120000, 0x789ABC`                                                          
       -   DSP to buffer: words: `0x12FFFF, 0x789ABC` → bytes: `0x12,0xBC,0x9A,0x78` *(Truncated)*                                            
       -   DSP to buffer: words: `0x120000, 0x789ABC` → bytes: `0x12,0xBC,0x9A,0x78` *(Truncated)*                                            
   -   0x4 = *Transfer skipped*                                                                                                               
   -   0x5 = *Transfer skipped*                                                                                                               
   -   0x6 = 24 bit in LSB (1 DSP word / 4 bytes); *padded*                                                                                   
       -   Buffer to DSP: bytes: `0x56,0x34,0x12,0x00` → word: `0x123456` *(Padding ignored)*                                                 
       -   Buffer to DSP: bytes: `0x56,0x34,0x12,0xFF` → word: `0x123456` *(Padding ignored)*                                                 
       -   DSP to buffer: word: `0x123456` → bytes: `0x56,0x34,0x12,0x00` *(Zero padding)*                                                    
   -   0x7 = *Transfer skipped*                                                                                                               
                                                                                                                                              
   **Unknown (Bit-offset 13; 1-bit):**                                                                                                        
                                                                                                                                              
   -   0 = ?                                                                                                                                  
   -   1 = ?                                                                                                                                  
                                                                                                                                              
   **DSP address step size (Bit-offset 14; 10-bits):**                                                                                        
                                                                                                                                              
   Used in DSP address calculation                                                                                                            |
| 2    | Sample count               | Behaviour depends on interleave setting                                                                                                   
                                                                                                                                              
   -   Non-interleaved mode:                                                                                                                  
                                                                                                                                              
                                                                                                                                              
   **Sample count (Bit-offset 0; 24-bits):**                                                                                                  
                                                                                                                                              
   The number of samples to be transferred                                                                                                    
                                                                                                                                              
   -   Interleaved mode:                                                                                                                      
                                                                                                                                              
                                                                                                                                              
   **Channel count (Bit-offset 0; 4-bits):**                                                                                                  
                                                                                                                                              
   The number of channels minus 1. For 3 channels, this has to be 0x2.                                                                        
                                                                                                                                              
   **Block count (Bit-offset 4; 20-bits):**                                                                                                   
                                                                                                                                              
   The number of blocks to be transferred.                                                                                                    |
| 3    | DSP address                | This is the address in the DSP:                                                                                                           
                                                                                                                                              
   -   0x0000 - 0x17FF = X-Memory                                                                                                             
   -   0x1800 - 0x27FF = Y-Memory                                                                                                             
   -   0x2800 - 0x37FF = P-Memory                                                                                                             |
| 4    | Buffer offset              | *Only used for scratch buffers, ignored otherwise.* This is the offset within the buffer where the first sample is accessed.              
                                                                                                                                              
   If this is above or equal to the buffer end (buffer base + buffer size), then the write will behave like a non circular write.             |
| 5    | Buffer base                | *Only used for circular scratch buffer, ignored otherwise.* The start address of the buffer.                                              |
| 6    | Buffer size                | *Only used for circular scratch buffer, ignored otherwise.* Size of buffer minus 1. For a buffer with 0x1000 bytes, this has to be 0xFFF. |

### FIFO

Each of the DSP has 4 output FIFOs, and 2 input FIFOs.

All FIFOs belonging to the same DSP share the memory view. The memory
view is controlled in the DSPs FIFO Scatter-Gather entries.

Each FIFO has 3 address registers, which define a ringbuffer:

-   Base address: The address of the first byte in the FIFO.
-   End address: The address of the first byte that's no longer part of
    the FIFO (so this address is exclusive).
-   Current address: The absolute address of the next byte that will be
    written.

During a transfer, the current address is incremented. If the current
address hits the end address during a transfer, the current address is
moved to the base, where the transfer continues.

If the transfer length is greater than the FIFO length when using the
output FIFO, the transfer will continue normally and overwrite
previously transferred bytes. If the transfer length is greater than the
FIFO length when using the input FIFO, the transfer will continue
normally and read previously transferred bytes again.

If the current address is below the base address when starting a
transfer, it is moved to the base address. If the current address is
above or equal to the end address when starting a transfer, the DSP
hangs.

Related links
-------------

-   [Modernized fork of a56, open-source assembler for the similar 56000
    architecture](https://github.com/XboxDev/a56)
-   [Script to analyze DSP
    instructions](https://github.com/JayFoxRox/xbox-tools/pull/70)
-   [Script to control DSP
    DMA](https://github.com/JayFoxRox/xbox-tools/pull/60)
-   [MediaStream product
    page](https://web.archive.org/web/20010212122052/http://www.parthus.com:80/platforms/parthus_mediastream/index.html)
-   [First Silicon MediaStream
    tools](https://web.archive.org/web/20011130084353/http://www.fs2.com:80/parthus_download/)
-   [Tasking MediaStream
    compiler](https://web.archive.org/web/20020213235936/http://www.tasking.com/products/MediaStream/index.html)

