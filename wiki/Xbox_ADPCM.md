---
title: Xbox ADPCM
permalink: wiki/Xbox_ADPCM/
layout: wiki
---

Xbox used it's own WAV file format to encode ADPCM data. This format is
often called Xbox ADPCM.

Standard IMA ADPCM WAV files would use format code 0x0011, whereas Xbox
ADPCM files use format code 0x0069. Despite this format-code difference
the file formats (including header and data) are otherwise the same.

All Xbox ADPCM files seem to use 64 samples per block. This value is
also stored in the 'fmt ' extra-data which is always 2 bytes, containing
the Bytes `0x40, 0x00` (64 as unsigned 16-bit integer). Because of that,
all Xbox ADPCM files will have a block alignment of 36 (Mono) or 72
(Stereo).

Aside from this there are no known differences to IMA ADPCM. This is
probably because the [APU](/wiki/APU "wikilink") VP will decode ADPCM in
hardware. Microsoft probably had little control over the APU ADPCM
implementation and had to stay compatible to standard ADPCM.

### Index-Table

Same as IMA ADPCM

|     | +0  | +1  | +2  | +3  | +4  | +5  | +6  | +7  |
|-----|-----|-----|-----|-----|-----|-----|-----|-----|
| 0   | -1  | -1  | -1  | -1  | 2   | 4   | 6   | 8   |
| 8   | -1  | -1  | -1  | -1  | 2   | 4   | 6   | 8   |

### Step-Table

Same as IMA ADPCM

|     | +0    | +1    | +2    | +3    | +4    | +5    | +6    | +7    | +8    | +9    |
|-----|-------|-------|-------|-------|-------|-------|-------|-------|-------|-------|
| 0   | 7     | 8     | 9     | 10    | 11    | 12    | 13    | 14    | 16    | 17    |
| 10  | 19    | 21    | 23    | 25    | 28    | 31    | 34    | 37    | 41    | 45    |
| 20  | 50    | 55    | 60    | 66    | 73    | 80    | 88    | 97    | 107   | 118   |
| 30  | 130   | 143   | 157   | 173   | 190   | 209   | 230   | 253   | 279   | 307   |
| 40  | 337   | 371   | 408   | 449   | 494   | 544   | 598   | 658   | 724   | 796   |
| 50  | 876   | 963   | 1060  | 1166  | 1282  | 1411  | 1552  | 1707  | 1878  | 2066  |
| 60  | 2272  | 2499  | 2749  | 3024  | 3327  | 3660  | 4026  | 4428  | 4871  | 5358  |
| 70  | 5894  | 6484  | 7132  | 7845  | 8630  | 9493  | 10442 | 11487 | 12635 | 13899 |
| 80  | 15289 | 16818 | 18500 | 20350 | 22385 | 24623 | 27086 | 29794 | 32767 |       |

### Algorithm

The algorithms for encoding and decoding are the same as IMA ADPCM. For
an example implementation and an explanation, refer to the related
links.

Related links
-------------

-   [Sample files by
    FFmpeg](http://samples.ffmpeg.org/game-formats/xbox-adpcm-wav/)
-   [Explanation of IMA ADPCM
    encoding](https://wiki.multimedia.cx/index.php/IMA_ADPCM)
-   [Tool to decode Xbox ADPCM
    files](https://github.com/JayFoxRox/xbox-tools/tree/master/adpcm-decoder)
-   [1](https://wiki.multimedia.cx/index.php/Microsoft_IMA_ADPCM)

