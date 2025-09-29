# Restart (Restart Program)

The Restart instruction is used to restart the program under program control.

## Syntax

Restart

This example program can be used to see the results of the Restart instruction when it is executed. After the program has run for a few minutes, use software or a keyboard display to manually set the ProgramRestart variable to True. The program is recompiled, and variables, intermediate storage, and port status is reset to their default states.

```
'Declare variable to control restart and initialize to False
Public ProgramRestart As Boolean = False
'Declare Other Program Variables
Public Counter As Long, Batt_V

DataTable (Results,True,-1 )
DataInterval (0,1,Min,10)
Sample (1,Counter,Long)
Minimum (1,Counter,Long,False,False)
Maximum (1,Counter,Long,False,False)
EndTable

BeginProg
Scan (1,Sec,1,0)
Battery (Batt_V)
Counter=Counter+1
If ProgramRestart = True
Restart
EndIf
CallTable Results
NextScan
EndProg
```

## Remarks

When the Restart instruction is executed, the running program is stopped and restarted. The values in variables and intermediate storage locations are cleared, and any ports that have been set to True are reset to False unless otherwise set under program control when the program is recompiled. Data tables are not reset unless other changes were made that affect memory or data table structure, so record numbers in data tables will not be reset to 1.

This instruction is not typically required for normal program operation, but it does allow a way to restart the datalogger under program control if needed.
