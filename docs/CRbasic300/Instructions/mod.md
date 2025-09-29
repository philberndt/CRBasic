# MOD (Modulo Divide)

The MOD function is used to perform a modulo divide of two numbers.

## Syntax

result = operand1 MOD operand2

The example uses the MOD operator to determine if a 4-digit year is a leap year.

```
Public TestYr, LeapStatus As Boolean

BeginProg
TestYr = 1995
Scan (1,Sec,3,0)
If TestYrMod4 <> 0 Then
LeapStatus=False
ElseIf TestYrMod400 = 0 Then
LeapStatus=True
ElseIf TestYrMod100 = 0 Then
LeapStatus = False
Else
LeapStatus = True
EndIf
NextScan
EndProg
```

## Remarks

The MOD function divides operand1 by operand2 and returns the remainder as the result. For example, in the expression A = 19 MOD 6.7, A equals 5.6. That is:

19 / 6.7 = 2. 835820896

6.7 \* 0.835820896 = 5.6

The operands can be any number or numeric expression.
