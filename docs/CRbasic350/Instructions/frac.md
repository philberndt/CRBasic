# FRAC (Fraction)

The FRAC function returns the fractional part of a number.

## Syntax

FRAC( number )

```
Public A, B, Number, FRACResult'Declare variables.

BeginProg

A =FRAC( -99.8 )'Returns -0.8
B =FRAC( 99.8 )'Returns 0.8
Scan (1,Sec,3,0)
'Change Number in the public table to see the FRAC() of different values
FRACRESULT =FRAC(Number)
NextScan

EndProg
```

## Remarks

The FRAC function returns the fractional portion of the value in the Number argument.
