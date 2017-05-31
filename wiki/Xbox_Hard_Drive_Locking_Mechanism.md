---
title: Xbox Hard Drive Locking Mechanism
permalink: wiki/Xbox_Hard_Drive_Locking_Mechanism/
layout: wiki
---

by *SpeedBump* (original version: 13 August 2002)

The hard drive in the MS Xbox(tm) is a standard ide drive, which
implements a rarely used security feature to restrict access to its
data. This document will describe in full the security features, and the
algorithms required to access the data on an xbox drive.

The IDE (ATA) commands
----------------------

The ATA spec defines a feature subset which allows for the user to limit
access to the drive's data behind a hardware implemented locking
mechanism. There are several commands in the SECURITY feature subset,
but the command of most interest is the SECURITY\_UNLOCK command.

SECURITY\_UNLOCK requires that the user provide one of two 32 byte
passwords, either a user or master password. The Xbox uses the user
password. Details on the data formats and timings for the data to be
sent to the ide drive can be found in the ata specs (see
<https://web.archive.org/web/20100617023052/http://www.t13.org/>).

The password
------------

The drive password is generated in two distinct phases. The first phase
extracts a key (referred to as the HDKey) from the eeprom data on the
Xbox. The HDKey is unique to each Xbox making this first phase dependant
only on the Xbox eeprom of the unit. The second phase uses this HDKey to
generate a password which is specific to the drive being unlocked (keyed
to the model and serial numbers of the drive).

Drive Data
----------

During the second phase, the serial and model numbers are needed. These
values are available in the response data from the DEVICE\_IDENTITY ata
command. However, the data needs to be properly reorganized. It is read
in big endian words, and needs to be byte swapped first to get the byte
ordering correct. Then, starting from the end of the data (serial == 20
bytes, model == 40 bytes) ignore ASCII spaces (byte value of 0x20) at
the end of the data. Zeros are \*not\* trimmed, \*only\* spaces. Do not
be fooled into believing that this data is a string. On some drives this
is the case, but on others there are non-ascii values in the fields.

Basic Security algorithms
-------------------------

There are two primary crytography routines needed when generating an
XBox drive password, SHA1 and RC4.

SHA1 is a hashing algorithm. It's primary purpose is to take an input
message and create a (relatively) small signature (called a digest)
which is unique to the original message. One of the goals of SHA1 is to
make it difficult to alter the input message in such a way as to result
in the same output digest.

RC4 is a symmetric cipher. This means that the algorithm for encryption
is the same as that for decryption. The purpose is to make one key work
in both directions.

There is an algorithm called HMAC which uses a hashing algorithm (in
this case SHA1) to generate a cryptographically “strong” signature. I'm
sure there is a mathematical basis for this, but I'm not willing to try
to understand it :)

The Password Algorithm
----------------------

(some syntax notes, key data is shown entering functions from the side,
data is shown entring from above or below, in order of presentation from
left to right)

                                           RC4_key &gt;--(second)--&gt;--,
                                             /|\                   |
                                              |                    |
     .-&lt;--|__eeprom_key__|--&gt;-----------&gt; HMAC_SHA1                |
     |                                       /|\                   |
     |                                        |                    |
     |                        .---&gt;-----------'                    |
     |                        |                                    |
     |  eeprom_data = |__data_hash___|__enc_conf__|__enc_data__|   |
     |                        |             |           |          |
     |                        |            \|/          |          |
     |                        |        rc4_decrypt &lt;----|---------&lt;|
     |                        |             |           |          |
     |                       \|/            |           |          |
     |                 (must be equal)      |          \|/         |
     |                       /|\            |      rc4_decrypt &lt;---'
     |                        |             |           |
     |                        |            \|/         \|/
     |                        |      |_confounder_|____data____|
     |                        |       /            /    |
     |                        |      /            /     |
     |                        |     /            /      |
     |                        |    /            /       |
     |                        |   \|/          /       \|/
     `---&gt;-----------------&gt; HMAC_SHA1        /   |__HDKey__|__|
                                    /|\      /         |
                                     \______/          |
                                                       |
                   .-------------------------&lt;--------'
                   |
                   |              model_number   serial_number
                   |                      \        /
                   |                       \      /
                   `---&gt;-----------------&gt; HMAC_SHA1
                                               |
                                              \|/
                                          HD_password

  
This seems to be the easiest way to show the required calculations.

Basically there are several intermediate steps. First, generate the
RC4\_key from the eeprom\_key and the data\_hash (first 20 bytes of
eeprom\_data). Use the RC4\_key to decrypt the encrypted confounder (8
bytes 20 bytes into eeprom\_data) and the encrypted data (20 bytes 28
bytes into eeprom\_data). Now generate an HMAC\_SHA1 hash from the
eeprom\_key and the decrypted confounder and data. Verify that this hash
matches the data\_hash stored in the eeprom. If they don't match then
the eeprom data is not correct. If the hashes match then the first 16
bytes of the decrypted data field is the HDKey.

Once you have the HDKey get the model number and serial number from the
ide drive. Generate an HMAC\_SHA1 hash from the HDKey, model and serial
numbers. The resulting 20 bytes are the HD password. The remaining 12
bytes needed for the password are zeros.

Remaining Questions
-------------------

The algorithm is well known, however it is dependant on the eeprom\_key.
It would be ideal if this key could be compiled into a driver to perform
the generation and the unlocking. However noone appears to be able to
answer the question of legality. Is it legal to privide the eeprom key?
Either way, the drive can be unlocked. The ability to distribute the key
will only help people use the drive outside the xbox (plus make it
simpler to unlock the drive in the xbox).
