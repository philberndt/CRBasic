# COS (Cosine)

The COS function returns the cosine of an angle.

## Syntax

COS( angle )

```
'This program calculates the sine, cosine, and tangent of angles from 0 to 360 at 30 degree intervals.
'Results are sent to Trig Table every 2 seconds.
Const Pi = 4 * ATN(1)'Declare Pi as a constant
Public Degrees, Radians, Cosine, Sine, Tangent'Declare variables

DataTable(Trig,True,-1)
'Sample variables in Trig Table
Sample(1,Degrees,FP2)
Sample(1,Radians,FP2)
Sample(1,Sine,FP2)
Sample(1,Cosine,FP2)
Sample(1,Tangent,FP2)
EndTable

'MainProgram
BeginProg
Scan(2,sec,0,0)
If Degrees >= 360 Then Degrees =0'When degrees reaches 360, reset degrees to zero
Radians = Degrees*(Pi/180)'Calculate angle in radians
Sine = SIN(Radians)'Calculate sine of angle
Cosine =COS(Radians)'Calculate cosine of angle
Tangent = TAN(Radians)'Calculate tangent of angle
'Note: Tangent is undefined at 90 and 270 degrees. Table will show tan(90) = tan(270) = -7999
CallTable(Trig)'Call Trig Table
Degrees+=30'Increment angle by 30 degrees
NextScan
EndProg
```

## Remarks

The COS function takes an angle and returns the ratio of two sides of a right triangle. The ratio is the length of the side adjacent to the angle divided by the length of the hypotenuse. The result lies in the range -1 to 1.

The Angle argument can be any valid numericexpressionmeasured in radians.

[AngleDegrees](angledegrees.md) can be used to change the source for this function to degrees instead of radians.

To convert degrees to radians, multiply degrees by Pi/180. To convert radians to degrees, multiply radians by 180/Pi. Pi is approximately 3.141593.
