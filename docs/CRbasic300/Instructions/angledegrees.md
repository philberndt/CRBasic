# AngleDegrees (Return Degrees)

The AngleDegrees declaration is used to set math functions in the program to return, or to expect as the source, degrees instead of radians.

## Syntax

AngleDegrees

In the following test program (which finds the arccosine of a random value), AngleDegrees is placed at the beginning of the program. It causes the value returned by ACOS to be in degrees instead of radians.

```
'Set functions to Angles instead of radians
AngleDegrees

'Declare variables & constants
Public RNumber, ANumber
Dim Time(9)
Const Upper=1
Const Lower=-1

'Main program
BeginProg
Scan (1,Sec,3,0)
'Realtime(sec) is used as a seed for RND
RealTime (Time())
Randomize Time(6)
RNumber=((Upper - Lower-1) * RND)
ANumber=ACOS(RNumber)
NextScan
EndProg
```

## Remarks

The AngleDegrees instruction is placed in the declarations section of the program, before the code enclosed in the BeginProg/EndProg instructions.

AngleDegrees affects the following instructions that return an angle in radians:[ATN](atn.md),[ATN2](atn2.md),[ACOS](acos.md),[ASIN](asin.md),[RectPolar](rectpolar.md).

Angle Degrees affects the following instructions that expect an angle in radians as the source:[COS](cos.md),[TAN](tan.md), and [SIN](sin.md).

Negative radians will convert to negative degrees.
