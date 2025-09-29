# TableName.TableFull (Determine if Table is Full)

The TableName.TableFull syntax is used to indicate whether a fill and stop table is full or whether a ring-mode table has begun overwriting its oldest data.

## Syntax

TableName.TableFull(1,1)

In the following program example, two thermocouples are measured every second and the sample temperatures are stored in a DataTable named Temp1Sec. The DataTable, which is in fill and stop mode, is filled every 15 minutes. When the table becomes full the TableFull syntax evaluates True (-1) and control port 1 is set high (on).

```
'Declaration of Variables
Public Panel
Public TCTemp(2)
Public TableStatus'Variable to hold true/false value for TableFull syntax
Public PortControl'Variable to hold State for Port

'Table Definition
DataTable (Temp1Sec,1,900)'Create table holding 15 mins of data
DataInterval (0,1,Sec,10)'1 second interval
FillStop'table will stop when full
Sample (2,TCTemp(1),IEEE4)
EndTable

'Program
BeginProg
Scan (1,Sec,3,0)

PanelTemp (Panel,15000)
TCDiff (TCTemp,1,mv5000C,1,TypeT,Panel,True,0,15000,1.0,0)

CallTable Temp1Sec
TableStatus=Temp1Sec.TableFull(1,1)'Store result of TableFull in TableStatus Variable
If TableStatus = -1'Set PortControl variable to 1 if table is full
PortControl=1
Else
PortControl=0'Set PortControl variable to 0 if table is not full
EndIf
Portset (C1,PortControl)'Evaluate PortControl variable to determine whether port should be on or off
NextScan
EndProg
```

## Remarks

The TableName.TableFull syntax will evaluate as True or False based on the fill status of the table.

| Table Type    | Returned Value | Description                                        |
| ------------- | -------------- | -------------------------------------------------- |
| Ring Mode     | 0 (False)      | Table has not begun overwriting the oldest data.   |
| Ring Mode     | -1 (True)      | Table has begun overwriting the oldest data.       |
| Fill and Stop | 0 (False)      | Table is not full.                                 |
| Fill and Stop | -1 (True)      | Table is full and data is no longer being written. |

The name of the DataTable is entered in place of the TableName parameter. TableName is limited to 20 characters.

For additional data table access functionality, see [Data Table Access](../Info/datatableaccess.md).

**NOTE:** A fill and stop table that is full (and to which data is no longer being written) can be reset under program control using [ResetTable](resettable.md). A table can be reset manually usingLoggerNet

**Note:** Campbell Scientific's datalogger support software for programming, communications, and data retrieval between dataloggers and a computer.

.
