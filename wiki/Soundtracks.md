---
title: Soundtracks
permalink: wiki/Soundtracks/
layout: wiki
---

The Xbox allows for soundstracks to be stored on the harddrive and
played in-game. Music is copied from audio CDs using the dashboard and
converted to WMA audio files. The notes gathered here are from
decompiling StDB.dll from *Xbox Soundtrack Manager*.

Folder Structure
----------------

All soundtrack related data is stored in E:\\TDATA\\fffe0000. Songs are
organized into groups of folders containing up to 6 WMA files.
Zero-padded 4 decimal integers are used for the folder names, and
zero-padded 8 decimal integers are used for the WMA file names. Metadata
such as song title and which soundtrack a song belongs to is stored in
the ST.DB file

ST.DB
-----

The ST.DB file contains information about soundtracks loaded on the
Xbox. Up to 100 soundtracks with 500 songs each can be used. Each header
and struct described below is padded to 512 bytes.

File Layout:

    (start of file)
    0000  Main Header
    0200  Soundtrack Struct 1
    0400  [Soundtrack Struct 2]
          ...
    CA00  Song Group 1
    CC00  [Song Group 2]
          ...
    (end of file)

### Main Header

All values are little-endian.

| Type  | Description          | Comment           |
|-------|----------------------|-------------------|
| int32 | magic                | always 0x00000001 |
| int32 | numSoundtracks       |                   |
| int32 | nextSoundtrackId     |                   |
| int32 | soundtrackIds\[100\] |                   |
| int32 | nextSongId           |                   |
| char  | padding\[96\]        |                   |

### Soundtrack Struct

| Type   | Description           | Comment           |
|--------|-----------------------|-------------------|
| int32  | magic                 | always 0x00021371 |
| int32  | id                    |                   |
| uint32 | numSongGroups         |                   |
| int32  | songGroupIds          |                   |
| int32  | totalTimeMilliseconds |                   |
| wchar  | name\[64\]            | Unicode string    |
| char   | padding\[64\]         |                   |

### Song Group Struct

| Type  | Description               | Comment                                         |
|-------|---------------------------|-------------------------------------------------|
| int32 | magic                     | always 0x00031073                               |
| int32 | soundtrackId              |                                                 |
| int32 | id                        |                                                 |
| int32 | padding                   |                                                 |
| int32 | songId\[6\]               |                                                 |
| int32 | songTimeMilliseconds\[6\] |                                                 |
| wchar | songName\[384\]           | 6 Unicode strings, each padded to 64 characters |
| char  | padding\[64\]             |                                                 |

Default Encoding Settings
-------------------------

These are the encoding settings used by the dashboard when encoding WMA
files:

| Key               | Value                                    |
|-------------------|------------------------------------------|
| Format            | WMA Version 2                            |
| Codec Description | Windows Media Audio V8                   |
| Encoding Tool     | Windows Media Encoding Utility 8.0.0.403 |
| Bit Rate          | 128 kbps                                 |
| Channels          | 2                                        |
| Sampling Rate     | 44.1 kHz                                 |
| Bit Depth         | 16 bit                                   |

The track number is added to the metadata.
