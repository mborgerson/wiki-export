---
title: PIC
permalink: wiki/PIC/
layout: wiki
---

Retrieved from
[1](https://web.archive.org/web/20100617022549/http://www.xbox-linux.org/wiki/PIC)

by *Georg Lukas, Michael Steil, Jeff Mears*, 26 June 2002

Little is known about how to program the PIC. Basically, the PIC is an
independent 16 bit microprocessor with its own RAM and I/O lines,
connected to the CPU through the SMBus. Its own firmware makes it
possible for the PIC to be talked to on a very high level. Flashing of
the LED, for instance, is completely controlled by the PIC.

Command overview
----------------

|             |                                                      |
|-------------|------------------------------------------------------|
| **Command** | **Description**                                      |
| 0x01        | PIC version string                                   |
| 0x03        | tray state                                           |
| 0x04        | A/V Pack state                                       |
| 0x09        | CPU temperature (C)                                  |
| 0x0A        | board temperature (C)                                |
| 0x0F        | reads scratch register written with 0x0E             |
| 0x10        | current fan speed (0~50)                             |
| 0x11        | interrupt reason                                     |
| 0x18        | reading this reg locks up xbox in “overheated” state |
| 0x1B        | scratch register for the original kernel             |
| 0x1C        | random number for boot challenge                     |
| 0x1D        | random number for boot challenge                     |
| 0x1E        | random number for boot challenge                     |
| 0x1F        | random number for boot challenge                     |

  
  
**Writing:**  

|             |                                                             |
|-------------|-------------------------------------------------------------|
| **Command** | **Description**                                             |
| 0x01        | PIC version string counter reset                            |
| 0x02        | reset and power off control                                 |
| 0x05        | power fan mode (0 = automatic; 1 = custom speed from reg    
                                                               
  0x06)                                                        |
| 0x06        | power fan speed (0..~50)                                    |
| 0x07        | LED mode (0 = automatic; 1 = custom sequence from reg 0x08) |
| 0x08        | LED flashing sequence                                       |
| 0x0C        | tray eject (0 = eject; 1 = load)                            |
| 0x0E        | another scratch register? seems like an error code.         |
| 0x19        | reset on eject (0 = enable; 1 = disable)                    |
| 0x1A        | interrupt enable (write 0x01 to enable; can't disable once  
                                                               
  enabled)                                                     |
| 0x1B        | scratch register for the original kernel                    |
| 0x20        | response to PIC challenge (written first)                   |
| 0x21        | response to PIC challenge (written second)                  |

PIC version string
------------------

The PIC version can be read from register 0x01 as a 3-character ASCII
string. Each time you read that register, the next of the 3 characters
is returned. It repeats after the third read. Also, the counter can be
reset to the first letter by writing 0x00 to this register. Some Xboxes
have the letters “SMC P01” physically etched into the top of the PIC,
and some have it on a sticker. The P01 is the version number. The
following table shows known PIC versions:

  

|             |          |                 |
|-------------|----------|-----------------|
| **Version** | **Hex**  | **Description** |
| P01         | 50 30 31 | retail xbox 1.0 |
| P05         | 50 30 35 | retail xbox 1.1 |
| DXB         | 44 58 42 | debug kit       |

Reset and Power Off
-------------------

The PIC controls the power system of the Xbox. To reset or power off the
system, you write a request to register 0x02. Since the PIC takes some
time to respond, you should do a cli/hlt loop after writing to the PIC.
These are the values you can use (this is NOT a bitfield):

|           |                                                                |
|-----------|----------------------------------------------------------------|
| **Value** | **Description**                                                |
| 0x01      | resets the system and all hardware                             |
| 0x40      | power cycle: powers off then on (also clears scratch register) |
| 0x80      | power off                                                      |

The AV Pack
-----------

The A/V connector on the xbox rear contains several pins to detect the
kind of attached cable. In the PIC there is a register (0x04), where
this information can be read out by software. The following values are
known:

|           |                      |
|-----------|----------------------|
| **Value** | **Description**      |
| 0x00      | SCART                |
| 0x01      | HDTV                 |
| 0x02      | VGA                  |
| 0x03      | RFU                  |
| 0x04      | S-Video              |
| 0x05      | undefined            |
| 0x06      | standard             |
| 0x07      | missing/disconnected |

The LED
-------

The PIC has its own timer to flash the LEDs: The LED color goes through
four phases, 0 to 3. In each phase, the color of the LED can be
different. The LED can be off, red, green, or, by turning on both red
and green, orange. All the CPU has to do to make the PIC control the LED
in a specified way is to transmit a byte containing the color sequence
to it.

The PIC, by default, manages the LEDs itself. While the system is on,
the LED normally is solid green. But when you hit eject, the PIC causes
the LED to flash green. To override this, and control the LEDs as
desired, you must write 0x01 to register 0x07 after writing your color
sequence to 0x08.

Each phase consists of two bits, one for red and one for green, so four
phases fit into a byte. Thus, the control byte looks as follows:

|            |                 |
|------------|-----------------|
| **Bit \#** | **Description** |
| 7          | Red (Phase 0)   |
| 6          | Red (Phase 1)   |
| 5          | Red (Phase 2)   |
| 4          | Red (Phase 3)   |
| 3          | Green (Phase 0) |
| 2          | Green (Phase 1) |
| 1          | Green (Phase 2) |
| 0          | Green (Phase 3) |

To set an LED code, the command code 0x08 and the LED code as data must
to be sent to I2C device 0x20, followed by the command code 0x07 with a
0x01 as data, both in byte mode.

     SMBusWriteCommand(0x20, 8, false, states);
     SMBusWriteCommand(0x20, 7, false, 1);

  
These are some example settings:

|          |            |                                     |
|----------|------------|-------------------------------------|
| **Code** | **Binary** | **Description**                     |
| 0x00     | 00000000   | Off                                 |
| 0xF0     | 11110000   | Solid red                           |
| 0x0F     | 00001111   | Solid green                         |
| 0xFF     | 11111111   | Solid orange                        |
| 0xC0     | 11000000   | Slow red flashing                   |
| 0xA0     | 10100000   | Fast red flashing                   |
| 0x63     | 01100011   | Off - red - orange - green flashing |

You can use the blink tool available from CVS as a frontend for this:

1.  blink ABCD

  
ABCD are the LED states for the four phases, possible values for every
state are:

|           |                    |
|-----------|--------------------|
| **Value** | **Description**    |
| r         | red                |
| g         | green              |
| o         | orange (red+green) |
| x         | off                |

If you want to make it slowly switch red/green, run blink rrgg, fast
orange blinking can be achieved with blink oxox.

Interrupt reasons
-----------------

The PIC sets the interrupt reason register (0x11) with a bitmask of the
following values to indicate to the operating system why the interrupt
occurred:

|           |                      |
|-----------|----------------------|
| **Value** | **Description**      |
| 0x01      | power button pressed |
| 0x10      | A/V Pack removed     |
| 0x20      | eject button pressed |

Scratch register values
-----------------------

The original kernel uses this register (0x1B) to “remember” some
information across a reboot, and interprets its contents after
rebooting. The PIC itself probably makes no interpretation of this
register. The register is a bitmask with the following values:

|           |                                  |
|-----------|----------------------------------|
| **Value** | **Description**                  |
| 0x01      | eject after boot                 |
| 0x02      | display error message after boot |
| 0x04      | skip boot animation              |
| 0x08      | run dashboard no matter what     |

PIC Challenge (regs 0x1C~0x21)
------------------------------

To try to thwart any attempt to hijack the entire boot process by
bypassing the MCPX, the PIC does a security check on the booting ROM.
After reset, the PIC gives the CPU about 250 ms to respond to a
cryptographic challenge before the system is reset. The PIC generates
four random(?) bytes, read from registers 0x1C~0x1F. To disable the
countdown, you hash these four bytes and write the result to 0x20 and
0x21. Details of this process are given
[here](https://web.archive.org/web/20100617022549/http://www.xbox-linux.org/docs/pichandshake.html)
.

On a “Debug Kit” (PIC version “DXB”), registers 0x1C~0x1F all return a
value of 0x00. It is unknown whether the challenge system is active at
all on these machines.

<hr />
(original version:
[2](https://web.archive.org/web/20100617022549/http://www.xbox-linux.org/docs/smc.html))
