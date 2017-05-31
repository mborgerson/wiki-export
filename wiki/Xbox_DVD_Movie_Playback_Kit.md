---
title: Xbox DVD Movie Playback Kit
permalink: wiki/Xbox_DVD_Movie_Playback_Kit/
layout: wiki
---

by *Rob Reilink*, 3 Mar 2003

Introduction
------------

The DVD-IR remote receiver is a part of the DVD kit which allows you to
view DVDs on your Xbox. It comes together with a remote controller.
Although it may seem just a simple microcontroller device with a
receiver module, there is something more inside which could make it even
more interesting.

Pictures
--------

Ok, let's start with the pictures from the internals:

**Missing image**  
*Dvdirfront.jpg*  
Image:Dvdirfront.jpg  
  
**Missing image**  
*Dvdirback.jpg*  
Image:Dvdirback.jpg  
  

IC's
----

So, what is inside and what does it do?

-   U1 92163
    [STMicroelectronics](https://web.archive.org/web/20100617020513/http://www.st.com/)
    &lt;[Datasheet](https://web.archive.org/web/20100617020513/http://www.st.com/stonline/books/pdf/docs/5521.pdf)&gt;

  
This big square IC on the backside is the microcontroller.
STMicroelectronics describes it as “8/16-BIT FULL SPEED USB MCU FOR
COMPOSITE DEVICES WITH 16 ENDPOINTS, 20K ROM, 2K RAM, I 2 C, SCI, &
MFT”. Since the program resides inside in its ROM, it is almost
impossible to extract the program from inside.

-   U2 TSOP-1556 [Vishay
    Telefunken](https://web.archive.org/web/20100617020513/http://www.vishay.com/)
    &lt;[Datasheet](https://web.archive.org/web/20100617020513/http://www.vishay.com/docs/82029/82029.pdf)&gt;

  
This black box on the middle of the frontside is an integrated IR
receiver. It filters the received infrared pulses and demodulates them.
Its filter frequency is 56kHz, while 38kHz is standard for most remote
controls. Therefore, chances are few other remotes will work with the
Xbox receiver.

-   U3 MX23C4000TC-10
    [Macronix](https://web.archive.org/web/20100617020513/http://www.macronix.com/)
    &lt;[Datasheet](https://web.archive.org/web/20100617020513/http://www.macronix.com/QuickPlace/hq/PageLibrary48256D9D002BA613.nsf/h_6057FA6682A90C3948256DCE0052D2D3/67DCB124F1BE4E7D48256DC50039AC31/$File/MX23C4000-4.2.pdf/?OpenElement)&gt;

  
This wide TSOP IC on the frontside could be the most interesting of all.
It is a 4MBit mask ROM. It is assumed that it contains the DVD player
application. WHY? Why would they not just put this little 512kb max.
application on the harddisk? Why another ROM which contains the program?

<!-- -->

  
One could think it is to allow them to upgrade the application easily,
but the real reason seems to be different: licensing. As the label on
the back notes: “Made under license from Dolby Laboratories”. By
including the software in the DVD Remote kit, they don't have to pay
Dolby for every Xbox sold, but just for every DVD Remote kit sold. This
allows them to keep the cost of the Xbox down.

-   U4 HC574 [Texas
    Instruments](https://web.archive.org/web/20100617020513/http://www.ti.com/)
    &lt;[Datasheet](https://web.archive.org/web/20100617020513/http://focus.ti.com/lit/ds/symlink/sn74hc574.pdf)&gt;

  
This 20-pin standard logic IC is an octal D-flipflop, which splits the
databus from the 92163 to 8 adress bits. This technique is very well
known from the 8051 and other microcontrollers.

Hacking!
--------

As the dashboard presumably downloads the code from the ROM into the
memory of the Xbox, this could be a hardware hack requiring no hardware
modifications. It should be noted though, that the ROM is probably
scrambled. Also, the microcontroller could encrypt the data even more.
As the mask ROM is not a proprietary device, it is known not to contain
any encryption hardware. On the other hand is it quite reasonable to
assume Microsoft also signed this piece of code, and the dashboard might
refuse to run it if it is not signed.
