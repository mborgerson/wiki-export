---
title: Halo: Combat Evolved
permalink: wiki/Halo:_Combat_Evolved/
layout: wiki
---

### Debugging

To enable debugging of the original NTSC version with xbdm (be sure to
use the retail xbdm version 5455 if on a retail box due to low memory
constraints) replace the single instance of `6A 00 FF 74 24 08 E8 35`
with `33 C0 40 C2 04 00 E8 35` in the default.xbe which will skip a call
to `XnTerm`.

### System-Link protocol

The messages are encapsulated in Xbox secured network traffic.

    enum GameMode {
      CTF = 0,
      Slayer = 1,
      Oddball = 2,
      King = 3,
      Race = 4
    };

#### Server announce request

This is a broadcast message from UDP Port 5151 to port 5150.

The packet is 16 bytes long.

      b'\x01\x0c\x01\x14\x1f\x00\x01zLa(\xe9\x03\xfc\xf7\x00' # Python string  

#### Server announce response

This is a broadcast message from UDP Port 5150 to port 5151.

The packet is 280 bytes long.

    struct {

      ...

      // assert(udp_payload[43:47] == bytes([0x0C,0x00,0x00,0x50]));

      uint32_t unk47; // +47
      uint32_t unk51; // +51
      uint16_t unk55; // +55
      uint32_t unk57; // +57
      wchar_t server_name[62]; // +61 Only 15 symbols will be displayed
      char map_path[128]; // +123

      ...

      uint16_t game_mode; // +251
      uint16_t unk253; // +253
      uint16_t player_count; // +255
      uint16_t unk257; // +257
      uint16_t game_mode_parameter; // +259

      // 1 = game in progress
      // 2 = accepting players (takes priority over "game in progress")
      // 4 = team game (otherwise: free for all)
      // If neither 1 or 2 is provided, then the game is "(closed)"
      uint16_t flags; // +261

      char weird[16]; // +263 contains ASCII string "$;$Dage in a bot"

      uint8_t unk279; // +279
    };
