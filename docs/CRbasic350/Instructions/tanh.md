# TANH (Hyperbolic Tangent)

The TANH function returns the hyperbolic tangent of an expression or value.

## Syntax

TANH( Expr )

The example uses TANH to calculate the hyperbolic tangent of a voltage input.

```
Public Volt1, Ans'Declare variables.
BeginProg
Scan (1,Sec,3,0)
VoltDiff(Volt1,1,mv2500,1,True,100,4000,1,0)

'Returns voltage on Channel(1) to Volt (1)
Ans =TANH( Volt1 )'The Hyperbolic Tangent of Volt1.
NextScan
EndProg
```

## Remarks

The TANH function returns the hyperbolic tangent [ tanh(x) = sinh(x)/cosh(h) ] for the value defined in the Expr argument.
