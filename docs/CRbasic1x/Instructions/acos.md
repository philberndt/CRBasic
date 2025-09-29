# ACOS (Arccosine)

The ACOS function returns the arc cosine of a number.

## Syntax

ACOS( Number )

The example uses ACOS to calculate Pi. By definition, ACOS(0) is 90 degrees; 180 degrees equals Pi radians.

```
Public Pi'Declare variables.

BeginProg
Pi = 2 *ACOS( 0 )'Calculate Pi.
EndProg
```

## Remarks

The Number argument can be any valid numeric [expression](../terms/expression.md) that has a value between -1 and 1, inclusive.

The ACOS function takes the ratio (number) of two sides of a right triangle and returns the corresponding angle. This ratio is the length of the side adjacent to the angle divided by the length of the hypotenuse. The result is expressed in radians and falls within the range of -π/2 to π/2 radians. Here, π is approximately 3.141593.

To return the result of this function in degrees instead of radians, you can use the [AngleDegrees](angledegrees.md) function.

To convert degrees to radians, multiply the degrees by π/180. To convert radians to degrees, multiply the radians by 180/π.

Please note: ACOS is the inverse trigonometric function of [COSINE](cos.md). COSINE takes an angle as its argument and returns the length ratio of the side adjacent to the angle to the hypotenuse.
