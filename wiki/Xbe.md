---
title: Xbe
permalink: wiki/Xbe/
layout: wiki
tags:
 - Fileformats
---

XBE files (XBox Executable) are the main files that are executed in the
Xbox System. In official games, these files are created by game
developers, and then signed by Microsoft.

### Title ID

A title ID is usually 2 ASCII letters for the publisher, followed by a
u16 integer game number (Above 2000 for non-original Xbox games)

| Publisher ID | Name                                                             |
|--------------|------------------------------------------------------------------|
| AC           | Acclaim Entertainment                                            |
| AH           | ARUSH Entertainment                                              |
| AQ           | Aqua System                                                      |
| AS           | ASK                                                              |
| AT           | Atlus                                                            |
| AV           | Activision                                                       |
| AY           | Aspyr Media                                                      |
| BA           | Bandai                                                           |
| BL           | Black Box                                                        |
| BM           | BAM! Entertainment                                               |
| BR           | Broccoli Co.                                                     |
| BS           | Bethesda Softworks                                               |
| BU           | Bunkasha Co.                                                     |
| BV           | Buena Vista Games                                                |
| BW           | BBC Multimedia                                                   |
| BZ           | Blizzard                                                         |
| CC           | Capcom                                                           |
| CK           | Kemco Corporation                                                |
| CM           | Codemasters                                                      |
| CV           | Crave Entertainment                                              |
| DC           | DreamCatcher Interactive                                         |
| DX           | Davilex                                                          |
| EA           | Electronic Arts (EA)                                             |
| EC           | Encore inc                                                       |
| EL           | Enlight Software                                                 |
| EM           | Empire Interactive                                               |
| ES           | Eidos Interactive                                                |
| FI           | Fox Interactive                                                  |
| FS           | From Software                                                    |
| GE           | Genki Co.                                                        |
| GV           | Groove Games                                                     |
| HE           | Tru Blu (Entertainment division of Home Entertainment Suppliers) |
| HP           | Hip games                                                        |
| HU           | Hudson Soft                                                      |
| HW           | Highwaystar                                                      |
| IA           | Mad Catz Interactive                                             |
| IF           | Idea Factory                                                     |
| IG           | Infogrames                                                       |
| IL           | [Interlex Corporation](/wiki/Interlex_Corporation "wikilink")          |
| IM           | Imagine Media                                                    |
| IO           | Ignition Entertainment                                           |
| IP           | Interplay Entertainment                                          |
| IX           | InXile Entertainment                                             |
| JA           | Jaleco                                                           |
| JW           | JoWooD                                                           |
| KB           | Kemco                                                            |
| KI           | Kids Station Inc.                                                |
| KN           | Konami                                                           |
| KO           | KOEI                                                             |
| KU           | Kobi and/or GAE (formerly Global A Entertainment)                |
| LA           | LucasArts                                                        |
| LS           | Black Bean Games (publishing arm of Leader S.p.A.)               |
| MD           | Metro3D                                                          |
| ME           | Medix                                                            |
| MI           | Microïds                                                         |
| MJ           | Majesco Entertainment                                            |
| MM           | Myelin Media                                                     |
| MP           | MediaQuest                                                       |
| MS           | Microsoft Game Studios                                           |
| MW           | Midway Games                                                     |
| MX           | Empire Interactive                                               |
| NK           | NewKidCo                                                         |
| NL           | NovaLogic                                                        |
| NM           | Namco                                                            |
| OX           | Oxygen Interactive                                               |
| PC           | Playlogic Entertainment                                          |
| PL           | Phantagram Co., Ltd.                                             |
| RA           | Rage                                                             |
| SA           | Sammy                                                            |
| SC           | SCi Games                                                        |
| SE           | SEGA                                                             |
| SN           | SNK                                                              |
| SS           | Simon & Schuster                                                 |
| SU           | Success Corporation                                              |
| SW           | Swing! Deutschland                                               |
| TA           | Takara                                                           |
| TC           | Tecmo                                                            |
| TD           | The 3DO Company (or just 3DO)                                    |
| TK           | Takuyo                                                           |
| TM           | TDK Mediactive                                                   |
| TQ           | THQ                                                              |
| TS           | Titus Interactive                                                |
| TT           | Take-Two Interactive Software                                    |
| US           | Ubisoft                                                          |
| VC           | Victor Interactive Software                                      |
| VN           | Vivendi Universal (just took Interplays publishing rights)       |
| VU           | Vivendi Universal Games                                          |
| VV           | Vivendi Universal Games                                          |
| WE           | Wanadoo Edition                                                  |
| WR           | Warner Bros. Interactive Entertainment                           |
| XI           | XPEC Entertainment and Idea Factory                              |
| XK           | *Xbox kiosk disk?*                                               |
| XL           | *Xbox special bundled or live demo disk?*                        |
| XM           | Evolved Games                                                    |
| XP           | XPEC Entertainment                                               |
| XR           | Panorama                                                         |
| YB           | YBM Sisa (South-Korea)                                           |
| ZD           | Zushi Games (formerly Zoo Digital Publishing)                    |

The title ID seems to double the information from the [Xbox Game
Disc](/wiki/Xbox_Game_Disc "wikilink") mastering code etched into the ring or
readable from the DMI. The game number is expressed in 3 decimal digits
here which suggests that it will always be below 1000.

**Examples**:

[FIFA Soccer 2003](/wiki/FIFA_Soccer_2003 "wikilink"):

-   DMI and mastering code: EA02302E (Meaning: publisher EA, game number
    023, version 02, region Europe)
-   Title ID:

[Halo: Combat Evolved](/wiki/Halo:_Combat_Evolved "wikilink"):

-   DMI and mastering code: MS00402A (Meaning: publisher Microsoft, game
    number 004, version 02, region America)
-   Title ID: 4D530004 \[MS-004\]

[Halo: Combat Evolved](/wiki/Halo:_Combat_Evolved "wikilink"):

-   DMI and mastering code: MS00404E (Meaning: publisher Microsoft, game
    number 004, version 04, region Europe)
-   Title ID: 4D530004 \[MS-004\]

### Sections

#### .text

The .text section contains all x86 subroutines to be executed by the
[processor](/wiki/CPU "wikilink").

#### .rdata

The .rdata section contains the [kernel thunk table](/wiki/Kernel "wikilink").
The ordinals in the table are to be resolved to the kernel's actual
calling routine, when loaded.

Certificate
-----------

Each Xbox executable has a certificate that contains information about
the title.

-   Time and date when the certificate was created
-   Title ID
-   Title name
-   Alternative title IDs
-   Allowed types of media that the executable can be run from (HD, DVD,
    CD, etc.)
-   Game region
-   Game ratings
-   Disk number
-   Version
-   LAN key raw data used for [System Link](/wiki/System_Link "wikilink")
-   Signature key raw data (used to sign
    [savegames](/wiki/Xbox_Savegame_System "wikilink"))
-   Alternate signature keys
-   Original size of the certificate
-   Online service name (not present in early executables)
-   Run time security flags (not present in early executables)

Xbox Alpha executable format
----------------------------

Binaries from early Xbox development (Alpha units), are using a
different binary format. There are no known public tools that can read
them. Known differences include that the first bytes of the file are
'XE' instead the 'XBEH' from the final XBE format. The format is rumored
to be more like the Windows PE format.

Resources and links
-------------------

-   [.XBE File Format 1.1](http://www.caustik.com/cxbx/download/xbe.htm)

