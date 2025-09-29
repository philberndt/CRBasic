# DataTime (Data Time)

The DataTime instruction is used in a data table to specify whether timestamps are recorded with the datalogger's system time at the top of the scan, or alternatively, with the datalogger's system time at the time of storing data.

## Syntax

DataTable (TableName,True,1000)

DataTime(DataTimeOpt)

'Output instructions

EndTable

In the following program, a thermocouple measurement is made every two seconds. The program includes two tables: one with the DataTime instruction set to store system time (DataTimeOn) and another to use the default of the time at the beginning of the scan (DataTimeOff). A delay is added to the program so that you can see the difference in the recorded timestamps for the data tables. Effectively, the DataTimeOn records are stored with an odd timestamp and the DataTimeOff records are stored with an even timestamp.

```
Public PTemp, TCTemp

DataTable (DataTimeOn,True,1000)
DataTime(1)
Sample (1,TCTemp,FP2)
EndTable

DataTable (DataTimeOff,True,1000)
Sample (1,TCTemp,FP2)
EndTable

BeginProg
Scan (2,Sec,3,0)
PanelTemp (PTemp,4000)
TCDiff (TCTemp,1,mv34,1,TypeT,PTemp,True,0,4000,1.0,0)

Delay (0,1,Sec)
CallTable (DataTimeOn)
CallTable (DataTimeOff)
NextScan
EndProg
```

## Remarks

By default, timestamps are recorded in data tables with the datalogger's system time at the top of the scan. However, for some applications it may be desired to record timestamps at the time of storing data, rather than at the top of the scan. The DataTime instruction is used in a data table to specify whether timestamps are recorded with the datalogger's system time at the top of the scan, or with the datalogger's system time at the time of storing data.

## Parameter

# DataTimeOpt (Timestamp Options)

Use the drop-down list in the CRBasic Editor to select one of two option codes:

| Code | Description                                                                                                     |
| ---- | --------------------------------------------------------------------------------------------------------------- |
| 0    | Scan time: Timestamps records in the table with the datalogger's system time at the top of the scan.            |
| 1    | System time: Timestamps records in the table with the datalogger's system time at the time of storing the data. |

Type: Constant

If DataTime is not used in a data table, the records are timestamped with the time at the top of the scan.

The use of DataTime does not affect the DataInterval. It still triggers based on the time at the top of the scan.
