# LOG or LN (Logarithm)

The LOG, or LN, function returns the natural logarithm of a number.

## Syntax

LOG( number )

or

LN( number )

The example calculates the value of e, then uses the LOG function to calculate the natural logarithm of e to the first, second, and third powers.

```
Public I, M'Declare variables.
BeginProg
M = Exp( 1 )
For I = 1 To 3'Do three times.
M =LOG( EXP( 1 ) ^ I )
Next I
EndProg
```

## Remarks

The Number argument can be any valid numericexpressionthat results in a value greater than 0. The natural logarithm is the logarithm to the base e. The constant e is approximately 2.718282. You can calculate base-n logarithms for any number x by dividing the natural logarithm of x by the natural logarithm of n as follows:

LOGN( x ) = LOG( x ) / LOG( n )

ANAN

**Note:** Not a number. A data word indicating a measurement or processing error. Voltage overrange, SDI-12 sensor error, and undefined mathematical results can produce NAN.

is returned if Number is a NAN or equal to/less than 0.

**NOTE:** CRBasic uses base e for both Log and LN.
