# PreserveVariables (Preserve Variables)

The PreserveVariables instruction is used to retain in memory the values for all variables declared by the Dim or Public statements.To preserve the value of one variable, see the [PreserveOneVariable](preserveonevariable.md) instruction. This instruction is compatible with Operating System 6.01 or later.

## Syntax

PreserveVariables

In the following example program, the PreserveVariables instruction is placed at the beginning of the program. All variables will reflect the last known value if the datalogger experiences a power loss or the program is stopped using the Stop and Retain Data option.

The program can be tested by manually typing in values for Test() or setting Flag(4) high (which, in turn, sets high a control port). If power is removed from the datalogger and reapplied, the variables are reinitialized to the last known value.

```
PreserveVariables

Public RefTemp, TCTemp(2)
Public Flag(4) As Boolean, PortHigh AS Boolean

DataTable (TCTemp,True,-1)
DataInterval (0,1,Min,10)
Average (2,TCTemp(),FP2,False)
Maximum (2,TCTemp(),FP2,False,True)
Minimum (2,TCTemp(),FP2,False,True)
EndTable

BeginProg
Scan (1,Sec,3,0)
PanelTemp (RefTemp,4000)
TCDiff (TCTemp,1,mv34,1,TypeT,RefTemp,True,0,4000,1.0,0)

CallTable (TCTemp)
If Flag(4) Then
PortHigh=True
Else
PortHigh=False
EndIf
PortSet (C1,PortHigh )
NextScan
EndProg
```

## Remarks

The PreserveVariables instruction is placed in the declarations section of the program prior to the BeginProg instruction. When PreserveVariables is present in a program, the values of variables will not be reinitialized after a recompile (due to power loss, Constant Table change, File Control restart, etc.), provided there are no changes to the declaration of variables (Public, Dim, local variables in functions), or to data table fields. Hence, if a program is run with the same variables as the previous running program, variables values are preserved if the program is run with the option to retain data (Do not erase data files). If the Erase data files option is chosen, the variables are reinitialized.

**NOTE:** PreserveVariables preserves values that have been written to variables **on the main scan interval**. It does not preserve intermediate values that are held in memory, such as the temporary values used to calculate an average over an output interval.

** NOTE:**Because the CR300 Series does not have battery-backed RAM, PreserveVariables is handled differently in the CR300 than in other dataloggers (for example, CR6, CR3000/CR1000/CR800). In the CR300 Series, the variables must be copied from RAM to flash memory.** The fastest this copy process can run is 10 seconds**. If the CR300 is reset before the variables are copied to the flash memory, the current values of the variables may be lost. For example, if you are running a 1 second scan and the CR300 loses power, variables are restored to what they were when last saved, which may be 10 seconds ago. Further, the additional time required for this copy process may result in skipped scans. Finally, some of the flash memory that is normally used for data tables will instead be used for PreserveVariables.
