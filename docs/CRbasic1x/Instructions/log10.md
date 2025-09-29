# LOG10 (Logarithm Base 10)

The LOG10 function returns the logarithm base 10 of a number.

## Syntax

LOG10( number )

This example demonstrates the use of the LOG10 instruction. Using the numeric display, the user can change the value of the variable Number to see the base 10 logarithm of Number. By changing the variable B, the user can see the base-B logarithm of Number.

```
Public Number, B, Log10ofNumber, LogBofNumber'Declare variables.

BeginProg
Scan(1,Sec,3,0)
Log10ofNumber =LOG10(Number)'Base-10 logarithm of Number.
LogBofNumber =LOG10(Number)/LOG10(B)'Base-B logarithm of number
NextScan
EndProg
```

## Remarks

The Number argument can be any valid numericexpressionthat has a value greater than 0. You can calculate base-n logarithms for any number x by dividing the logarithm base 10 of x by the logarithm base 10 of n as follows:

LOGN( x ) = LOG10( x ) / LOG10( n )
