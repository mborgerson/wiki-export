---
title: XDVDFS
permalink: wiki/XDVDFS/
layout: wiki
---

XDVDFS (Also known as XISO) is the image format used for [Xbox Game
Discs](/wiki/Xbox_Game_Disc "wikilink"). It is stored in the data area.

Format
------

Each sector is 2048 bytes.

### Filesystem

### Volume descriptor

32 sectors which are zero-filled. The Volume descriptor is 4096 bytes,
but split into 2x 2048 sections.

-   **Section 1:** The first data is the magic at sector 32. It is
    always “MICROSOFT\*XBOX\*MEDIA”.
-   **Section 2:** The first data is the magic at sector 32. It is
    always “MICROSOFT\*XBOX\*MEDIA”.

#### Examples

##### Wave 1

Developers used xblayout to create DVD layout. Then generated the final
image using xbpremaster. The final image was submitted to Microsoft.

-   [Azurik - Rise of Perathia](/wiki/Azurik_-_Rise_of_Perathia "wikilink")
    (NTSC)
    -   xblayout version: 1.0.3926.1
    -   xbpremaster version: 1.0.3926.1
-   [Genma Onimusha](/wiki/Genma_Onimusha "wikilink") (PAL)
    -   xblayout version: 1.0.4039.1
    -   xbpremaster version: 1.0.4039.2
-   [Max Payne](/wiki/Max_Payne "wikilink") (PAL)
    -   xblayout version: 1.0.4134.1
    -   xbpremaster version: 1.0.4242.1
-   [Petit Copter](/wiki/Petit_Copter "wikilink") (Japanese)
    -   xblayout version: 1.0.4361.1
    -   xbpremaster version: 1.0.4361.2
-   [007 - Agent Under Fire](007_-_Agent_Under_Fire "wikilink") (PAL)
    -   xblayout version: 1.0.4432.1
    -   xbpremaster version: 1.0.4432.1

##### Wave 2

. Developers used a new version of xblayout and submitted the layout +
content to Microsoft Microsoft generated the final image.

-   [Metal Gear Solid 2 -
    Substance](/wiki/Metal_Gear_Solid_2_-_Substance "wikilink") (NTSC)
    -   xblayout version: 1.0.4721.1
-   [Battle Engine Aquila](/wiki/Battle_Engine_Aquila "wikilink") (PAL)
    -   xblayout version: 1.0.4831.1

##### Wave 3

. Same as wave 2, but Microsoft upgraded their tool to use new random
padding.

-   [Metal Gear Solid 2 -
    Substance](/wiki/Metal_Gear_Solid_2_-_Substance "wikilink") (PAL)
    -   xblayout version: 1.0.5120.1
-   [Shenmue II](/wiki/Shenmue_II "wikilink") (PAL)
    -   xblayout version: 1.0.5120.1

##### Wave 4

. Same as wave 3, but developers now had to use the xbgamedisc tool
instead of xblayout. The mastering tool used by microsoft also leaves a
version identifier now.

-   [Star Wars - Knights of the Old
    Republic](/wiki/Star_Wars_-_Knights_of_the_Old_Republic "wikilink") (PAL)
    -   xbgamedisc version: 2.1.0.5233.1
    -   mastering tool version: 2.1.0.5233.1
-   [Indiana Jones and the Emperor's
    Tomb](/wiki/Indiana_Jones_and_the_Emperor's_Tomb "wikilink") (PAL)
    -   xbgamedisc version: 2.1.0.5233.1
    -   mastering tool version: 2.1.0.5233.1
-   [Dynasty Warriors 4](/wiki/Dynasty_Warriors_4 "wikilink") (PAL)
    -   xbgamedisc version: 2.1.0.5344.1
    -   mastering tool version: 2.1.0.5344.1
-   [The Matrix - Path of Neo](/wiki/The_Matrix_-_Path_of_Neo "wikilink")
    (PAL)
    -   xbgamedisc version: 2.1.0.5849.1
    -   mastering tool version: 2.1.0.5849.1
-   [The Suffering - Ties That
    Bind](/wiki/The_Suffering_-_Ties_That_Bind "wikilink")
    -   xbgamedisc version: 2.1.0.5849.1
    -   mastering tool version: 2.1.0.5849.1
-   [Reservoir Dogs](/wiki/Reservoir_Dogs "wikilink") (PAL)
    -   xbgamedisc version: 2.1.0.5849.1
    -   mastering tool version: 2.1.0.5849.1

### Directory Entry

#### Version 4361

File entry flags:

-   READONLY = 0x01
-   HIDDEN = 0x02
-   SYSTEM = 0x04
-   DIRECTORY = 0x10
-   ARCHIVE = 0x20
-   NORMAL = 0x80

### File data blocks

#### Version 4361

Incomplete sectors are padded with 0x00 bytes.

### Random blocks

#### Version 3926 - 4721

Seeded and then starting to emit bytes in data area. Filled with the
following algorithm:

    // State
    uint32_t a;
    uint32_t b;
    uint32_t c;

    // Helper
    static uint32_t Value(void) {
      uint64_t result;
      result = c;
      result += 1;
      result *= b;
      result %= 0xFFFFFFFB;
      c = result & 0xFFFFFFFF;
      return c ^ a;
    }

    // buffer must be at even address, length must be multiple of 2
    void Fill(uint16_t* buffer, size_t length) {
      while(length >= 2) {
        *buffer++ = Value() >> 8;
        length -= 2;
      }
    }

    void Seed() {
      const uint32_t b_seeds[8] = {
        0x52F690D5,
        0x534D7DDE,
        0x5B71A70F,
        0x66793320,
        0x9B7E5ED5,
        0xA465265E,
        0xA53F1D11,
        0xB154430F
      };

      FILETIME ft;
      GetSystemTimeAsFileTime(&ft);
      uint32_t seed = ft.dwLowDateTime ^ ft.dwHighDateTime;
      a = 0;
      b = b_seeds[seed & 7];
      c = seed;
      a = Value();
    }

The RNG is seeded when the mastering tool / wizard is started. The first
portion of the code to use this random number generator is the code
which generates layer 0 and layer 1.

#### Version 4831 - 5849

The algorithm was switched to RC4-256-drop-2048.


    #include <openssl/rc4.h>

    RC4_KEY rc4key;

    void Fill(uint8_t* buffer, size_t length) {
      // We need a zero buffer as OpenSSL will want to XOR the keystream with data.
      // The XDVDFS random data is the keystream without changes, so we let XOR OpenSSL XOR with 0x00 bytes.
      uint8_t zero_buffer[length];
      memset(zero_buffer, 0x00, length);
      RC4(&rc4key, length, zero_buffer, buffer);
    }

    void Seed() {
      union {
        uint8_t raw[16];
        struct {
          struct _FILETIME SystemTimeAsFileTime; // first 8 bytes, little-endian
          uint64_t tsc; // later 8 bytes, little-endian
        };
      } key;

      // Initialize seed
      key.tsc = rdtsc();
      GetSystemTimeAsFileTime(&key.SystemTimeAsFileTime);
      RC4_set_key(&rc4key, 16, &key);

      // Drop the first 2048 bytes of the RC4 keystream
      uint8_t discard_buffer[2048];
      Fill(discard_buffer, 2048);
    }

The RNG is seeded when the mastering tool / wizard is started. The first
portion of the code to use this random number generator is the code
which generates layer 0 and layer 1.

### Security blocks

#### Version 4361

Treated like random block.

Tools
-----

-   [XBISO](https://github.com/OpenXDK/xbiso)
-   [extract-xiso](https://github.com/mborgerson/extract-xiso)

