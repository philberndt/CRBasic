# Sub, Exit Sub, EndSub (Begin, Exit, End Subroutine)

The Sub declaration is used to mark the beginning of a subroutine in the program code. The Exit Sub statement specifies the condition under which the program should exit the subroutine. The EndSub statement is placed after the last instruction in the subroutine code to mark the end of the subroutine.

## Syntax

```
Sub
```

SubName[(ArgumentList)]

[ statementblock ]

[

```
Exit Sub
```

]

[ statementblock ]

```
EndSub
```

The following example uses the Sub/EndSub statements to create a subroutine that converts three thermocouple measurements to degrees F.

Arguments are used in the subroutine to optimize the code. With each pass through the For/Next loop, TC(i) is mapped to Tmp and TC_F(i) is mapped to TmpF. Therefore, the associated equations for the four passes through the loop are:

pass 1: TC_F(1) = TC(1) \* 1.8 + 32

pass 2: TC_F(2) = TC(2) \* 1.8 + 32

pass 3: TC_F(3) = TC(3) \* 1.8 + 32

```
'Declare Variables:
Public RefT,RefT_F, TC(3), I, TC_F(3)

'Data output in deg C:
DataTable (TempsC,1,-1)
DataInterval (0,1,Min,10)
Average (1,RefT,FP2,0)
Average (3,TC(),FP2,0)
EndTable

'Same Data output in F:
DataTable (TempsF,1,-1)
DataInterval (0,1,Min,10)
Average (1,RefT_F,FP2,0)
Average (3,TC_F(),FP2,0)
EndTable

'Subroutine to convert temperature in degrees C to degrees F
SubConvertCtoF (Tmp, TmpF)
TmpF = Tmp*1.8 +32
EndSub

BeginProg
Scan (1,Sec,3,0)
'Measure Temperatures (panel, 3 thermocouples) in deg C
PanelTemp (RefT,4000)
TCDiff (TC,3,mv34,1,TypeT,RefT,True,0,4000,1.0,0)

'Call Output Table for C
CallTable TempsC
'Convert TC Temps to F using Subroutine:
For I = 1 to 3
Call ConvertCtoF(TC(I), TC_F(I))
Next I
ConvertCtoF(RefT, RefT_F)
'Call Output Table for F:
CallTable TempsF
NextScan
EndProg
```

Strings can be used as arguments in a subroutine. The following program section is an example of using a subroutine to split a string into two arrays: one with strings and one with reals. This could be used when a sensor returns a mixture of numbers and strings. The different types of variables could then be sampled out of the relevant arrays depending on type.

```
SubSplitdata (InString as string * 2000, strdata(25) as string, realdata(25))

Dim i

'Split incoming data into strings initially
'Space separated variables in the string

SplitStr (strdata(),InString," ",25,5)
For i=1 to 25
'Convert the strings to numerics - non-numerics will be returned as NAN
realdata(i)=strdata(i)
Next i
EndSub
```

## Remarks

A Subroutine is a separate procedure that is called by the main program using a Call statement. A Subroutine can take arguments, perform a series of statements, and change the value of its arguments. The Scan/NextScan instruction loop can be used within a Subroutine to set a different execution interval than the main program.

Subroutines must be declared before they are called in the program. The code for a Subroutine cannot be contained within the code for another Subroutine; however, a Subroutine can be called by another Subroutine. Subroutines cannot be used in an expression.

Variables declared by Dim within a subroutine or function are local to that subroutine or function. The same variable name can be used within other subroutines or functions or as a global variable without conflict. Variables used as parameters to a subroutine or function are also local. The array length of a local variable in a subroutine will take on the array length of a global variable passed into the local variable. Note that variables initialized within a subroutine are initialized only at compile time. They are not re-initialized with each call to the subroutine.

Subroutines include the ability to pass in optional arguments. These optional parameters are preceded by the Optional keyword. Optional parameters must be intitialized using a constant value (required parameters cannot be initialized). The default for an optional parameter is used by passing in a comma; multiple default parameters are specified using multiple commas. Required parameters cannot follow optional parameters in the definition of the function.

## Parameters

# Sub

The Sub declaration identifies the beginning of a Subroutine.

# Name

The name for the subroutine. The name is limited to 39 characters.

Type: Alphanumeric

Subroutine names follow the same rules that constrain the names of other variables. Because SubNames are recognized by all procedures in all modules, the name cannot be the same as any other globally recognized name in the program.

# Arguments

The values to be passed to the subroutine (the variables can be Float, Long, or String; if not specified Float is assumed). The values are passed in the order listed.

Type: Constant, Variable, Array, or Expression

The use of an ArgumentList lets you assign variables within the main program to variables in the subroutine, process those variables in the subroutine, and then use the resulting processed values later in the program. Multiple variables are separated by commas. Changing an argument's value inside the Subroutine changes its value in the calling procedure. Use of ArgumentList is optional.

# Statementblock

A statementblock is any group of statements that are executed within the body of the Subroutine.

# Exit Sub

The Exit Sub statement causes an immediate exit from a Subroutine. Program execution continues with the instruction that follows the call to the Subroutine. Any number of Exit Sub statements can appear anywhere in a Subroutine. The Exit Sub statement is optional.

# End Sub

The EndSub statement marks the end of a Subroutine.

**Caution:** Subroutines can be recursive; that is, they can call themselves to perform a given task. However, recursion can lead to strange results.
