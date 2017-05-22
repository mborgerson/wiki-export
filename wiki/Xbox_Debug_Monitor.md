---
title: Xbox Debug Monitor
permalink: wiki/Xbox_Debug_Monitor/
layout: wiki
---

The **Xbox Debug Monitor** (**XBDM**) is a feature of Xbox Development
Kits that provides remote debugging, file management, console discovery,
and other services on TCP/UDP port 731. It is loaded by debug kernels at
startup from `C:\xbdm.dll` and its configuration is read from
`E:\xbdm.ini`. XBDM is distinct from KD and uses a different wire
protocol.

Name Answering Protocol
-----------------------

An Xbox Development Kit (XDK) can be assigned a *debug name* that
identifies it on the local network. XBDM provides the ability to resolve
a debug name to an IP address ([forward
lookup](#Forward_Lookup "wikilink")), resolve an IP address to a debug
name ([reverse lookup](#Reverse_Lookup "wikilink")), and
[discover](#Console_Discovery "wikilink") all XDKs on the local network
using a very simple UDP-based protocol.

| *Offsets* | Octet | 0    | 1           |
|-----------|-------|------|-------------|
| Octet     | Bit   | 0    | 1           |
| 0         | 0     | Type | Name Length |
| 2         | 16+   | Name |

A NAP packet contains 3 fields, the last of which is variable-length.
The minimum length of a NAP packet is 2 bytes and the maximum is 257.
Invalid packets are silently dropped by XBDM.

Type  
This unsigned 8-bit field may contain the values 1 (lookup), 2 (reply),
or 3 (wildcard).

Name Length  
This unsigned 8-bit field specifies the length of the Name field and
should be a value from 0 to 255. For Type 3 packets, this field should
always be 0. For Type 1 and Type 2 packets, this field should never
be 0.

Name  
This variable-length field contains the ASCII-encoded debug name for
Type 1 and Type 2 packets. The number of bytes in this field is given by
the Length field. It should not contain any `NUL` characters.

### Forward Lookup

To resolve a debug name to an IP address, send a Type 1 NAP packet
containing the debug name to be resolved to UDP address
255.255.255.255:731. The XDK with that name will respond with a Type 2
NAP packet and its IP address can be retrieved from the UDP header.
There is no way to prevent multiple XDKs being assigned the same debug
name, so it's possible that the client may receive replies from multiple
IP addresses.

### Reverse Lookup

To resolve an IP address to a debug name, send a Type 3 NAP packet with
no name (length 0) to the IP address on UDP port 731. Assuming the
target is actually an XDK, it will respond with a Type 2 NAP packet
containing its name. This is very similar to the Console Discovery
process (below), except that by sending the wildcard packet to a single
IP address, only that XDK will respond.

### Console Discovery

To discover all XDKs on the local network, send a Type 3 NAP packet with
no name (length 0) to the UDP address 255.255.255.255:731. Each XDK will
respond with a Type 2 NAP packet containing its name. As with a forward
lookup, the client may receive multiple replies with the same name, but
different IP addresses.

Remote Debugging and Control Protocol
-------------------------------------

The Remote Debugging and Control Protocol (RDCP) is a text-based
protocol transmitted over a TCP connection on port 731. RDCP resembles
protocols like FTP and SMTP, making it possible to communicate with XBDM
using just a Telnet client in many cases.

When a connection is established, XBDM sends `201- connected` (or
`200- connected` in version 3944) followed by <CR><LF> (that is, a
carriage return character followed by a line feed character). The RDCP
client is then free to send a command followed by <CR><LF> or <CR><NUL>.
A command consists of a name and zero or more parameters separated by
whitespace characters. The format of the parameters is defined by the
command, but most commands use the form `key=value`. Parameter values
that contain whitespace characters must be surrounded by double quotes
(e.g. “`some`` ``value`” or `key=`“`some`` ``value`”).

After executing a command, XBDM replies with a response line consisting
of a three-digit status code and message of the form
`999- message text`<CR><LF>. Note that unlike similar protocols, the `-`
(dash) is always present in responses and messages cannot span multiple
lines.

### Status codes

In responses, 2xx status codes indicate success and 4xx codes indicate
failure. Most codes have a default message, but some commands leave the
message field empty while others use the message field to hold whatever
data was requested by the client or additional information about an
error.

#### 2xx Success

<span id="status_200">200- OK</span>  
Standard response for successful execution of a command.

<span id="status_201">201- connected</span>  
Initial response sent after a connection is established. The client does
not need to send anything to solicit this response.

<span id="status_202">202- multiline response follows</span>  
The response line is followed by one or more additional lines of data
terminated by a line containing only a `.` (period). The client must
read all available lines before sending another command.

<span id="status_203">203- binary response follows</span>  
The response line is followed by raw binary data, the length of which is
indicated in some command-specific way. The client must read all
available data before sending another command.

<span id="status_204">204- send binary data</span>  
The command is expecting additional binary data from the client. After
the client sends the required number of bytes, XBDM will send another
response line with the final result of the command.

<span id="status_205">205- connection dedicated</span>  
The connection has been moved to a dedicated handler thread (see
[\#Connection dedication](#Connection_dedication "wikilink")).

#### 4xx Failure

<span id="status_400">400- unexpected error</span>  
An internal error occurred that could not be translated to a standard
error code. The message is typically more descriptive, such as “out of
memory” or “bad parameter”.

<span id="status_401">401- max number of connections exceeded</span>  
The connection could not be established because XBDM is already serving
the maximum number of clients (4).

<span id="status_402">402- file not found</span>  
An operation was attempted on a file that does not exist.

<span id="status_403">403- no such module</span>  
An operation was attempted on a module that does not exist.

<span id="status_404">404- memory not mapped</span>  
An operation was attempted on a region of memory that is not mapped in
the page table.

<span id="status_405">405- no such thread</span>  
An operation was attempted on a thread that does not exist.

<span id="status_406">406-</span>  
An attempt to set the system time with the `setsystime` command failed.
This status code is undocumented.

<span id="status_407">407- unknown command</span>  
The command is not recognized.

<span id="status_408">408- not stopped</span>  
The target thread is not stopped.

<span id="status_409">409- file must be copied</span>  
A move operation was attempted on a file that can only be copied.

<span id="status_410">410- file already exists</span>  
A file could not be created or moved because one already exists with the
same name.

<span id="status_411">411- directory not empty</span>  
A directory could not be deleted because it still contains files and/or
directories.

<span id="status_412">412- filename is invalid</span>  
The specified file contains invalid characters or is too long.

<span id="status_413">413- file cannot be created</span>  
The file cannot be created for some unspecified reason.

<span id="status_414">414- access denied</span>  
The file cannot be accessed at the connection's current privilege level
(see [\#Security](#Security "wikilink")).

<span id="status_415">415- no room on device</span>  
The target device has run out of storage space.

<span id="status_416">416- not debuggable</span>  
The title is not debuggable.

<span id="status_417">417- type invalid</span>  
The performance counter type is invalid.

<span id="status_418">418- data not available</span>  
The performance counter data is not available.

<span id="status_420">420- box not locked</span>  
The command can only be executed when security is enabled (see
[\#Security](#Security "wikilink")).

<span id="status_421">421- key exchange required</span>  
The client must perform a key exchange with the `keyxchg` command (see
[\#Security](#Security "wikilink")).

<span id="status_422">422- dedicated connection required</span>  
The command can only be executed on a dedicated connection (see
[\#Connection dedication](#Connection_dedication "wikilink")).

### Connection dedication

**Connection dedication** is the process of moving a client connection
from the *global server thread* to a *threaded command handler thread*
with the `dedicate` command.

By default, commands sent to XBDM are processed on the global server
thread. Built-in commands and custom command handlers registered with
`DmRegisterCommandProcessor` are run on this thread and may only execute
kernel APIs. Commands that need to call CRT or XAPI functions must run
in a threaded command handler, registered with
`DmRegisterCommandProcessorEx` or `DmRegisterThreadedCommandProcessor`.

A client that has not dedicated its connection will receive a
`422- dedicated connection required` response if it tries to execute a
threaded command on the global server thread. After dedicating itself to
a threaded command handler, the client can no longer send built-in or
non-threaded commands until it re-dedicates itself to the global server
thread.

### Security

TODO

### Commands

*See also: [XBDM commands by
version](/wiki/XBDM_commands_by_version "wikilink")*

#### <span id="cmd_adminpw">adminpw</span> (Set administrator password)

#### <span id="cmd_altaddr">altaddr</span>

#### <span id="cmd_authuser">authuser</span> (Authenticate user)

#### <span id="cmd_boxid">boxid</span>

#### <span id="cmd_break">break</span>

#### <span id="cmd_bye">bye</span> (Close connection)

#### <span id="cmd_capcontrol">capcontrol</span>/<span id="cmd_capctrl">capctrl</span>

#### <span id="cmd_continue">continue</span>

#### <span id="cmd_crashdump">crashdump</span>

#### <span id="cmd_d3dopcode">d3dopcode</span>

#### <span id="cmd_dbgname">dbgname</span> (Get/set debug name)

#### <span id="cmd_dbgoptions">dbgoptions</span>

#### <span id="cmd_debugger">debugger</span>

#### <span id="cmd_debugmode">debugmode</span>

#### <span id="cmd_dedicate">dedicate</span> (Dedicate connection)

#### <span id="cmd_deftitle">deftitle</span>

#### <span id="cmd_delete">delete</span> (Delete file)

#### <span id="cmd_dirlist">dirlist</span> (List files in directory)

#### <span id="cmd_dmversion">dmversion</span> (Get debug monitor version)

#### <span id="cmd_drivefreespace">drivefreespace</span> (Get free space on drive)

#### <span id="cmd_drivelist">drivelist</span> (List drive letters)

#### <span id="cmd_dvdblk">dvdblk</span> (Read block from DVD)

#### <span id="cmd_dvdperf">dvdperf</span>

#### <span id="cmd_fileeof">fileeof</span>

#### <span id="cmd_flash">flash</span> (Flash BIOS image)

#### <span id="cmd_fmtfat">fmtfat</span> (Format FAT partition)

#### <span id="cmd_funccall">funccall</span>

#### <span id="cmd_getcontext">getcontext</span> (Get thread context)

#### <span id="cmd_getd3dstate">getd3dstate</span> (Get Direct3D state)

#### <span id="cmd_getextcontext">getextcontext</span> (Get extended thread context)

#### <span id="cmd_getfile">getfile</span> (Download file)

<table>
<colgroup>
<col width="1%" />
<col width="99%" />
</colgroup>
<thead>
<tr class="header">
<th><p>3944-4432</p></th>
<th><p><span style="font: 14px monospace">getfile name=</span></p></th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><p>4532+</p></td>
<td><p><span style="font: 14px monospace">getfile name= [offset= size=]</span></p></td>
</tr>
</tbody>
</table>

#### <span id="cmd_getfileattributes">getfileattributes</span> (Get file attributes)

#### <span id="cmd_getgamma">getgamma</span> (Get gamma table)

#### <span id="cmd_getmem">getmem</span> (Read memory)

#### <span id="cmd_getmem2">getmem2</span> (Read memory)

#### <span id="cmd_getpalette">getpalette</span>

#### <span id="cmd_getpid">getpid</span>

#### <span id="cmd_getsum">getsum</span>

#### <span id="cmd_getsurf">getsurf</span>

#### <span id="cmd_getuserpriv">getuserpriv</span> (Get user's privilege level)

#### <span id="cmd_getutildrvinfo">getutildrvinfo</span> (Get utility drive information)

#### <span id="cmd_go">go</span>

#### <span id="cmd_gpucount">gpucount</span> (Toggle GPU counters)

#### <span id="cmd_halt">halt</span>

#### <span id="cmd_irtsweep">irtsweep</span>

#### <span id="cmd_isbreak">isbreak</span>

#### <span id="cmd_isdebugger">isdebugger</span>

#### <span id="cmd_isstopped">isstopped</span>

#### <span id="cmd_kd">kd</span> (Enable/disable kernel debugger)

#### <span id="cmd_keyxchg">keyxchg</span> (Perform key exchange)

#### <span id="cmd_lockmode">lockmode</span>

#### <span id="cmd_lop">lop</span>

#### <span id="cmd_magicboot">magicboot</span>

#### <span id="cmd_memtrack">memtrack</span>

#### <span id="cmd_mkdir">mkdir</span> (Create directory)

#### <span id="cmd_mmglobal">mmglobal</span>

#### <span id="cmd_modlong">modlong</span>

#### <span id="cmd_modsections">modsections</span>

#### <span id="cmd_modules">modules</span>

#### <span id="cmd_nostopon">nostopon</span>

#### <span id="cmd_notify">notify</span>

#### <span id="cmd_notifyat">notifyat</span>

#### <span id="cmd_pbsnap">pbsnap</span>

#### <span id="cmd_pclist">pclist</span> (List performance counters)

#### <span id="cmd_pdbinfo">pdbinfo</span>

#### <span id="cmd_pssnap">pssnap</span>

#### <span id="cmd_querypc">querypc</span> (Query performance counter)

#### <span id="cmd_reboot">reboot</span>

#### <span id="cmd_rename">rename</span> (Rename file)

#### <span id="cmd_resume">resume</span>

#### <span id="cmd_screenshot">screenshot</span> (Take screenshot)

#### <span id="cmd_sendfile">sendfile</span> (Upload file)

#### <span id="cmd_servname">servname</span>

#### <span id="cmd_setconfig">setconfig</span>

#### <span id="cmd_setcontext">setcontext</span> (Set thread context)

#### <span id="cmd_setfileattributes">setfileattributes</span> (Set file attributes)

#### <span id="cmd_setsystime">setsystime</span> (Set system time)

#### <span id="cmd_setuserpriv">setuserpriv</span> (Set user's privilege level)

#### <span id="cmd_signcontent">signcontent</span>

#### <span id="cmd_stop">stop</span>

#### <span id="cmd_stopon">stopon</span>

#### <span id="cmd_suspend">suspend</span>

#### <span id="cmd_sysfileupd">sysfileupd</span> (Update system file)

#### <span id="cmd_systime">systime</span> (Get system time)

#### <span id="cmd_threadinfo">threadinfo</span> (Get thread information)

#### <span id="cmd_threads">threads</span> (List threads)

#### <span id="cmd_title">title</span>

#### <span id="cmd_user">user</span>

#### <span id="cmd_userlist">userlist</span> (List users)

#### <span id="cmd_vssnap">vssnap</span>

#### <span id="cmd_walkmem">walkmem</span>

#### <span id="cmd_writefile">writefile</span>

#### <span id="cmd_xbeinfo">xbeinfo</span>

#### <span id="cmd_xtlinfo">xtlinfo</span>

See Also
--------

-   [Xbox Neighborhood](/wiki/Xbox_Neighborhood "wikilink") – An XDK tool that
    utilizes the XBDM protocols

External Links
--------------

-   [Yelo
    Neighborhood](https://bitbucket.org/remnantmods/yelo-neighborhood/)
    – An open-source re-implementation of [Xbox
    Neighborhood](/wiki/Xbox_Neighborhood "wikilink") written in C\#.
-   [ViridiX](https://github.com/Ernegien/ViridiX) – An open-source
    collection of Xbox debugging libraries and tools written in C\#.
-   [xbdm-rs](https://github.com/docbrown/xbdm-rs) - An open-source XBDM
    client written in Rust.

