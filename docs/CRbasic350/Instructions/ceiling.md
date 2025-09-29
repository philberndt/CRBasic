# Ceiling (Round to Higher Number)

The Ceiling function rounds a value to a higher number.

## Syntax

_variable_ = Ceiling(Number)

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

The Ceiling function rounds a Number up to an integer value.

To round a value down to an integer, use the [Floor](floor.md) function. To perform arithmetic rounding on a value, use the [Round](round.md) function.
