# TableName.FieldName (Access a Field from a Table Record)

The TableName.FieldName syntax is used to access a specific field from a record in a table. For dataloggers with integrated CELL modems, this instruction can also be used to monitor daily and monthly data usage in kilobytes [(seeMonitoring Cellular Diagnostics and Data Usage Programmaticallyfor more information)](Monitoring%20Cellular.md).

## Syntax

TableName.FieldName(FieldNameIndex,RecordsBack )

This example uses Tablename.fieldname to populate a text string with the latest value from the table Test.

```
Public PTemp, TCTemp(2)
Public Text As String * 30

DataTable (Test,True,-1)
DataInterval (0,1,Min,10)
Sample (2,TCTemp(1),FP2)
EndTable

BeginProg
Scan (1,Sec,3,0)

PanelTemp (PTemp,4000)
TCDiff (TCTemp,2,mv34,1,TypeT,PTemp,True,0,4000,1.0,0)

Text="Current temperature is " +Test.TCTemp(1)
calltable (Test)
NextScan
EndProg
```

## Remarks

TableName is the name of the table where the value to be accessed is stored. The table can be a user-defined data table or the status table (a value from the public table can be accessed in this way too, but it is simpler to merely use the variable name). TableName is limited to 20 characters. FieldName is the name of the field in the table, and it is always an array even when it consists of only one variable. FieldNameIndex specifies the array variable from which to retrieve data. RecordsBack specifies the number of records back in the DataTable the desired value resides (1 is the most recent record stored). A negative number can be entered for the RecordsBack parameter to specify the time, in seconds since 1990, for the record to be retrieved (refer to the [SecsSince1990](secssince1990.md) example program for similar usage of a negative RecsBack parameter in the GetRecord instruction).

| **NOTE:** Each output processing instruction other than Sample() appends a 3 character suffix to the fieldname in the data table. See [Output Instruction Suffixes](../Info/outputinstructionsuffixes1.md). This suffix must be included when using TableName.FieldName. You can view the fieldnames for a program opened in the CRBasic Editor by selecting Tools | Show Tables from the menu. (The program must be saved before table information can be displayed.) |

If the FieldNameIndex and the RecordsBack is not specified, then (1,1) is assumed. If the FieldNameIndex is specified, but the RecordsBack is not, (FieldNameIndex,1) is assumed.

If the variable holding the returned value is formatted as a float or a long and the field being accessed is a timestamp, the time is reflected in seconds since 1990. However, if FieldNameIndex is entered as a negative value, then time is reflected in usec since 1990. This time value can be converted to a standard datalogger timestamp if the variable is declared as a Long and it Sampled into a table using the NSEC data format.

If the variable is formatted as a string, the format of the returned timestamp can be set using the same syntax as the [TableName.TimeStamp](tablenametimestamp.md) instruction. Note, however that if the variable is a multi-dimensional variable, this formatting will only work if the FieldNameIndex (i.e., the format parameter) is greater than the dimension of the variable.

### Example

Tdiff = Temp.TC_Avg(1,1) - Temp.TC_Avg(1,101)

The difference between the current value and the value recorded 101 records back for the TC_Avg(1) field in the Temp table is stored in the variable Tdiff.

For additional data table access functionality, see [Data Table Access](../Info/datatableaccess.md).
