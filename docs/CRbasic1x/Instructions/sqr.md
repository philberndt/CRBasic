# SQR (Square Root)

The SQR function returns the square root of a number.

## Syntax

SQR( number )

The example uses SQR to calculate the square root of Vin on differential terminal 1.

```
'Apply AC or DC voltage to Diff CH1
'If Vin goes negative, the Root values will go to zero

Public root, Vin'Declare variables

DataTable (SQRoots,1,1000)'Store average Vin and square root values in table
DataInterval (0,1,sec,10)
Average (1,Vin,FP2,0)
Average (1,root,FP2,0)
EndTable

BeginProg
Scan(1,Sec,1,0)'Scan rate 1 second
VoltDiff(Vin,1,mv5000C,1,1,0,15000,1,0)

If Vin < 0 Then
root = 0'Avoid trying to take square root of neg number, set = 0.
Else
root =SQR(Vin)
EndIf
CallTable SQRoots
Nextscan
EndProg
```

## Remarks

This function calculates the square root of the value specified by the Number argument. The Number argument can be any valid numericexpressionthat results in a value greater than or equal to 0.
