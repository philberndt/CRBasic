# ATN2 (Arctangent of Y/X)

The ATN2 function returns the arctangent of y/x.

## Syntax

ATN2( Y, X )

The example uses ATN2 to calculate Pi. By definition, ATN2(5,5) is 45 degrees; 180 degrees equals Pi radians.

```
Public Pi'Declare variables.

BeginProg
Pi = 4 *ATN2( 5, 5 )'Calculate Pi.
EndProg
```

## Remarks

ATN2 function calculates the arctangent of Y/X, returning an angle (in radians) in the range of Pi to Pi. The signs of the parameters determine the quadrant of the returned value.

| Quadrant | Sign Y | Sign X | ATN2         |
| -------- | ------ | ------ | ------------ |
| I        | +      | +      | 0 to Pi/2    |
| II       | +      | -      | Pi/2 to Pi   |
| III      | -      | -      | -Pi/2 to -Pi |
| IV       | -      | +      | 0 to Pi/2    |

ATN2 is defined for every point except the origin (X = 0 and Y = 0).[AngleDegrees](angledegrees.md) can be used to return the result of this function in degrees instead of radians.

Pi is approximately 3.141593. To convert degrees to radians, multiply degrees by Pi/180. To convert radians to degrees, multiply radians by 180/Pi.

**NOTE:** ATN2 is the inverse trigonometric function of TAN, which takes an angle as its argument and returns the ratio of two sides of a right triangle. Do not confuse ATN2 with the cotangent, which is the reciprocal of a tangent (1/tangent).
