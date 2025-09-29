# CallTable (Call Table)

The CallTable instruction is used to call a Data table.

## Syntax

```
CallTable
```

Name

In the following program code, the CallTable instruction is used to transfer program control to the METDATA table. If the trigger condition for the table is True, the table is processed. Control is returned to the main program after processing is complete.

```
'Define program variables
Public Windsp, Rain, Time(9)

'Define the DataTable, METDATA
DataTable (METDATA,1,1000)'Trigger for METDATA is True; allocate 1000 records
DataInterval (0,1,Min,10)'Store data on 1 minute intervals
Sample (1,WINDSP,FP2)
Totalize (1,RAIN,FP2,False )
EndTable

'Main program- Read datalogger real-time clock
'Measure 2 pulse count channels and Call DataTable
BeginProg
Scan (1,Sec,3,0)
RealTime (TIME)
PulseCount (Windsp,1,C1,1,1,1.0,0)
PulseCount (Rain,1,C2,1,0,1.0,0)
CallTableMETDATA'Call to METDATA DataTable
NextScan
EndProg
```

## Remarks

The CallTable instruction is used in the main program to call a Data table. The Data table must appear in the Declaration section of the program, before the BeginProg statement (i.e., before the beginning of the main program). When the Data table is called, it will check the output condition and process data as programmed.
