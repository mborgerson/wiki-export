---
title: Xbox Savegame System
permalink: wiki/Xbox_Savegame_System/
layout: wiki
---

Retrieved from
[1](https://web.archive.org/web/20100617021545/http://www.xbox-linux.org/wiki/Xbox_Savegame_System)

The Xbox savegame management system is a simple organizational system
that allows users to manage their saved games, downloaded contents from
Xbox LIVE, and copy savegames to memory units.

Overview
--------

The Xbox savegame system uses folders and metadata to organize savegames
according to its game, type, and name (in that order). All
game-generated data are stored in either E:\\UDATA or E:\\TDATA. The
UDATA folder is generally used to store gamesaves (user data), and the
TDATA folder is generally used to store LIVE contents and other
configurations (such as downloaded levels, high-score tables, game
updates, and other miscellaneous configurations). These are the
universal elements of gamesaves:

-   A title meta (universal)
-   A title image (universal)
-   A save title
-   A save image

Saves are nested individually inside the game's folder, which are nested
inside the game-data folders (UDATA and TDATA). Every XBE that had ever
run will have a folder for itself in both of the game-data folders
(however, the Xbox Dashboard is a special case).

Structure Layout
----------------

Here's a typical layout of a the savegame folders, shown with Splinter
Cell and Xbox Dashboard Music.

    +-E:
      +-TDATA
      | +-5553000c
      | | +-audiovideo.par
      | | +-contentimage.xbx
      | +-fffe0000
      |   +-music
      |     +-ST.DB
      +-UDATA
        +-5553000c
          +-8E6AA806E588
          | +-GMMAN_Profile.sg1
          | +-GMMAN_Profile.sg2
          | +-GMMAN_Profile.sg3
          | +-Profile.inf
          | +-SaveMeta.xbx
          +-SaveImage.xbx
          +-TitleImage.xbx
          +-TitleMeta.xbx

As you can see, in the top level there are two folders, TDATA and UDATA.
Under each of those folders are folders named according to the game's ID
(for example, Splinter Cell's ID is 5553000c). They are in both TDATA
and UDATA folders.

### UDATA Folder

The UDATA folder is generally used to store user content, such as save
games, settings, and user-created customizations. Under this folder are
folders for each individual game, named by their ID number. Under every
game folder, there are two files: TitleImage.xbx and TitleMeta.xbx.

#### TitleImage.xbx

TitleImage.xbx is a 128\*128px Xbox texture, used for displaying the
game's logo at the left of the game's section in the Memory browser. If
this file is missing or corrupted, a generic Xbox logo will be displayed
in its place.

#### TitleMeta.xbx

TitleMeta.xbx is a text file for indicating what title of the game would
be displayed in the Memory browser. The title is taken from the embedded
title of the executable that created the game folder (ie. if you run
Splinter Cell's default.xbe first, the title will be “Splinter Cell”. If
you run its downloader.xbe (same game ID), the title will be
“Downloader”). If this file is missing or corrupted, the game's title
will be displayed as “Unknown Title”. The structure for TitleMeta.xbx
is:

    TitleName=Game Name

In the case of Splinter Cell, it is `TitleName=Splinter Cell`.

##### Localization

The TitleMeta.xbx file may contain localization:

    [default]
    TitleName=Network Configurations
    [EN]
    TitleName=Network Configurations
    [JA]
    TitleName=ネットワーク設定データ
    [DE]
    TitleName=Daten für Netzwerkeinstellungen
    [FR]
    TitleName=Données des paramètres réseau
    [ES]
    TitleName=Datos de configuración de red
    [IT]
    TitleName=Dati impostazioni di rete
    [KO]
    TitleName=네트워크 설정 데이터
    [TW]
    TitleName=網路設定資料
    [BR]
    TitleName=Dados de Configurações da Rede

There is a separate TitleName entry listed under each of the languages.
The entry under `[default]` will be used if a localized game title
cannot be found in this file for the current system language.

### Gamesaves

Each individual gamesave or game profile is under the game's folder in
the UDATA folder. Each save or profile has its own folder. The name of
the folders differs from game to game, as there is not set format for
the name of a save folder. Ie. one game may choose to name its save
folder as “001aecf7”, another game may name the save folder as
“Profile01”. There is always a SaveMeta.xbx file under the folder, and
there may also be a SaveImage.xbx file there. Each save folder may
contain whatever data that is needed for a valid save.

#### SaveMeta.xbx

SaveMeta.xbx is very similar to TitleMeta.xbx. This defines the name of
a save and is displayed on the left side of the screen when the save is
selected. If this file is missing or corrupted, the gamesave's name will
be displayed as “Corrupted Save”. Its format is as the following:

    Name=Save Name

In the case of a Splinter Cell game save, it is something like
`Name=GMMAN_Profile`.

#### SaveImage.xbx

SaveImage.xbx is a 64\*64px Xbox texture, used to represent an
individual save folder. It'll be located in the game's folder under the
UDATA folder if all of the saves use the same image (to avoid
unnecessary duplication), or under each individual save folder, if there
are different kinds of saves located in each folder. The gamesave
folder's SaveImage.xbx has precedence over the game folder's
SaveImage.xbx. If this file is missing or corrupted, a generic Xbox logo
will be in its place.

Universal save image:

    +-UDATA
      +-5553000c
        +-SaveImage.xbx

Individual save image:

    +-UDATA
      +-4d530013
        +-129E1A3F1B2A
          +-SaveImage.xbx

### TDATA Folder

The TDATA folder is similar in structure to the UDATA folder. However,
it is used to store Xbox LIVE contents, game configurations, and other
miscellaneous data. With the TDATA game folders, there is no specific
structure, so games may store whatever file it needs wherever inside its
game folder.

#### contentimage.xbx

Contentimage.xbx is used in the same way as SaveImage.xbx, except that
it is used specifically to indicate downloaded contents.

Special Case of Xbox Dashboard
------------------------------

The Xbox Dashboard does not abide by these structures. For one, it
doesn't have a UDATA folder, and in the TDATA folder it doesn't have any
.xbx files. However, in its TDATA folder (E:\\TDATA\\fffe0000), there is
a folder named `music`. Inside it is a file named `ST.DB` and several
folders with hexadecimal names. This is where soundtracks are stored.

If you go into the LIVE Dash's “Test Connections” screen and press the
Black button, a folder that follows the structure will be created under
the game folder fffe0000 under the UDATA folder. This contains
diagnostic information about the connection to Xbox LIVE.

Other Occurrences in the UDATA and TDATA Folders
------------------------------------------------

-   Under the Dashboard's TDATA game folder, a blank file named
    &lt;code&gt;NoisyCamera&lt;/code&gt; may be present. This is a
    marker for an Easter egg on the main menu of the Dashboard. Toggle
    it the same way as the full-screen visualization egg.
-   Under the UDATA folder, there may be a file named
    &lt;code&gt;NICKNAME.XBN&lt;/code&gt;. This stores the Xbox's
    nickname for online games.

Application to Xbox-Linux
-------------------------

One way the Xbox-Linux project can take advantage of the gamesave system
is by having a all Xbox-Linux distributions reside inside an Xbox-Linux
game folder. Each individual distribution would be a “gamesave.” For
example, suppose that the Xbox-Linux project uses the game ID `ffffff00`
and that we're using Xebian as the distro. Here would be the layout of
the files following the gamesave structure:

    +-E:
      +-TDATA
      | +-ffffff00
      |   +-linuxboot.cfg
      +-UDATA
        +-ffffff00
          +-debian
          | +-initrd
          | +-rootfs
          | +-SaveImage.xbx
          | +-SaveMeta.xbx
          | +-swap
          | +-vmlinuz
          +-TitleImage.xbx
          +-TitleMeta.xbx

Xromwell would search the gamesave directories for linuxboot.cfg and
have a special method of handling of a special tag for the gamesave
system, so that the following would point to the Xebian files in the
UDATA structure:

    title Xebian
    kernel&nbsp;!gs/debian/vmlinuz
    initrd&nbsp;!gs/debian/initrd
    append init=/linuxrc root=/dev/ram0 kbd-reset xbox=fatx_e:/UDATA/ffffff00/debian ramdisk_blocksize=4096 
    xboxfb y

Contents of SaveMeta.xbx:

    Name=Xebian

Contents of TitleMeta.xbx:

    TitleName=Xbox-Linux

SaveImage.xbx would show the Debian logo, and TitleImage.xbx would show
Tux.

With this arrangement, it would be easy to uninstall or copy Xbox-Linux
distributions simply by selecting its icon in the Memory browser. To
completely get rid of Xbox-Linux distributions, just select the Tux
icon. Distribution on a specially formatted USB drive would also be
possible: create a pre-installed Linux image with a FATX filesystem, and
then `dd` it onto the flash drive.
