# TableHide (Hide Table)

The TableHide instruction is used to suppress the display and data collection of a data table in datalogger memory.

## Syntax

DataTable (Table1,True,-1)

TableHide

_(data table instructions)_

EndTable

In the following example program, the table named "hidden" will not be available for viewing or data collection.

```
Public RefTemp

DataTable (Hidden,True,-1)
TableHide
Sample (1,RefTemp,FP2)
EndTable

BeginProg
Scan (1,Sec,3,0)
PanelTemp (RefTemp,15000)
CallTable (Hidden)
NextScan
EndProg
```

## Remarks

The TableHide instruction must be placed inside the table declaration for the table that you want hidden. If security is enabled in the datalogger the highest level of security will "unlock" the table so that it can be viewed or so that its data can be collected.

The Status and Public tables cannot be hidden. A hidden table is not included in the datalogger's table definitions when requested by software.

The following data table access functions are not affected by this instruction: SendGetVariables, GetDataRecord, GetRecord, tablename.fieldname references, and showing the table information in the Status Table).

To use SendData or SendTableDef with a hidden table, append .secured to the end of the table name (for example, Mytable.secured).
