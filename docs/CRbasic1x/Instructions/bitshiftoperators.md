# Bit Shift Operators

The bit shift operators (>> or <<) perform an arithmetic bit shift operation on a numeric expression.

## Syntax

Variable = Numeric Expression>>Amount

The following program shows the use of the right bit shift operator (>>).

```
Public NumEx AS LONG, Shift(8) AS LONG
```

BeginProg
NumEx = 2048
Scan (1,Sec,3,0)
Shift(1)=NumEx>>1
Shift(2)=NumEx>>2
Shift(3)=NumEx>>3
Shift(4)=NumEx>>4
Shift(5)=NumEx>>5
Shift(6)=NumEx>>6
Shift(7)=NumEx>>7
Shift(8)=NumEx>>8
NextScan
EndProg

```

## Remarks


>> shifts the bit pattern to the right.

<< shifts the bit pattern to the left.

The Amount argument is the number of bits to shift left or right. Amount must be an integer.

This datalogger uses the big-endian

**Note:** Endianness refers to byte order. With the little-endian format, bytes are ordered with the least significant byte (the "little end") first. With the big-endian format, bytes are ordered with the most significant byte ("big end") first. The CR300, GRANITE 9, and GRANITE 10 dataloggers use the little-endian format. The CR800, CR1000, CR3000, CR6, CR1000X, and GRANITE 6 use the big-endian format. Byte order when sending string variables as serial data is identical in big-endian and little-endian CSI dataloggers. Only numeric values sent as multiple bytes require attention to big-endian and little-endian issues.

system for storing multi-byte data.

**NOTE:** TheLong

**Note:** Data type used when declaring a variable as an integer.

data type is a signed integer. Shifting the bit pattern to the right maintains the same sign (i.e., the most significant bit is maintained as a 1 if the number is a negative). An [AND](and.md) operation can be performed to strip the unwanted bits for an unsigned integer.
```
