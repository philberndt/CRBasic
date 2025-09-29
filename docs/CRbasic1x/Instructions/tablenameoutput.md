# TableName.Output (Data Output to DataTable)

The TableName.Output syntax is used to determine if data was written to a specific DataTable the last time that the DataTable was called.

## Syntax

TableName.Output(1,1)

This program example uses the TableName.Output(1,1) syntax to determine if the program has stored data in the DataTable called Main.

```
Public Heater
Units Heater = Deg_F
Public Flag
Dim TRef'Declare Reference Temp variable
Public DataOut

DataTable(MAIN,True,-1)'Trigger, auto size
DataInterval(0,50,msec,100)'50 mSec interval, 100 lapses
DataEvent(5,Heater>=89,True,10)'Event Driven Table
Sample(1,Heater,FP2)
EndTable

BeginProg
Scan(50,msec,0,0)

PanelTemp(TRef,15000) 'RefTemp
TCDiff(Heater,1,mv200C,1,TypeT,TRef,False,0,15000,1.8,32)

CallTable MAIN

DataOut =Main.output(1,1)'Set DataOut = the current value returned by the Main.Output instruction.
If DataOut = 0 Then'If DataOut = 0 (no output this scan),
Flag = false'Set Flag low (no output)
Else
Flag = true'Set Flag high (data is output)
EndIf

NextScan
EndProg
```

## Remarks

The name of the DataTable is entered in place of the TableName parameter. TableName is limited to 20 characters. TableName.Output = -1 (true) if data was written to the specified DataTable, and = 0 (false) if data was not written to the DataTable. Note that TableName.Output is set back to false when the table is called and output did not occur.

For additional data table access functionality, see [Data Table Access](../Info/datatableaccess.md).
