# Round

The Round function rounds a value to a higher or lower number.

## Syntax

_variable_ = Round(Number,Decimal)

The following program shows the use of the Round, Ceiling, and Floor functions. You can see the results of each function by using the software's numeric display to enter a value into the Number variable or to change the Decimal variable in Round.

```
Public Number, Decimal, RoundVar, FloorVar, CeilVar

BeginProg
Number=1234.5678
Decimal=1
Scan (1,Sec,3,0)
RoundVar=Round(Number,Decimal)
CeilVar=Ceiling(Number)
FloorVar=Floor(Number)
NextScan
EndProg
```

## Remarks

The Round function rounds the Number up if the determining digit is 5 or greater; otherwise, it rounds down. This is commonly referred to as arithmetic rounding.

**NOTE:** Negative numbers effectively round down if the determining digit is greater than 5 and up if it is less than 5; for example, -8.6 rounds to -9.

To round a value up or down to an integer, use the [Ceiling](ceiling.md) function or the [Floor](floor.md) function.

## Parameters

# Number

The value on which to perform the rounding operation. It can be any value or anexpression.

Type: Expression

# Decimal

Used to determine how many decimal places to keep. If Decimal is set to 0, the result will be an integer. If Decimal is a negative number, it specifies the power of 10 to which you want to round.

Type: Variable or Constant

## Examples:

| Function           | Return  |
| ------------------ | ------- |
| Round(172.345, 2)  | 172.35  |
| Round(-172.345,2)  | -172.35 |
| Round(172.345, 0)  | 172     |
| Round(172.234, -1) | 170     |
| Round(172.234, -2) | 200     |
