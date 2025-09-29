# IIF

The IIF function is used to evaluate an expression and return one of two values based on that evaluation.

## Syntax

```
IIF
```

(Expression,TrueValue,FalseValue)

In the following example, IIF is used to set control port 1 high if the temperature is above 23 degrees. Otherwise, the control port is low.

```
'Declare Variables
Public Temp, Set

'Main Program
BeginProg
Scan (1,Sec,3,0)
PanelTemp (Temp,15000)
Set=IIF(Temp>23,1,0)
Portset (C1,Set)
NextScan
EndProg
```

## Remarks

The Expression to be evaluated is defined in parameter 1. If the Expression is True, the TrueValue (parameter 2) is returned. If the Expression is False, the FalseValue (parameter 3) is returned. By default, the values returned are of type Float. TrueValue and FalseValue can be Strings. For a string to be returned, the parameter must be enclosed in quotes. For example, IIF(1, 1 , 0 ). To obtain an output of type Long, apply FormatLong() in both true and false conditions. For example, IIF (1,FormatLong (ValueA,"%d"),FormatLong (ValueB,"%d")).

IIF always evaluates both the true and false arguments, even though it only returns one of them. This could lead to mathematical errors even when the expression returned is valid, but would not have been valid if the alternative argument had been used.
