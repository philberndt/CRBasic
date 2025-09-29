# TypeOf (Data Type Code)

The TypeOf function returns an integer that represents the data type code of a variable.

## Syntax

Variable = TypeOf(TypeVar)

The following program demonstrates the use of the TypeOf function to return the datatype code of different variables.

```
Public Type(6) as Long
Public RefTemp,Batt_Volt,TypeFloat
Public TypeLong As Long,TypeString As String
Public TypeBool as Boolean

DataTable (Table1,1,9999)
Sample (1,RefTemp,FP2)
Sample (1,Batt_Volt,IEEE4)
EndTable

BeginProg
Scan (1,Sec,0,0)
PanelTemp (RefTemp,4000)
Battery (Batt_Volt)
Type(1) =typeof(TypeFloat)'returns data type code 24
Type(2) =typeof(TypeLong)'returns data type code 20
Type(3) =typeof(TypeString)'returns data type code 11
Type(4)=typeof(TypeBool)'returns data type code 28
Type(5) =typeof(Table1.RefTemp)'returns data type code 7
Type(6) =typeof(Table1.Batt_Volt)'returns data type code 24

CallTable Table1
NextScan
EndProg
```

## Parameters

# TypeVar (Variable Datatype)

The variable for which to return the data type code. If the TypeVar parameter passed in is not a simple variable (_i.e._, is an expression or a constant) then 0 is returned.

See the following list for the data type code returned for different variable or data types. Note that these data type codes are for theCR350dataloggers that use little-endian

**Note:** Endianness refers to byte order. With the little-endian format, bytes are ordered with the least significant byte (the "little end") first. With the big-endian format, bytes are ordered with the most significant byte ("big end") first. The CR300, GRANITE 9, and GRANITE 10 dataloggers use the little-endian format. The CR800, CR1000, CR3000, CR6, CR1000X, and GRANITE 6 use the big-endian format. Byte order when sending string variables as serial data is identical in big-endian and little-endian CSI dataloggers. Only numeric values sent as multiple bytes require attention to big-endian and little-endian issues.

format. Dataloggers that use big-endian format (for example, CR1000X, CR6, GRANITE 6, CR3000, CR1000, CR800) return different data type codes.

| Code | Description                                                                                                                                             |
| ---- | ------------------------------------------------------------------------------------------------------------------------------------------------------- |
| 1    | One-byte unsigned integer                                                                                                                               |
| 7    | FP2 Two-byte floating point in final storage format                                                                                                     |
| 11   | String ASCII string                                                                                                                                     |
| 17   | Bool8- a bit field of 8 bits used for representing ports and flags                                                                                      |
| 20   | Long - 32-bit integer with least significant byte first                                                                                                 |
| 21   | UINT2 Two-byte unsigned integer with least significant byte first                                                                                       |
| 22   | UINT4 Four-byte unsigned integer with least significant byte first                                                                                      |
| 23   | NSec a composite of two Int4 values that represent a time stamp as seconds since midnight 1 January 1990 and nanoseconds into the second, repsepctively |
| 24   | IEEE4 Four-byte floating point in little-endian format                                                                                                  |
| 28   | Boolean 4-byte value where 0 is false and any other value is true                                                                                       |

Type: Variable
