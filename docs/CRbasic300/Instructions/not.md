# NOT

The NOT function is used to perform a logical negation on a number.

## Syntax

result =NOTNumber

The example sets the value of the variable Msg depending on the state of Flag.

```
Public Flag As Boolean'Declare variables
Public Msg

BeginProg
Scan (1,Sec,3,0)
IfNOTFlag Then'Evaluate expressions
Msg = 10
Else
Msg = 100
EndIf
NextScan
EndProg
```

## Remarks

If bit in _Number_ is 0, then result is 1.

If bit in _Number_ is 1, then result is 0.

Bit-wise operations are performed on integers; floating point values will first be converted before the bit-wise operation is performed. Although NOT is a bit-wise operator, it is often used to testBoolean

**Note:** Data type used to represent conditions or hardware that have only two states (true or false) such as flags and control ports.

(True/False) conditions. Boolean values are implemented as Longs that are restricted to -1 (True) or 0 (False). Any non-zero number >= 1 will evaluate as true (a float value between .999 and 0 when converted to a Long is 0). Because NOT is a bit-wise operation, the only non-zero number that NOT can operate on and return 0 is -1. The binary representation of -1 has all bits equal 1. That is why the predefined constant True = -1.

NOT (-1) = 0

NOT (0) = -1

NOT (NAN) = NAN

NAN = Not a number
