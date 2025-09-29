# Do...Loop

The Do Loop instructions are used to repeat a block of statements while a condition is true or until a condition becomes true.

## Syntax 1

Do[{

```
While
```

| Until} condition] |

[statementblock]

[ExitDo]

[statementblock]

Loop

## Syntax 2

Do

[statementblock]

[ExitDo]

[statementblock]

> Loop[{

```
While
```

| Until} condition] |

This example runs an infinite Do Loop (Loop 1) until the Flag variable is set True. For demonstration, use Table Monitor or Numeric Display to set the Flag variable True. When Loop 1 is exited, Loop 2 will run until the main scan counter reaches 60.

```
Public Flag as Boolean, ScanCount, Loopcount(2)
```

'Main Program
BeginProg
Scan (1,Sec,0,0)
ScanCount +=1'Increment main scan counter: Note, this counter will not increment as long as loop 1 is running
'Loop 1 will execute until Flag is set true. To set the flag true, use Table Monitor or Numeric Display and edit Flag from False to True.
Do
LoopCount(1)+=1'Increment Loop 1 counter
If Flag Then'If Flag = True, exit loop.
Exit Do
EndIf
Loop

NextScan

SlowSequence
'Loop 2 will execute until the main scan counter = 60
Scan(1,Sec,3,0)
Do
LoopCount(2)+=1
Loop UntilScanCount = 60

NextScan
EndProg

```
Example 2 demonstrates the significance of While/Until condition placement in the Do...Loop instruction. If the While/Until condition is placed at the beginning of the loop, the While/Until condition is checked before the code inside the loop is executed. If the While/Until condition is placed at the end of the loop, the code inside the loop is executed once before the condition is checked. There are two Do...Loop instructions in this program. One has a While condition at the beginning of the loop and the other has a While condition at the end of the loop.

```

Public Condition As Boolean = true'Variable for loop condition
Public LoopCount(2)'Array of variables for loop counting
'LoopCount(1) tracks how many times the code in the first Do...Loop is executed.
'LoopCount(2) tracks how many times the code in the second Do...Loop is executed.

BeginProg
'1st Do...Loop: While/Until condition is at the beginning of the loop
'The code in this Do...Loop will execute 20 times.
Do WhileCondition'Loop while Condition = True
LoopCount(1)+=1'Increment LoopCount(1)
If LoopCount(1) >= 20 Then Condition = False'Change Condition
'based on LoopCount(1). If condition equals False, the Do...Loop will be exited.
Loop
'2nd Do...Loop : While/Until condition is at the end of the loop
'The code in this Do...Loop will execute only once.
Do
LoopCount(2)+=1'Increment LoopCount(2)
Loop WhileCondition'Condition is not checked until after the code
'inside the Do...Loop has been executed once.
EndProg

```

## Remarks


The Do Loop instruction has these parts:


# Do


The Do statement declares the beginning of a Do Loop. It must be the first statement in a Do...Loop control structure.


# While


The While statement indicates that the loop is executed while the Condition is true.


# Until


The Until statement Indicates that the loop is executed until the Condition is true.


# Condition


The Condition is a numericexpressionthat is evaluated as True (nonzero) or False (0 or Null).


# StatementBlock (Statement Block)


The StatementBlock is the portion of the program that should be repeated until the loop is terminated by the Condition. These instructions lie between the Do and Loop statements.


# ExitDo (Exit Do Statement)


The ExitDo statement provides an alternate way to exit a Do...Loop. Any number of ExitDo statements may be placed anywhere in the Do...Loop. ExitDo statements are often used with the evaluation of some condition (for example, If...Then). When a Do Loop is exited, control is transferred to the statement immediately following the Loop. When Do...Loop statements are nested, control is transferred to the Do...Loop that is one nested level above the loop in which the ExitDo occurs.


# Loop


The Loop statement ends a Do...Loop control structure.
```
