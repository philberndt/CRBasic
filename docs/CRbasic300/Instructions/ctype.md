# CType (Cast to Data Type)

The CType function is used to cast an expression into a specified data type.

## Syntax

CType(Expression,Type)

In the following test program, elements of a String variable array are used in expressions that return different values based on the data type specified with the Ctype function. .

```
Public Type(6) As String

BeginProg
Type(1) = 3 + "3" + 3'returns 333
Type(2) = 3 +CType("3",Long) + 3'returns 9
Type(3) =CType("3",String) +CType(3,String) + "3"'returns 333
Type(4) = 123456789 + 1.0'returns 1.234568e8
Type(5) = 123456789 +CType(1.0,Long)'returns 123456790
Type(6) = 123456789 +CType(1.0,DOUBLE)'returns 123456790
EndProg
```

## Remarks

This function returns the Expression as the data type specified by the Type parameter.

## Parameters

# Expression

The Expression parameter is the value or string that should be cast into the specified data type.

# Type

Valid data types for the Type parameter areFloat

**Note:** Four-byte floating-point data type. Default datalogger data type for Public or Dim variables. Same format as IEEE4.

(orIEEE4

**Note:** Four-byte, floating-point data type. IEEE Standard 754. Same format as Float.

),Long

**Note:** Data type used when declaring a variable as an integer.

,String

**Note:** A data type used when declaring a variable consisting of alphanumeric characters.

, or Double.
