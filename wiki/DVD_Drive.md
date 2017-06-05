---
title: DVD Drive
permalink: wiki/DVD_Drive/
layout: wiki
---

The Xbox contains a DVD Drive which can read [Xbox Game
Discs](/wiki/Xbox_Game_Disc "wikilink").

There are four retail drives known to be used by Microsoft in the retail
version of the console. any xbox dvd drive can be used in any retail
xbox. Development kits can use and read debug signed code from these
drives. Other drives that might work are firmware modded PC drives like
Kreon drives.

List of Xbox DVD Drive manufacturers

-   Thomson
-   Philips
-   Samsung
-   Hitachi-LG (mainly 1.6?)

![Xbox DVD Drive
determination](Xbox_drivedetermination.png "Xbox DVD Drive determination")

Extra commands / modifications
------------------------------

### IDENTIFY

### READ DVD STRUCTURE

### MODE SENSE and MODE SELECT

Page 0x3E is used for security. Accessed through *MODE SELECT 10* and
*MODE SENSE 10*.

<table>
<thead>
<tr class="header">
<th><p>Offset</p></th>
<th><p>Field</p></th>
<th><p>Type</p></th>
<th><p>Notes</p></th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><p>0</p></td>
<td><p>Mode page</p></td>
<td><p>u8</p></td>
<td></td>
</tr>
<tr class="even">
<td><p>1</p></td>
<td><p>Length</p></td>
<td><p>u8</p></td>
<td><p>Excluding this and the field before. Should always be 18</p></td>
</tr>
<tr class="odd">
<td><p>2</p></td>
<td><p>Partition select</p></td>
<td><p>u8</p></td>
<td><p>0x00 = Video partition<br />
0x01 = Xbox partition<br />
<br />
This will be set to 0x01 by the kernel when the last challenge was verified. This is done by sending the same challenge again, the challenge id / value is not reset.</p></td>
</tr>
<tr class="even">
<td><p>3</p></td>
<td><p>Unknown</p></td>
<td><p>u8</p></td>
<td><p>If this is not 1, the kernel will reject this as an XGD (but still allow normal access?!)</p></td>
</tr>
<tr class="odd">
<td><p>4</p></td>
<td><p>Authenticated</p></td>
<td><p>u8</p></td>
<td><p>0x00 = Not authenticated<br />
0x01 = Already authenticated or authentication in progress<br />
<br />
This will be set to 0x01 by the kernel when the first challenge is send.</p></td>
</tr>
<tr class="even">
<td><p>5</p></td>
<td><p>Booktype (0xF0) and Bookversion (0x0F)</p></td>
<td><p>u8</p></td>
<td><p>Booktype 0xD is used for Xbox games. This must match info from the SS.</p></td>
</tr>
<tr class="odd">
<td><p>6</p></td>
<td><p>Unknown</p></td>
<td><p>u8</p></td>
<td><p>?</p></td>
</tr>
<tr class="even">
<td><p>7</p></td>
<td><p>Challenge id</p></td>
<td><p>u8</p></td>
<td></td>
</tr>
<tr class="odd">
<td><p>8</p></td>
<td><p>Challenge value</p></td>
<td><p>u32</p></td>
<td></td>
</tr>
<tr class="even">
<td><p>12</p></td>
<td><p>Response value</p></td>
<td><p>u32</p></td>
<td></td>
</tr>
<tr class="odd">
<td><p>16</p></td>
<td><p>Unknown</p></td>
<td><p>u8</p></td>
<td><p>Unused?</p></td>
</tr>
<tr class="even">
<td><p>17</p></td>
<td><p>Unknown</p></td>
<td><p>u8</p></td>
<td><p>Unused?</p></td>
</tr>
<tr class="odd">
<td><p>18</p></td>
<td><p>Unknown</p></td>
<td><p>u8</p></td>
<td><p>Unused?</p></td>
</tr>
<tr class="even">
<td><p>19</p></td>
<td><p>Unknown</p></td>
<td><p>u8</p></td>
<td><p>Unused?</p></td>
</tr>
</tbody>
</table>

DVD authentication
------------------

References and links
--------------------

-   [List of original Xbox DVD
    drives](http://web.archive.org/web/20151026074806/http://home.comcast.net/~admiral_powerslave/dvddrives.html)
-   <https://multimedia.cx/eggs/interfacing-to-an-xbox-optical-drive/>

