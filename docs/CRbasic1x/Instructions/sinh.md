# SINH (Hyperbolic Sine)

The SINH function returns the hyperbolic sine of an expression or value.

## Syntax

SINH( Expr )

The example uses SINH to calculate the hyperbolic sine of a voltage input.

```
Public Volt1, Ans'Declare variables

BeginProg
Scan ( 1,Sec, 3, 0)
VoltDiff(Volt1,1,mv5000C,1,True,0,15000,1,0)

'Returns voltage on Channel(1) to Volt(1)
Ans =SINH( Volt1 )'The Hyperbolic Sine of Volt1
NextScan
EndProg
```

## Remarks

The SINH function returns the hyperbolic sine [ SINH(x) = 0.5( ex- e-x) ] for the value contained in the Expr argument.
