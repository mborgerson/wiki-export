---
title: Kernel/RtlExtendedLargeIntegerDivide
permalink: wiki/Kernel/RtlExtendedLargeIntegerDivide/
layout: wiki
---

RtlExtendedLargeIntegerDivide will call
RtlRaiseStatus(STATUS\_INTEGER\_DIVIDE\_BY\_ZERO) if Divisor = 0.
Otherwise, a custom binary division algorithm is used to perform the
divide operation. The algorithm below was reverse engineered from
original hardware.

LARGE\_INTEGER quotient = Dividend; ULONG local\_remainder = 0; BOOLEAN
carry, remainder\_carry;

for (uint8\_t i = 64; i &gt; 0; i--) {

`   carry = (quotient.QuadPart >> 63) & 0x1;`  
`   remainder_carry = (local_remainder >> 31) & 0x1;`  
`   quotient.QuadPart <<= 1;`  
`   local_remainder = (local_remainder << 1) | carry;`  
`   if (remainder_carry || (local_remainder >= Divisor)) {`  
`       quotient.u.LowPart += 1;`  
`       local_remainder -= Divisor;`  
`   }`

} if (Remainder) {

`   *Remainder = local_remainder;`

}
