# MoveBytes (Move Bytes in Memory)

The MoveBytes instruction is used to move binary bytes of data into a different memory location.

## Syntax

```
MoveBytes
```

(Destination,DestOffset,Source,SourceOffset,NumBytes,Transfer[optional] )

### Example #1

Converts alittle-endian

**Note:** Endianness refers to byte order. With the little-endian format, bytes are ordered with the least significant byte (the "little end") first. With the big-endian format, bytes are ordered with the most significant byte ("big end") first. The CR300, GRANITE 9, and GRANITE 10 dataloggers use the little-endian format. The CR800, CR1000, CR3000, CR6, CR1000X, and GRANITE 6 use the big-endian format. Byte order when sending string variables as serial data is identical in big-endian and little-endian CSI dataloggers. Only numeric values sent as multiple bytes require attention to big-endian and little-endian issues.

number to abig-endian number with no transfer optional parameter.

```
Public big_endian_num as Long
Public little_endian_num as Long

Dim m, n

BeginProg
'For demonstration, set thelittle-endian number to 16,909,060, which is 01020304 in hexadecimal.
'The movebytes instruction below reserves the byte order to 04030201, which equals 67305985
little_endian_num = 16909060
Scan (1,sec,0,0)
For m=3 to 0 step -1
MoveBytes(big_endian_num,m,little_endian_num,n,1)
n=n+1
Next m
n = 0
NextScan
EndProg
```

### Example #2

Converts alittle-endian

**Note:** Endianness refers to byte order. With the little-endian format, bytes are ordered with the least significant byte (the "little end") first. With the big-endian format, bytes are ordered with the most significant byte ("big end") first. The CR300, GRANITE 9, and GRANITE 10 dataloggers use the little-endian format. The CR800, CR1000, CR3000, CR6, CR1000X, and GRANITE 6 use the big-endian format. Byte order when sending string variables as serial data is identical in big-endian and little-endian CSI dataloggers. Only numeric values sent as multiple bytes require attention to big-endian and little-endian issues.

number to abig-endian number using the optional transfer parameter.

Note: The optional transfer parameter is only available beginning with OS8.

```
Public big_endian_num as Long
Public little_endian_num as Long

Dim m

BeginProg
'For demonstration, set thelittle-endian number to 16,909,060 which is 01020304 in hexadecimal.
'The movebytes instruction below reverses the byte order to 04030201, which equals 6705953
little_endian_num = 16909060
Scan (1,sec,0,0)
'use optional transfer parameter
MoveBytes(big_endian_num,m,little_endian_num,0,4,4)'option4=big-endian 4 byte
NextScan
EndProg
```

## Remarks

TheCR300uses thelittle-endian

**Note:** Endianness refers to byte order. With the little-endian format, bytes are ordered with the least significant byte (the "little end") first. With the big-endian format, bytes are ordered with the most significant byte ("big end") first. The CR300, GRANITE 9, and GRANITE 10 dataloggers use the little-endian format. The CR800, CR1000, CR3000, CR6, CR1000X, and GRANITE 6 use the big-endian format. Byte order when sending string variables as serial data is identical in big-endian and little-endian CSI dataloggers. Only numeric values sent as multiple bytes require attention to big-endian and little-endian issues.

system for storing multi-byte data. When sending serial data to another device it may be necessary to move the serial data to a different memory location if the receiving device uses abig-endian system. The MoveBytes instruction can be used to accomplish this.

String variables can be declared as only one or two dimensions; for example, String(x) or String(x,y). To begin reading or modifying a string at a particular location into the string, enter the location as a third dimension; for example, String(x,y,n) where n is the desired character.

**NOTE:** Offsets in this instruction are zero-based (rather than one-based). Byte order when sending string variables as serial data is identical in big-endian and little-endianCampbell Scientificdataloggers. Only numeric values sent as multiple bytes require attention to big-endian and little-endian issues.

## Parameters

# Destination

The variable in which the moved bytes will be stored.

Type: Variable or Variable Array

# DestOffset (Destination Offset)

The offset into the memory location of the Destination variable, where the binary data will be stored.

Type: Constant or Variable

# Source

The binary data that should be copied into Destination. Note that Source remains unchanged after the move occurs.

Type: Variable or Variable Array

# SourceOffset (Source Offset)

The offset into the memory location of the Source variable, from where the binary data should be read.

Type: Constant or Variable

# NumBytes (Number of Bytes)

The number of bytes that should be placed in the Destination variable.

Type: Constant or Variable

## Optional Parameter

# Transfer

Transfer is an optional parameter that simplifies moving bytes between devices with differentendianess

**Note:** Endianness refers to byte order. With the little-endian format, bytes are ordered with the least significant byte (the "little end") first. With the big-endian format, bytes are ordered with the most significant byte ("big end") first. The CR300, GRANITE 9, and GRANITE 10 dataloggers use the little-endian format. The CR800, CR1000, CR3000, CR6, CR1000X, and GRANITE 6 use the big-endian format. Byte order when sending string variables as serial data is identical in big-endian and little-endian CSI dataloggers. Only numeric values sent as multiple bytes require attention to big-endian and little-endian issues.

. This option is commonly used when creating cross-datalogger platform code for the parsing, or formation, of communication packets. The transfer feature allows specification of the required endianness of the Destination packet and allos the datalogger to automatically perform byte swapping. Note that byte swapping only occurs if the Source endianness is different than the specified Destination endianess.

Valid options are:

| Option      | Description                                              |
| ----------- | -------------------------------------------------------- |
| 0 or absent | No byte swapping (just move the bytes)                   |
| 1           | Destination is little-endian (LE), 2 byte block swapping |
| 2           | Destination is little-endian (LE), 4 byte block swapping |
| 3           | Destination is big-endian (BE), 2 byte block swapping    |
| 4           | Destination is big-endian (BE), 4 byte block swapping    |
