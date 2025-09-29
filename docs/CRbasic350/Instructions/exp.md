# EXP (Exponential)

The EXP function returns e (the base of natural logarithms) raised to a power.

## Syntax

EXP( number )

This example uses EXP to calculate the value of e. EXP( 1 ) is e raised to the power of 1.

```
Public ValueOfE,x,y'Declare variables

BeginProg
'EXP(X) is e^x
'EXP(1) is e^1 or e.
ValueOfE =EXP( 1 )'Calculate value of e.

Scan(1,Sec,3,0)
Y=EXP(x)'Calculate e^x.
NextScan
EndProg
```

## Remarks

If the value of the Number argument exceeds 709.782712893, an overflow error occurs. The constant e is approximately 2.718282.

**NOTE:** The EXP function complements the action of the [LOG](logorln.md) function and is sometimes referred to as the antilogarithm.
