# ABS (Absolute Value)

The ABS function returns the absolute value of a number.

## Syntax

ABS( number )

The following example illustrates use of the ABS function.

```
Const PI = 3.141592654
Public X, Xabs, count

BeginProg
Scan (1,Sec,0,0)
count+=1
'X is the sine of angles at pi/8 intervals
X = SIN(count*PI/8)
'Xabs is the absolute value of X
Xabs =ABS(X)
NextScan
EndProg
```

## Remarks

The number argument can be any valid numericexpression. The absolute value of a number is its unsigned magnitude. For example, ABS(-1) and ABS(1) both return 1.
