---
title: Hard Drive
permalink: wiki/Hard_Drive/
layout: wiki
---

The original Xbox hard disk drive was 8 GB in size. Later releases, 10
GB drives; however, only the first 8 GB of the drive was used. See
[Hardware Revisions](/wiki/Hardware_Revisions "wikilink") for more
information.

Partitions
----------

The Xbox hard disk contains multiple partitions. Unlike a PC, which
typically contains either a [Master Boot
Record](https://en.wikipedia.org/wiki/Master_boot_record) or [GUID
Partition Table](https://en.wikipedia.org/wiki/GUID_Partition_Table) to
specify the partition information, the Xbox kernel uses a fixed
partition layout. The file system used on the Xbox is
[FATX](/wiki/FATX "wikilink"), a variant of FAT16/32 developed by Microsoft
specifically for the Xbox.

| Drive Letter | Description | Offset (bytes) | Size (bytes) | Filesystem      | Device Object (MS Retail Kernel) |
|--------------|-------------|----------------|--------------|-----------------|----------------------------------|
| N/A          | Config Area | 0x00000000     | 0x00080000   | Fixed Structure | N/A                              |
| X            | Game Cache  | 0x00080000     | 0x2ee00000   | FATX            | \\Device\\Harddisk0\\Partition3  |
| Y            | Game Cache  | 0x2ee80000     | 0x2ee00000   | FATX            | \\Device\\Harddisk0\\Partition4  |
| Z            | Game Cache  | 0x5dc80000     | 0x2ee00000   | FATX            | \\Device\\Harddisk0\\Partition5  |
| C            | System      | 0x8ca80000     | 0x1f400000   | FATX            | \\Device\\Harddisk0\\Partition2  |
| E            | Data        | 0xabe80000     | 0x131f00000  | FATX            | \\Device\\Harddisk0\\Partition1  |

  
  
*side note: CD/DVD Drive “D:” &lt;=&gt; “\\Device\\CdRom0”*

*and usually: added Drive “F:” &lt;=&gt;
“\\Device\\Harddisk0\\Partition6”*

  
  
   *added Drive “G:” &lt;=&gt; “\\Device\\Harddisk0\\Partition7”*

**Debug/Devkit HDD:**

| Drive Letter | Description | Offset (bytes) | Size (bytes) | Filesystem | Device Object (MS Retail Kernel) |
|--------------|-------------|----------------|--------------|------------|----------------------------------|
| \[FIXME\]    | \[FIXME\]   | \[FIXME\]      | \[FIXME\]    | \[FIXME\]  | \[FIXME\]                        |
||

**FIXME:**

-   Add info on how extended partitions are added.

Locking
-------

The hard drives in the Xbox are locked with a key which is unique to the
specific Xbox. The drive is unlocked by the kernel at boot.

### Unlocking for Backups

Before connecting an Xbox HDD to a PC for a backup or modification, the
drive must first be unlocked. This can be done with alternative
dashboards (such as EvoX). But beware, once you unlock the disk you
cannot use it with an official BIOS until you re-lock the disk! For this
reason it is suggested to use a patched BIOS which does not require the
disk to be locked. If you are unable to run unsigned code (needed to
unlock the HDD before powering off), it is possible to hot-swap the
drive after the Xbox has started. This is not a suggested method, but it
has been known to work. The idea is that you start the Xbox and wait for
the dashboard, at which point the drive will be unlocked. Then, while
the Xbox is running, you disconnect the IDE cable (but not the power!),
and then connect the drive to your PC. Then the drive can be mounted for
read/write (using XboxHDM), or imaged directly.

**FIXME:**

-   Provide more info on locking/unlocking procedure.
-   Provide details about the key and how it can be derived from the
    [EEPROM](/wiki/EEPROM "wikilink") data.

How To: Backup an HDD
---------------------

There are two general methods to back up your HDD: copying the files, or
creating a byte-for-byte image of the drive.

### Method 1: File Copy

This is an acceptable backup method, but it is not as accurate an exact
copy. This method requires less work to create the backup, but more work
to re-create a usable disk image. The dashboard files (found in C:) are
the most essential part of a backup, and a complete disk image can be
re-created (with some effort) with a copy of the dashboard files using a
tool such as XboxHDM.

#### Remote

Simply run an XBE on your Xbox that provides an FTP server. This is a
standard feature for alternative dashboards (such as EvoX). Then connect
to your Xbox from another system and copy all files in **C:** and
**E:**.

#### Direct

Unlock the HDD, connect it to your PC, mount the drive (see
[FATX](/wiki/FATX "wikilink")), copy the files.

### Method 2: Exact Copy

This is the most accurate method to backup your hard disk. This method
requires more work to create the backup, but does not require any effort
to create a usable disk image like the first method. There are multiple
ways to implement this method, one is provided here.

Unlock the HDD, connect it to your PC using a USB-IDE adapter
([available for
~$20USD](https://www.amazon.com/Sabrent-USB-DSC9-SATA-Drive-Converter/dp/B00DQJME7Y)).
In GNU/Linux and other \*NIX variants, DD can be used to perform the
block copy. For example: `sudo dd if=/dev/sdb of=xbox_hdd.raw bs=512`.
append `status=progress` to see the progress during copying if you run a
recent distro, like so:
`sudo dd if=/dev/sdb of=xbox_hdd.raw bs=512 status=progress`.If you're
dumping an original Xbox HDD (capacity 8G or 10G), this will finish
pretty quickly. The files can be extracted by mounting the filesystems
in the image (see [FATX](/wiki/FATX "wikilink")).

Further Reading
---------------

-   [Xbox Partitioning and Filesystem
    Details](http://hackipedia.org/Disk%20formats/Partition%20tables/X-Box/Xbox_Partitioning_and_Filesystem_Details.htm)

