# ATN (Arctangent)

The ATN function returns the arctangent of a number.

## Syntax

ATN( number )

The example uses ATN to calculate Pi. By definition, ATN(1) is 45 degrees; 180 degrees equals Pi radians.

```
Public Pi'Declare variables.

BeginProg
Pi = 4 *ATN( 1 )'Calculate Pi
EndProg
```

## Remarks

The Number argument can be any valid numericexpression.

The ATN function takes the ratio (number) of two sides of a right triangle and returns the corresponding angle. The ratio is the length of the side opposite the angle divided by the length of the side adjacent to the angle. The result is expressed in radians and is in the range -Pi/2 to Pi/2 radians. Pi is approximately 3.141593.

[AngleDegrees](angledegrees.md) can be used to return the result of this function in degrees instead of radians.

To convert degrees to radians, multiply degrees by Pi/180. To convert radians to degrees, multiply radians by 180/Pi.

**NOTE:** ATN is the inverse trigonometric function of [TAN](tan.md), which takes an angle as its argument and returns the ratio of two sides of a right triangle. Do not confuse ATN with the cotangent, which is the simple inverse of a tangent (1/tangent).
