# Call (Transfer Program Control)

The Call statement is used to transfer program control from the main program to a CRBasic subroutine.

## Syntax

CallName(Arguments)

In the following example program, a thermocouple measurement is evaluated. If the value is greater than 26, a subroutine is called in which the value is evaluated further.

If TCTemp is <26, the string Temp Normal is stored in the Test variable. If TCTemp is greater than 26, but less than or equal to 29, then the string "Temp Watch" is set. If TCTemp is greater than 29, the string "Temp Warning" is stored.

See the [Sub](subexitsubendsub.md) instruction for an example where arguments are used.

```
Public RefTemp, TCTemp, Test AS STRING * 15

DataTable (Table1,True,-1)
Sample (1,Test,String)
EndTable

Sub TempTest
IF TCTemp <= 29 Then Test = "TEMP WATCH"
IF TCTemp > 29 Then Test = "TEMP WARNING"
EndSub

BeginProg
Scan (1,Sec,3,0)
PanelTemp (RefTemp,4000)
TCDiff (TCTemp,1,mv34,1,TypeT,RefTemp,True,0,4000,1.0,0)

If TCTemp > 26 THEN
CallTempTest
Else
Test = "TEMP NORMAL"
ENDIF
CallTable (Table1)
NextScan
EndProg
```

## Remarks

Use of the Call keyword when calling a subroutine is optional. If Call is used and Arguments are being passed to the subroutine, the Arguments must be enclosed in parentheses.

## Parameters

# Call

Call is an optional keyword used to transfer program control to another procedure.

# Name

The name for the subroutine. The name is limited to 39 characters.

Type: Alphanumeric

For the Call statement, the Name parameter is the name of the procedure to call.

# Arguments

The values to be passed to the subroutine (the variables can be Float, Long, or String; if not specified Float is assumed). The values are passed in the order listed.

Type: Constant, Variable, Array, or Expression

For the Call statement, the Arguments are variables, arrays, orexpressionsthat should be passed to the subroutine. CRBasic passes all arguments into a subroutine by reference (that is, a reference to the memory location of the variable is passed, rather than an actual value). Therefore, if the value of an argument is changed by the subroutine, the change will take effect in the main program as well.
