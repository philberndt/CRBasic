# If...Then...Else

The If, Then, Else statements are used to process one or more instructions conditionally, based on the evaluation of anexpression.

## Syntax 1 (single line form)

```
If
```

condition

```
Then
```

thenstatements [

```
Else
```

elsestatements]

## Syntax 2 (block form)

```
If
```

condition1

```
Then
```

[statementblock-1]

[

```
ElseIf
```

condition2 Then

[statementblock-2] ]

[

```
Else
```

[statementblock-n] ]

```
EndIf
```

(or

```
End If
```

)

The example illustrates the various forms of the If...Then...Else syntax.

```
Dim X, Y, Temp( 5 )'Declare variables.
X = Temp( 1 )
IfX < 10Then
Y = 1'1 digit.
ElseIfX < 100Then
Y = 2'2 digits.
Else
Y = 3'3 digits.
EndIf
. . . .'Run some code.
. . . .'Run some code.
```

The Second Example is a full program that shows the basic functionality of the If..Then..Else syntax.

```
'This program measures the room temperature using a type T thermocouple.
'A string variable called Temp_Status changes between Cold, Normal, and Hot
'depending on the room temperature.

'Declare Public Variables
Public Temp'Variable for temperature
Public Temp_Status As String *50'variable for temperature status
Dim Tref'variable for reference temperature

'Declare Temperature_Data table
DataTable (Temperature_Data,True,-1)
Sample(1,Temp,FP2)
Sample(1,Temp_Status,String)
EndTable

'Begin main program
BeginProg
Scan (1,sec,2,0)
PanelTemp (TRef,4000)
TCDiff (Temp,1,mv34,1,TypeT,TRef,True,0,4000,1.0,0)

'Measure temp of Diff Chan 1
'a temperature between 20 and 25 degrees C is designated normal
IfTemp >=20 AND Temp <= 25Then
Temp_Status = "Normal"'update Temperature status
'a temperature above 25 degrees C is designated Hot
ElseIfTemp > 25Then
Temp_Status = "Hot"
'A temperature below 20 degrees C is designated Cold
ElseIfTemp < 20Then
Temp_Status = "Cold"
EndIf
CallTable Temperature_Data
NextScan
EndProg
```

## Remarks

The single line form shown in Syntax 1 is often useful for short, simple conditional tests. The EndIf statement is not used.

The block form of an If...Then control structure, as shown in Syntax 2, provides more structure and flexibility than the single line form and is usually easier to read, maintain, and debug. This structure allows you to specify an alternate Condition to evaluate if the first Condition is not True by using the ElseIf statement. The statements in a block form control structure cannot have any characters, other than spaces or tabs, before them on a program line. An EndIf statement must close the block form of an If Then control structure.

The AND and OR instructions can be used along with the If Then Else instructions for more flexible (and powerful) programming logic.

## Parameters

# If

The If statement begins the If...Then control structure.

# Condition

The Condition is an expression that evaluates True (nonzero) or False (0 and Null). With the single-line form, you can have multiple statements for a Condition, but they must be on the same line and separated by colons, as in the following statement:

If A > 10 Then A = A + 1 : B = B + A : C = C + B

The implied EndIf occurs at the very end of the line.

# Then

The Then statement defines actions to be taken if the Condition is True.

# ThenStatements

The ThenStatements are instructions or program branches to be performed when condition is true.

# ElseIf

The ElseIf statement is used to specify another Condition to be evaluated if the first Condition is False. If the first Condition is True, the ElseIf statement is not evaluated and control is passed to the portion of code following the Then statement. Multiple ElseIf statements can be used in one If Then control structure. ElseIf statements must appear before the Else statement.

# Else

The Else statement defines actions to be taken if the Condition is False. If the Else statement is not present, control passes to the next statement in the program.

# ElseStatements

The ElseStatements are instructions or program branches to be performed when the Condition is False.

# EndIf

The EndIf instruction is used to indicate the end of the If/Then/Else construct. It can also be written as End If.

Also see [Conditional Compiling](../Info/conditionalcompilation.md).

**NOTE:** The [Select Case](selectcase.md) instruction may be more useful when evaluating a single expression that has several possible actions.
