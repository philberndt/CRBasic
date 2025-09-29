# COSH (Hyperbolic Cosine)

The COSH function returns the hyperbolic cosine of anexpressionor value.

## Syntax

COSH( expression )

The example uses COSH to calculate the hyperbolic cosine of a voltage input and store the result in the Ans variable.

```
Public Volt1, Ans'Declare variables.

BeginProg
Scan (1,Sec,3,0)
VoltDiff (Volt1,1,mv5000C,1,True ,200,15000,1.0,0)'Return voltage on DiffChan1

Ans =COSH( Volt1 )
NextScan
EndProg
```

## Remarks

The COSH function takes a value and returns the hyperbolic cosinefor that value.
