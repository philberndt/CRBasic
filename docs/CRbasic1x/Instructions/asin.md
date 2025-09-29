# ASIN (Arcsine)

The ASIN function returns the arcsin of a number.

## Syntax

ASIN( number )

The example uses ASIN to calculate Pi. By definition, ASIN(1) is 90 degrees; 180 degrees equals Pi radians.

```
Public Pi'Declare variables.

BeginProg
Pi = 2 *ASin( 1 )'Calculate Pi.
EndProg
```

## Remarks

The Number argument can be any valid numericexpressionthat has a value between -1 and 1 inclusive.

The ASIN function takes the ratio (number) of two sides of a right triangle and returns the corresponding angle. The ratio is the length of the side opposite to the angle divided by the length of the hypotenuse. The result is expressed in radians and is in the range -Pi/2 to Pi/2 radians. Pi is approximately 3.141593.

[AngleDegrees](angledegrees.md) can be used to return the result of this function in degrees instead of radians.

To convert degrees to radians, multiply degrees by Pi/180. To convert radians to degrees, multiply radians by 180/Pi.

**NOTE:** ASIN is the inverse trigonometric function of [Sin](sin.md), which takes an angle as its argument and returns the length ratio of the side opposite the angle to the hypotenuse.
