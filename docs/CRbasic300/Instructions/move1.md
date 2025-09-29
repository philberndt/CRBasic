# Move (Move Values)

The Move instruction is used to move the values in a range of variables into different variables or fill a range of variables with a constant.

## Syntax

```
Move
```

(Dest,DestReps,Source,SourceReps)

In the following program, the Move instruction is used to move data from the Time and GoesTime arrays, into the ClockValues array so that the datalogger clock can be set using the ClockSet instruction.

```
Public GoesTime(4), Time(9), ClockValues(7), Flag(1)

BeginProg
Scan (1,Sec,3,0)
RealTime (Time())
'If IfTime (0,1, Day) Then
If Flag(1) Then
GOESStatus (GoesTime(),0)
Move(ClockValues(),7,Time(),7)
Move(ClockValues(4),3,GoesTime(2),3)
ClockSet (ClockValues())
EndIf
NextScan
EndProg
```

## Remarks

## Parameters

# Dest (Destination)

The Variable in which to store the results of the instruction. Right-click the parameter to display a list of defined variables.

If this instruction has a Repetitions parameter and it is greater than 1, the results are stored in an array with the variable name. The array must be dimensioned large enough to hold all of the values returned from all of the Reps.

Type: Variable or Array

# Reps (Repetitions)

The number of repetitions for the measurement or instruction.

Type: Constant integer (or expression that evaluates as a constant).

For the Move instruction, the DestReps parameter is the number of variables in which the variables being moved are stored.

# Source

The name of the Variable that is the input for the instruction. Right-click the parameter to display a list of defined variables.

Type: Variable

For the Move instruction, the Source parameter is used to define the first variable from which variables will be moved. If a constant is entered here, that value is placed in each variable defined by the Dest and DestRep parameters.

# Reps (Repetitions)

The number of repetitions for the measurement or instruction.

Type: Constant integer (or expression that evaluates as a constant).

For the Move instruction, the SourceReps parameter is the number of variables that should be moved into the Dest array. This parameter should be set equal to the DestReps parameter, or, as a special case, set to 1. If this parameter is set to 1, the same value is placed in each variable of the Dest array.
