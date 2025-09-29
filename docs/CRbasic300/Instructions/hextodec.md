# HexToDec (Convert Hexadecimal String)

The HexToDec function is used to convert a hexadecimal string to a float or integer.

## Syntax

_Variable_ = HexToDec(Expression)

In the following example, a value entered into the Expression variable is converted into a hexadecimal value by the Hex function. The [HexToDec](#) function is then used to convert the hexadecimal string back to a decimal value.

```
'Example Program
'Use Table Monitor or Numeric Display to enter value for Expression variable

Public Expression As Long, Hexadecimal As String, Decimal

BeginProg
Scan (1,Sec,3,0)
'Convert Expression to Hexadecimal
Hexadecimal =Hex(Expression)
'convert Hexadecimal to Decimal
Decimal =HexToDec(Hexadecimal)
NextScan
EndProg
```

## Remarks

The HexToDec function can be used to return the decimal representation of Expression into a variable.

Conversion from a hexadecimal string to a decimal value can also be accomplished by prefacing any hexadecimal string with &H.

## Parameter

# Expression

The expression on which to perform the Decimal to Hex or Hex to Decimal conversion.

Type: String (for HextoDec) or Decimal Value (for Hex)
