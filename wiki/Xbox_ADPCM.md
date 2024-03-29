---
title: Xbox ADPCM
permalink: wiki/Xbox_ADPCM/
layout: wiki
tags:
 - APU
---

Xbox used it's own WAV file format to encode data using
[ADPCM](/wiki/Wikipedia:Adaptive_differential_pulse-code_modulation "wikilink").
This format is often called Xbox ADPCM.

Standard IMA ADPCM WAV files would use format code 0x0011, whereas Xbox
ADPCM files use format-code 0x0069.

There are always 1 (Mono) or 2 (Stereo) channels.

All Xbox ADPCM files seem to store 64 ADPCM input nibbles per block.
This value is also stored in the 'fmt ' extra-data which is always 2
bytes, containing the Bytes `0x40, 0x00` (64 as unsigned 16-bit
integer). Because of that, all Xbox ADPCM files will have a block
alignment of 36 (Mono) or 72 (Stereo) Bytes. As the decoder-setup in
every block contains a predictor for each channel, there will be 65
samples / channel output per block (130:36 compression ratio = 72.3%
compressed).

Aside from what was mentioned, there are no known differences to IMA
ADPCM. This is probably because the [APU](/wiki/APU "wikilink") VP will decode
ADPCM in hardware. Microsoft probably had little control over the APU
ADPCM implementation and had to stay compatible to standard ADPCM.
Players supporting IMA ADPCM also support Xbox ADPCM. However, they
might reject files due to the different format-code.

### Block format

Same as IMA ADPCM

In the following tables, the following notation is used:

-   All indices start at 0
-   W\# denotes a 32-bit word
-   B\# denotes a Byte (8-bit)
-   P denotes the ADPCM predictor for the block
-   SI denotes the Step-Index for the block
-   S\# denotes a sample
-   Background color denotes the channel:
    -   <div style="display: inline-block; width:10px; height:10px; border:1px solid black; background-color:SkyBlue">
        </div>
        Blue: Left = Right

    -   <div style="display: inline-block; width:10px; height:10px; border:1px solid black; background-color:White;">
        </div>
        White: Left

    -   <div style="display: inline-block; width:10px; height:10px; border:1px solid black; background-color:Tomato">
        </div>
        Red: Right

#### Mono

<table>
<colgroup>
<col width="-30%" />
<col width="20%" />
<col width="20%" />
<col width="20%" />
<col width="50%" />
<col width="20%" />
</colgroup>
<thead>
<tr class="header">
<th><p>32-bit word</p></th>
<th><p>W0</p></th>
<th><p>W1</p></th>
<th><p>W2</p></th>
<th><p>...</p></th>
<th><p>W8</p></th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><p>Byte</p></td>
<td><p>B0</p></td>
<td><p>B1</p></td>
<td><p>B2</p></td>
<td><p>B3</p></td>
<td><p>B4</p></td>
</tr>
<tr class="even">
<td><p>Meaning</p></td>
<td><p>P = S0</p></td>
<td><p>SI</p></td>
<td></td>
<td><p>S2</p></td>
<td><p>S1</p></td>
</tr>
</tbody>
</table>

#### Stereo

| 32-bit word | W0     | W1  | W2  | W3     |
|-------------|--------|-----|-----|--------|
| Byte        | B0     | B1  | B2  | B3     |
| Meaning     | P = S0 | SI  |     | P = S0 |

...

| 32-bit word | W14 | W15 | W16 | W17 |
|-------------|-----|-----|-----|-----|
| Byte        | B56 | B57 | B58 | B59 |
| Meaning     | S50 | S49 | S52 | S51 |

### Index-Table

Same as IMA ADPCM

|     | | +0 | | +1 | | +2 | | +3 | | +4 | | +5 | | +6 | | +7 |
|-----|------|------|------|------|------|------|------|------|
| | 0 | -1   | -1   | -1   | -1   | 2    | 4    | 6    | 8    |
| | 8 | -1   | -1   | -1   | -1   | 2    | 4    | 6    | 8    |

### Step-Table

Same as IMA ADPCM

|      | | +0  | | +1  | | +2  | | +3  | | +4  | | +5  | | +6  | | +7  | | +8  | | +9  |
|------|-------|-------|-------|-------|-------|-------|-------|-------|-------|-------|
| | 0  | 7     | 8     | 9     | 10    | 11    | 12    | 13    | 14    | 16    | 17    |
| | 10 | 19    | 21    | 23    | 25    | 28    | 31    | 34    | 37    | 41    | 45    |
| | 20 | 50    | 55    | 60    | 66    | 73    | 80    | 88    | 97    | 107   | 118   |
| | 30 | 130   | 143   | 157   | 173   | 190   | 209   | 230   | 253   | 279   | 307   |
| | 40 | 337   | 371   | 408   | 449   | 494   | 544   | 598   | 658   | 724   | 796   |
| | 50 | 876   | 963   | 1060  | 1166  | 1282  | 1411  | 1552  | 1707  | 1878  | 2066  |
| | 60 | 2272  | 2499  | 2749  | 3024  | 3327  | 3660  | 4026  | 4428  | 4871  | 5358  |
| | 70 | 5894  | 6484  | 7132  | 7845  | 8630  | 9493  | 10442 | 11487 | 12635 | 13899 |
| | 80 | 15289 | 16818 | 18500 | 20350 | 22385 | 24623 | 27086 | 29794 | 32767 |       |

### Algorithm

The algorithms for encoding and decoding are the same as IMA ADPCM. For
an example implementation and an explanation, refer to the related
links.

Related links
-------------

-   [Explanation of IMA ADPCM
    algorithms](https://wiki.multimedia.cx/index.php/IMA_ADPCM)
-   [Explanation of IMA ADPCM WAV
    storage](https://wiki.multimedia.cx/index.php/Microsoft_IMA_ADPCM)
-   [Tool to decode Xbox ADPCM
    files](https://github.com/JayFoxRox/xbox-tools/tree/master/adpcm-decoder)
-   [Sample files by
    FFmpeg](http://samples.ffmpeg.org/game-formats/xbox-adpcm-wav/)

