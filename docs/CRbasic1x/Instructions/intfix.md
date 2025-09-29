# INT, FIX (Integer, Fix Integer)

The INT and FIX functions are used to return the integer portion of a number.

## Syntax

INT( number )

FIX( number )

This example illustrates the use of INT and FIX.

```
Public A, B, C, D'Declare variables.
BeginProg
A =INT( -99.8 )'Returns -100
B =FIX( -99.8 )'Returns -99
C =INT( 99.8 )'Returns 99
D =FIX( 99.8 )'Returns 99
EndProg
```

## Remarks

The Number argument can be any valid numericexpression. Both INT and FIX remove the fractional part of number and return the resulting integer value.

If the numeric expression results in a Null value, INT and FIX return aNAN

**Note:** Not a number. A data word indicating a measurement or processing error. Voltage overrange, SDI-12 sensor error, and undefined mathematical results can produce NAN.

.

The difference between INT and FIX is that if number is negative, INT returns the first negative integer less than or equal to the Number argument, whereas FIX returns the first negative integer greater than or equal to the Number. For example, INT converts -8.4 to -9, and FIX converts -8.4 to -8.

FIX(number) is equivalent to:

SGN(number) \* INT(ABS(number))
