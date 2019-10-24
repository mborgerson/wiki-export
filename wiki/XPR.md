---
title: XPR
permalink: wiki/XPR/
layout: wiki
---

**XPR** probably stands for **Xbox Packed Resource**. It's a structure
and file-format used by the [Microsoft XDK](/wiki/Microsoft_XDK "wikilink").
It is being used in parts of [XAPI](/wiki/XAPI "wikilink").

XPRs can store a variety of different data types that are useful for
Xbox D3D. XPRs are storing data similar to internal structures in the
D3D driver. So D3D has mechanisms to simply load such resources into D3D
objects (by moving them into GPU accessible memory).

The structure is something like this:

    struct {
      char magic[4]; // Always "XPR0" / 0x30525058
      uint32_t size; // Size of XPR (including magic and this field)
      uint32_t header_size; // Size of the following header (always 2048?)
    } XPR;

This is followed by:

    union {
      struct {
        union {
          uint32_t flags; // Type information and who "owns" this resource
          struct {
            uint32_t ref_count:16; // (lowest bits)
            uint32_t type:3;
            uint32_t unk3:13; // (highest bits)
          };
        };
        union { // Vertexbuffer (type 0x0)
          ...
        } vertexbuffer;
        union { // Indexbuffer (type 0x1)
          ...
        } indexbuffer;
        union { // Unknown (type 0x2)
          ...
        } ...;
        union { // Unknown (type 0x3)
          ...
        } ...;
        union { // Texture (type 0x4)
          uint32_t unk0; // Always 0?
          uint32_t unk1; // Always 0?
          uint32_t format; // GPU register
          uint32_t unk2; // Always 0?
          uint32_t end; // Always 0xFFFFFFFF
        } texture;
        union { // Unknown (type 0x5)
          ...
        } ...;
        union { // Unknown (type 0x6)
          ...
        } ...;
        union { // Unknown (type 0x7)
          ...
        } ...;
      };
      uint8_t pad[2048]; // Padded with 0xAD bytes
    } XPR_header;

The headers are then followed by the resource data.
