# RectPolar (Rectangular to Polar Coordinates)

The RectPolar function is used to convert rectangular coordinates into polar coordinates.

## Syntax

RectPolar(Dest,Source)

In the following example, a counter (Deg) is incremented from 0 to 360 degrees. The cosine and sine of the angle are taken to get X and Y in rectangular coordinates. RectPolar is then used to convert the value to polar coordinates.

```
'CR1000X Series
Public XY(2),Polar(2),Deg,AnglDeg
Const Pi=4*ATN(1)'Define Pi
Alias XY(1)=X'Setup alias for the X coordinate
Alias XY(2)=Y'Setup alias for the Y coordinate
Alias Polar(1)=Length'Setup alias for the Vector coordinate
Alias Polar(2)=AnglRad'Setup alias for the Angle coordinate

DataTable(RtoP,1,361)
FillStop
Sample(1,Deg,IEEE4)
Sample(2,XY,IEEE4)
Sample(2,Polar,IEEE4)
Sample(1,AnglDeg,IEEE4)
EndTable

BeginProg
Scan (1,Sec,3,0)
For Deg=0 to 360
XY(1)=COS(Deg*Pi/180)' COS of angle in radians
XY(2)=Sin(Deg*Pi/180)' Sin of angle in radians
RectPolar(Polar,XY)' Conversion to Polar notation
AnglDeg=Polar(2)*180/Pi' Convert angle to degrees for comparison
CallTable RtoP' Store results in table
Next Deg
NextScan
EndProg
```

## Remarks

Both arguments (Dest and Source) must be arrays dimensioned to at least two elements. Source(1) should have the X coordinate value and Source(2) should have the Y coordinate value. The vector length is returned to the array element specified in Dest(1); the angle in radians is returned in the array element specified in Dest(2).

## Parameters

# Dest (Destination)

Variable array in which to store the 2 resultant values. The length of the vector is stored in the specified destination element and the angle, in radians( π), is stored in the next element of the array

Type -- Variable Array

# Source

The variable array containing the X and Y coordinates that are to be converted to polar coordinates. The X value must be in the specified array element and the Y value in the next element of the array.

Type -- Variable Array

[AngleDegrees](angledegrees.md) can be used to return the result of this function in degrees instead of radians.
