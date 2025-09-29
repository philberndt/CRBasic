# TableName.TableSize (Size Allocated for DataTable)

The TableName.TableSize syntax is used to return the number of records that have been allocated in the program for a DataTable.

## Syntax

TableName.TableSize(1,1)

This example program uses the TableName.TableSize syntax to raise Flag(1) high when the DataTable is full.

```
Public Flag
Dim TRef'Declare Reference Temp variable
Public Heater
Alias Heater = deg F

DataTable(TEMP,True,500)'Trigger, 500 records
DataInterval(0,10,msec,50)
Sample (1,Heater,FP2)'1 Reps,Source,Res
EndTable

BeginProg'Program begins here
Scan(10,msec,0,0)

PanelTemp(TRef,15000)
TCDiff(Heater,1,mv200C,1,TypeT,TRef,True,0,15000,1.8,32)

'________________________ Table Full Logic________________________
'Test if table TEMP record count is equal or greater than table size
Flag = IIF(TEMP.Record(1,1) >=TEMP.TableSize(1,1),True,False)

CallTable TEMP
NextScan
EndProg
```

## Remarks

The name of the DataTable is entered in place of the TableName parameter. TableName is limited to 20 characters. The size, in records, of the DataTable defined by the TableName parameter is returned.

For additional data table access functionality, see [Data Table Access](../Info/datatableaccess.md).
