# Floor (Round to a Lower Number)

The Floor function rounds a value to a lower number.

## Syntax

_variable_ = Floor(Number)

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

The Floor function rounds a Number down to an integer value.

To round a value up to an integer, use the [Ceiling](ceiling.md) function. To perform arithmetic rounding on a value, use the [Round](round.md) function.
