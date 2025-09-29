# PreserveOneVariable (Preserve One Variable)

The PreserveOneVariable instruction is used to retain in memory the last known value for a specfic variable or variable array declared by the Dim or Public statements. To preserve the value of all variables in the program, see the [PreserveVariables](preservevariables.md) instruction. This instruction is supported in Operating System v.6 or later.

## Syntax

PreserveOneVariable(VariableName)

In the following example program, the PreserveOneVariable instruction is placed after the declaration of the variables that are to be preserved.

The program can be tested manually by using Numeric Monitor to set the Flag variable high (which, in turn, sets control port, C1, high) and also set each of the Preserve\_ variables to a numeric value. If power is removed from the datalogger and reapplied, the values of variables specified in the PreserveOneVariable instructions are preserved.

```
Public PTemp
Public Flag As Boolean, PortHigh AS Boolean
Public Preserve_All(2), Preserve_One(2), Preserve_None(2)'Set these Preserve variables to any number between 1 and 7999 via numeric monitor
PreserveOneVariable(Flag)'Preserve value of Boolean variable, Flag
PreserveOneVariable(Preserve_All)'Preserve both elements of this array
PreserveOneVariable(Preserve_One(2))'Preserve just the last element of this array

DataTable (Test,True,-1)
Sample (1,Flag,FP2)
EndTable

BeginProg
Scan (1,Sec,3,0)
PanelTemp (PTemp,4000)
If Flag
PortHigh=True
Else
PortHigh=False
EndIf
PortSet (C1,PortHigh)
CallTable (Test)
NextScan
EndProg
```

## Remarks

The PreserveOneVariable instruction is similar to PreserveVariables except that PreserveOneVariable preserves only the specified variable or array instead of all variables in the program. Hence, PreserveOneVariable is more efficient than PreserveVariables because it can be used to preserve only specific variables that the programmer deems necessary. Each variable or array that should be preserved requires a separate PreserveOneVariable instruction.

Another difference between PreserveOneVariable and PreserveVariables is that PreserveOneVariable allows a variable to be preserved even if the program changes; i.e., if the declaration of other variables (Public, Dim, local variables in functions), or data table fields change between program versions, the variable is still preserved as long as the name, type and size of the variable remain the same. Hence, if a program is run with the same PreserveOneVariable as the previous running program, the variable s value is preserved if the program is run with the option to retain data (Do not delete data). If the Delete Data option is chosen, the variable is reinitialized.

The PreserveOneVariable instruction is placed after the declaration of the variable to be preserved. Note that PreserveOneVariable preserves the value that has been written to the variable only. It does not preserve intermediate values that are held in memory, such as the temporary values used to calculate an average over an output interval. Also, the variable must be copied from RAM to flash memory. The fastest this copy process can run is 10 seconds. If the CR300 is reset before the variable is copied to the flash memory, the current value of the variable may be lost. For example, if you are running a 1 second scan and the CR300 loses power, the variable is restored to what is was when it was last saved, which may be 10 seconds ago. Further, the additional time required for this copy process may result in skipped scans. Finally, some of the flash memory that is normally used for data tables will instead be used for PreserveOneVariable.
