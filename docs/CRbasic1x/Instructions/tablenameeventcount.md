# TableName.EventCount (Number of Storage Events for an Event)

The TableName.EventCount syntax is used to return the number of data storage events that have occurred for an event driven table.

## Syntax

TableName.EventCount(1,1)

In the following program, EventCount is used to keep track of the number of data storage events that occur for TmpTable. Data is stored to TmpTable when a thermocouple temperature (TCDest) is greater than 77 degrees F. The data storage event ends when TCDest drops below 77 and 5 additional records are stored, after which, the value in DataCount is incremented.

```
'Program Variables
Public TRef, TCDest, DataCount
'Event Driven Table
'Begin Storing when TCDest is greater than 77
'Store 5 records after TCDest drops below 77
DataTable (TmpTable,1,1000)
DataEvent (0,TCDest>77,TCDest<77,5)
Sample (1,TCDest,FP2)
EndTable
'Table to Record number of Data Storage Events
DataTable (CntTable,1,1000)
Sample (1,DataCount,FP2)
EndTable
BeginProg
Scan (1,Sec,3,0)

PanelTemp (TRef,15000)
TCDiff (TCDest,1,mv200C,1,TypeT,TRef,True ,0,15000,1.8,32)

CallTable TmpTable
DataCount=TmpTable.EventCount(1,1)
CallTable CntTable

NextScan
EndProg
```

## Remarks

The name of the DataTable is entered in place of the TableName parameter. TableName is limited to 20 characters. The number of data storage events that have occurred in the DataTable defined by the TableName parameter is returned.

The [DataEvent](dataevent.md) instruction must appear in the DataTable declaration for EventCount to work. The event is registered in the datalogger after the DataEvent has ended and the value specified by RecAfter has been satisfied.

For additional data table access functionality, see [Data Table Access](../Info/datatableaccess.md).
