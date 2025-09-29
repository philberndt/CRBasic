# TypeVar (Variable Datatype)

The variable for which to return the data type code. If the TypeVar parameter passed in is not a simple variable (_i.e._, is an expression or a constant) then 0 is returned.

See the following list for the data type code returned for different variable or data types. Note that these data type codes are for theCR1000Xdataloggers that use big-endian

**Note:** Endianness refers to byte order. With the little-endian format, bytes are ordered with the least significant byte (the "little end") first. With the big-endian format, bytes are ordered with the most significant byte ("big end") first. The CR300, GRANITE 9, and GRANITE 10 dataloggers use the little-endian format. The CR800, CR1000, CR3000, CR6, CR1000X, and GRANITE 6 use the big-endian format. Byte order when sending string variables as serial data is identical in big-endian and little-endian CSI dataloggers. Only numeric values sent as multiple bytes require attention to big-endian and little-endian issues.

format. Dataloggers that use little-endian format (for example, CR300, CR350, GRANITE 9, GRANITE 10) return different data type codes.

| Code | Description                                                                                                                                            |
| ---- | ------------------------------------------------------------------------------------------------------------------------------------------------------ |
| 1    | UINT1 One-byte unsigned integer                                                                                                                        |
| 2    | UINT2 Two-byte unsigned integer with most significant byte first                                                                                       |
| 3    | UINT4 Four-byte unsigned integer with most significant byte first                                                                                      |
| 6    | Long - 32-bit integer with most significant byte first                                                                                                 |
| 7    | FP2 - Two-byte floating point in final storage format                                                                                                  |
| 9    | IEEE4 - Four-byte floating point in big-endian format                                                                                                  |
| 11   | String - ASCII string                                                                                                                                  |
| 14   | NSec a composite of two Int4 values that represent a time stamp as seconds since midnight 1 January 1990 and nanoseconds into the second, respectively |
| 17   | Bool8- a bit field of 8 bits used for representing ports and flags                                                                                     |
| 18   | IEEE8 Eight-Byte double-precision, 64-bit floating-point value                                                                                         |
| 28   | Boolean 4-byte value where 0 is false and any other value is true                                                                                      |

Type: Variable
