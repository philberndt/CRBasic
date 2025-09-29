# INTDV (Integer Division)

The INTDV operator performs an integer division of two numbers.

## Syntax

XINTDVY

In the following example an integer division is performed on two variables (X, Y) and the result is stored in another variable (Result).

Variables X and Y are placed outside the Scan/NextScan instruction set. This initializes the values when the program is compiled but allows them to be changed for example purposes. Use the numeric monitor to change X and/or Y and observe the change in Result.

```
Public Result, X, Y

BeginProg
X = 7
Y = 2
Scan (1,Sec,3,0)
Result = XINTDVY
NextScan
EndProg
```

## Remarks

The INTDV operator divides one number by another using integer division and returns the integer portion of the result. This operator can be used in an expression or as the source for a variable (for example, Result = X INTDV Y).
