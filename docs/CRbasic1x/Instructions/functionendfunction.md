# Function/EndFunction (Create a Function)

The Function/EndFunction declaration is used to create a user defined function.

## Syntax

```
Function
```

FunctionNameOptional _(optional parameters) optional_ AsDataType

Return( expression )_(optional)_

```
ExitFunction
```

_(optional)_

```
EndFunction
```

Calls to a Function

```
Variable = FunctionName( )'Call function
```

Variable = FunctionName'returns last value from function

```

### Example #1


The following example creates a function to return a string reflecting the day of the week.

```

Public DayofWeek As String \* 20

FunctionDayName As String
Dim RTime(9), InputDate
RealTime (RTime())
InputDate = RTime(8)
Select Case InputDate
Case 1
DayName = "Sunday"
Case 2
DayName = "Monday"
Case 3
DayName = "Tuesday"
Case 4
DayName = "Wednesday"
Case 5
DayName = "Thursday"
Case 6
DayName = "Friday"
Case 7
DayName = "Saturday"
Case Else = "Error in InputDate"
EndSelect
EndFunction

BeginProg
Scan (1,Sec,3,0)
DayOfWeek = DayName()
NextScan

EndProg

```

### Example #2


This example creates a function that issues a 2 second toggle to a control port whenever a flag is high and returns the string "BlinkBlink".

```

Public Flag(2), BlinkStatus As String

FunctionBlink As String
PortSet (C1,1 )
Delay (0,2,Sec)
PortSet (C1,0 )
Return("BlinkBlink")
EndFunction

BeginProg
Scan (1,Sec,3,0)
If Flag(1) Then
BlinkStatus = Blink()
Else
BlinkStatus="off"
EndIf
NextScan
EndProg

```

### Example #3


The following program shows the creation of Functions that could be used to perform various mathematical operations on values.

```

Public B,C,D,A(6), AST as String

FunctionTimes100 (x)
Return 100 \*x
EndFunction

FunctionPlusOne (x)
Return(x+1)
EndFunction

FunctionPlusTwo (x)
PlusTwo = PlusOne(x) + 1
Exit Function
EndFunction

FunctionxPlusy (x, y, Optional z = 0)
Return (x+y+z)
EndFunction

FunctionPlusFour (x) As Long
PlusFour = x + 4
EndFunction

FunctionStringHundred (x As Long) As String \* 30
Return(x + " Hundred")
EndFunction

BeginProg
B = 5
C = 4
D = 2
A(1) = Times100(B)
A(2) = PlusOne(B)
A(3) = PlusTwo(B)
A(4) = xPlusy(B,c)
A(5) = PlusFour(B)
A(6) = xPlusy(B,C,D)
ASt = StringHundred(B)
EndProg

```

### Example #4


This example demonstrates recalling the last value returned by a function by referencing the function without parentheses.
Note that this relies on initialization of the function to 0.This functionality is fully implemented inCR1000x OSs > 2.

```

Public Number(9)

Dim rTime(9)'Holds results of RealTime instruction

'Function with no parameters
FunctionFunction1
'Function1 returns a random number between 1 and 10
RealTime(rTime())
Randomize(rTime(6))
Return INT(10 \*RND+1)
EndFunction

'Function with a parameter
FunctionFunction2(N1)
Return N1\* 3
EndFunction

FunctionFunction3
'Function 3 returns the sum of the last returned values of Function1 and Function2
Return Function1 + Function2
EndFunction

FunctionFunction4
'Function4 returns its last returned value plus 1
Return Function4 + 1
EndFunction

FunctionFunction5(N1)
'Function5 returns its last returned value plus the parameter N1
Return Function5 + N1
EndFunction

BeginProg
Function4 = 0
Function5 = 0
Scan(1,Sec,3,5)

'Recall last value returned by a function without parameters
Number(1) = Function1()
Number(2) = Function1

'Recall last value returned by a function with a parameter
Number(3) = Function2(Number(1))
Number(4) = Function2

'Recall last value returned by a function without parameters
'and a function with parameters from within another function
Number(5) = Function3()

'Call a function with no parameters which recalls its own last value
Number(6) = Function4
Number(7) = Function4()

'Call a function with a parameter which recalls its own last value
Number(8) = Function5
Number(9) = Function5(4)

NextScan
EndProg

```

## Remarks


The Function/EndFunction declaration is similar to a subroutine declaration ([Sub/EndSub](subexitsubendsub.md)). By default, a Function is aFloat

**Note:** Four-byte floating-point data type. Default datalogger data type for Public or Dim variables. Same format as IEEE4.

, but it can be specified as aString

**Note:** A data type used when declaring a variable consisting of alphanumeric characters.

(with an optional \* size),Long

**Note:** Data type used when declaring a variable as an integer.

, orBoolean

**Note:** Data type used to represent conditions or hardware that have only two states (true or false) such as flags and control ports.

. As with a subroutine declaration, the parameter list describes local parameters and optionally their type (Float, Long, Boolean, String). If not specified, the default parameter type is Float. One difference between a Sub and a Function is a Function returns a value, whereas a subroutine does not. Another difference is if a value being passed into a function is an array, only the first value, or the value defined by the index, is used (that is, the entire array is not passed into the function). Strings are terminated with a null character; thus, a string passed into a function will include only those characters up until a null character is encountered. The FunctionName parameter is limited to 39 characters.

Function names with their parameters are called just like built in functions; i.e., by simply using their name with parameters anywhere within an expression. When a function is called, the parameters are copied into the Function's local parameter list, as is the case when subroutines are called. However, unlike subroutines, which when exited copy the local parameter values back out to any variables that were passed in, Functions do no such copying back out but rather return a value that is used by the expression that invoked the function. Only one instance of a function can run at any given time.

In order to call a Function, parentheses must be used even if the function has no parameters. The parentheses indicate a call to the function. When a function is called within the function itself and its parentheses are omitted, the last value returned by the function is used rather than the function running again. That is, when no parentheses are used, the function behaves like a variable that holds the last value returned by the function (see program example 4). Note that like variables, Functions are initialized to zero.

Return or ExitFunction exits the function at run time. EndFunction terminates the Function declaration and exits the function at run time. The expression returned by the Function is specified by Return(expression), or, if Return is not executed before EndFunction or ExitFunction, then the return value is specified by the assignment of an expression to the Function's name. If neither of these methods are used, then NAN is returned.

Functions include the ability to pass in optional parameters. These optional parameters are preceded by the Optional keyword. Optional parameters must be initialized using a constant value (required parameters cannot be initialized). The default for an optional parameter is used by passing in a comma; multiple default parameters are specified using multiple commas. Required parameters cannot follow optional parameters in the definition of the function.

Functions can be nested to a maximum of five deep. If a function is nested more than five deep, when the program is sent to the datalogger, a warning will be given that the function will not be executed because it is nested too deep.

Variables declared by Dim within a subroutine or function are local to that subroutine or function. The same variable name can be used within other subroutines or functions or as a global variable without conflict. Variables used as parameters to a subroutine or function are also local. Note that variables initialized within a subroutine or function are initialized only at compile time. They are not re-initialized with each call to the subroutine or function.

[Pointer variables](../Info/pointervariable.md) can be used in functions to pass in arguments based on memory location rather than variable name.
```
