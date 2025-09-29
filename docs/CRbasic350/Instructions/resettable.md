# ResetTable (Reset Table)

The ResetTable instruction is used to reset a Data table under program control.

## Syntax

ResetTable(TableName)

The following program uses ResetTable to reset table Test when Flag(1) becomes high.

```
Public RefTemp, TCTemp, Flag(1)

DataTable (Test,True,-1)
DataInterval (0,1,Min,10)
Sample (1,TCTemp,FP2)
EndTable

BeginProg
Scan (1,Sec,3,0)
If Flag(1) ThenResetTable(Test)
PanelTemp (RefTemp,4000)
TCDiff (TCTemp,1,mv34,1,TypeT,RefTemp,True,0,4000,1.0,0)

CallTable (Test)
NextScan
EndProg
```

## Remarks

When the ResetTable instruction is executed, all records are erased from the Data table specified by the TableName argument.
