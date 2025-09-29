# Hex (Convert to Hexadecimal)

The Hex function returns a hexadecimal string representation of value of type Long. The input is converted to a Long data type before it is converted to hexadecimal.

## Syntax

_Variable =_ Hex(Expression)

### Example #1

In the following example, a value entered into the Expression variable is converted into a hexadecimal value by the Hex function. The [HexToDec](hextodec.md) function is then used to convert the hexadecimal string back to a decimal value.

This program can be tested by using the Numeric Monitor to enter the value for Expression.

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

### Example #2

```
Public FloatVar = 0.01
Public Moved As Long
Public LongVar As Long = 100
Public HexFloat As String
Public HexMoved As String
Public HexLong As String

BeginProg
Scan (1,Sec,3,-1)
'try converting floating point value directly. notice that value will be
'rounded to nearest signed integer before hexadecimal conversion
HexFloat =Hex(FloatVar)

'by moving bytes of floating point number into a long first,
'we can get hexadecimal representation of floating point number
MoveBytes (Moved,0,FloatVar,0,4)
HexMoved =Hex(Moved)
'no need to convert first to get correct hex, already a long
HexLong =Hex(LongVar)
NextScan
EndProg
```

## Remarks

The Hex function returns a variable length string with no leading zeros. The result should be formatted as type String of at least 9 characters. If a fixed length string is required, use the [Sprintf](sprintf.md) function for conversion and formatting.

The input for Hex (Expression parameter) can be a constant, variable, or expression. Expression cannot be of type String. Keep in mind that the input is converted to Long during processing. For example Hex(9.469614E-13) will result in "0" and not "2B8545E1"; 9.469614E-13 will first be converted to 0 (the closet Long representation) and then to hexadecimal. To covert a floating point number to hexadecimal first move the bytes into a Long and then into Hex (refer to earlier Example program).

## Parameter

# Expression

The expression on which to perform the Decimal to Hex or Hex to Decimal conversion.

Type: String (for HextoDec) or Decimal Value (for Hex)
