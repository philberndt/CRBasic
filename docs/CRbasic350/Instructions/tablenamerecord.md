# TableName.Record (Determine DataTable Record Number)

The TableName.Record syntax is used to determine the record number of a specific DataTable record.

## Syntax

TableName.Record(1,n)

This example program uses the TableName.Record syntax to put a filemark in the DataTable every 100 records.

```
Public Heater
Units Heater = Deg_F

Public Flag
Dim TRef
Public RecData

DataTable(MAIN,True,-1)
DataInterval(0,50,msec,100)
Sample(1,Heater,FP2)
EndTable

BeginProg
Scan(50,msec,0,0)

PanelTemp(TRef,4000)'RefTemp,Integrate
TCDiff(Heater,1,mv34,1,TYPET,TRef,False,0,4000,1.8,32)

CallTable MAIN

RecData =Main.record(1,1)'RecData = record # of current record
If RecData Mod 100 = 0 Then'Conditional to put a filemark every 100 records
FileMark(Main)
Flag = -1
Else
Flag = 0
EndIf

NextScan
EndProg
```

## Remarks

This syntax returns the record number of the specified number of records prior to the current record. The name of the DataTable is entered in place of the TableName parameter. TableName is limited to 20 characters. The number of records to go back in the DataTable is specified by n.

For additional data table access functionality, see [Data Table Access](../Info/datatableaccess.md).
