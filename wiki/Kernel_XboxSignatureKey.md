---
title: Kernel/XboxSignatureKey
permalink: wiki/Kernel/XboxSignatureKey/
layout: wiki
---

This is used to find the Xbox Public RSA-2048 Key. This key is used to
verify the signed [Xbe](/wiki/Xbe "wikilink") executables.

`struct RSA_PUBLIC_KEY`  
`{`  
`   char Magic[4];          // `“`RSA1`”  
`   unsigned int Bloblen;       // 264 (Modulus + Exponent + Modulussize)`  
`   unsigned char Bitlen[4];    // 2048`  
`   unsigned int ModulusSize;   // 255 (bytes in the Modulus)`  
`   unsigned char Exponent[4];`  
`   unsigned char Modulus[256];     // Bit endian style`  
`   unsigned char Privatekey[256];  // Private Key -- Big endian style`  
`};`  
`struct RSA_PUBLIC_KEY xePublicKeyData;`

XBE signing process
-------------------

The XBE signing process consists of two stages:

1. Calculate a SHA-1 hash of the contents of each of the XBE sections
and store it in a table in the XBE header.

2. Calculate a SHA-1 hash of the XBE header, except for the *magic*
'XBEH' value and the *digital signature* field. The hash is then
encrypted with a 2048-bit RSA private key. The signature of all official
titles has been encrypted with The resulting encrypted hash *signature*
stored near after the XBE header XBEH *magic* field, in the *digital
signature* field.

XBE signature verification
--------------------------

When an Xbox verifies an executable, a similar process is followed:

1. All the XBE section hashes are recalculated and compared to the ones
stored in the XBE header section table. If these don't match,
verification fails.

2. Calculate a SHA-1 hash of the XBE header, except for the *magic*
'XBEH' value and the *digital signature* field.

3. The *digital signature* field from the XBE is decrypted with the
2048-bit RSA public key and compared to the SHA-1 hash of the XBE
header. Finally, if the two values match, the game is verified.

References and links
--------------------

-   [Xbedump
    source](https://github.com/xqemu/xbedump/blob/master/xboxlib.c)

