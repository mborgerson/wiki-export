---
title: Xtf
permalink: wiki/Xtf/
layout: wiki
---

XTF is a font file format used in the [Dashboard](/wiki/Dashboard "wikilink").
It became famous for being [exploited](/wiki/Exploits#Font_hacks "wikilink").

Dashboard fonts
---------------

The fonts were designed by [Steve
Matteson](/wiki/Wikipedia:Steve_Matteson "wikilink") for use in the Xbox
[Dashboard](/wiki/Dashboard "wikilink") as well as for promotional materials.

The XTF versions of these fonts contain 7365 glyphs each.

### Xbox.xtf

<img src="Xbox-dashboard-font-specimen.png" title="Xbox Font Specimen" alt="Xbox Font Specimen" width="200" />

A monospace font.

<div style="display: inline-block;">
<img src="Xbox-xtf.png" title="Xbox.xtf from Dashboard." alt="Xbox.xtf from Dashboard." width="800" />

</div>
### XBox Book.xtf

<div style="display: inline-block;">
<img src="XBox_Book-xtf.png" title="XBox Book.xtf from Dashboard." alt="XBox Book.xtf from Dashboard." width="800" />

</div>
File format
-----------

-   4 byte (magic)
-   4 byte (length prefix for following string)
-   zero-terminated string with given buffer length (font-name)
-   [GLYPHSET](https://msdn.microsoft.com/en-us/library/dd144956%28v=vs.85%29.aspx)
    (List of supported glyphs)
-   For each cGlyphsSupported:
    -   [GLYPHMETRICSFLOAT](https://msdn.microsoft.com/en-us/library/windows/desktop/dd374209(v=vs.85).aspx)
        (Metrics for each glyph)
    -   4 byte (Offset of glyph in file)
-   For each GLYPHSET range
    -   For each glyph in this range
        -   2 byte (Index count)
        -   2 byte (Vertex count)
        -   For each index:
            -   2 byte (Vertex index)
        -   For each vertex:
            -   4 byte float (X-coordinate)
            -   4 byte float (Y-coordinate)

The mesh data is stored as triangle list. 3 indices per triangle.

Links
-----

-   [A tool to convert XTF fonts to SVG
    fonts](https://github.com/JayFoxRox/xbox-tools/tree/master/xtf-converter)

