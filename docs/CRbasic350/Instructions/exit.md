# Exit

The Exit instruction is used to completely exit from the program.

## Syntax

Exit

In the following program, if a thermocouple temperature is greater than 30 degrees C, execution of the program is stopped.

```
Public PTemp, BattVolt, TCTemp

DataTable (Test,True,-1)
DataInterval (0,1,Min,10)
Sample (1,PTemp,FP2)
Sample (1,TCTemp,FP2)
EndTable

BeginProg
Scan (1,Sec,3,0)
Battery (BattVolt)
PanelTemp (PTemp,4000)
TCDiff (TCTemp,1,mv34,1,TypeT,PTemp,True,0,4000,1.0,0)

IF TCTemp >30 ThenExit
CallTable Test
NextScan
EndProg
```

## Remarks

When the Exit instruction is processed, program execution stops. Typically, Exit is used to stop the program when a condition is met and no additional measurements or data are required. An alternative to Exit may be to set the Count parameter of the [Scan](scannextscan.md) instruction.
