# TableName.EventEnd (End Data Table Storage Event)

The TableName.EventEnd syntax is used to determine when a data storage event ends for a DataTable.

## Syntax

TableName.EventEnd(1,1)

This program uses the TableName.EventEnd syntax to determine when a data storage event has reached completion. Three type T thermocouples are measured. The trigger for the start of a data event is when TC(1) exceeds 30 degrees C, after which the stop trigger is set immediately true.This is done to set a fixed size for the data storage event that can be duplicated in the worst case tables.

```
Const NumCases = 3'Number of Worst Cases to save
Const Max = 1
Public I, NumAbove30'Declare index and the ranking variable

Public RefTemp'Declare the variable for ref temperature
Public TC(3)'Declare the variable for TC measurements

Units RefTemp=degC
Units TC=degC

DataTable (Evnt,1,125)
DataInterval(0,0,msec,10)'Set the sample interval equal to the scan
DataEvent(20,TC(1)>30,-1,100)'20 records before TC(1)>30,
'100 records after TC(1)>30
Sample(1,RefTemp,IEEE4)'Sample the reference temperature
Sample(3,TC,IEEE4)'Sample the 5 thermocouple temperatures
EndTable

BeginProg
Scan(500,mSec,0,0)

PanelTemp(RefTemp,15000)
TCDiff (TC,3,mv200C,1,TypeT,RefTemp,True,0,15000,1.0,0)

CallTable Evnt

IfEvnt.EventEnd(1,1)'Check if an Event just Ended
I=100'Initialize Index
NumAbove30=0'Zero Ranking Variable
Do'Loop through the Event table
NumAbove30=NumAbove30+1'Counting the # of times TC(1)>30
I=I-1
Loop While I>0 and Evnt.TC(1,I)>=30'Quit looping when at end or 'TC(1)<30
WorstCase(Evnt,NumCases,Max,0,NumAbove30)'Check for worst case
EndIf
NextScan
EndProg
```

## Remarks

TableName.EventEnd can only be used for a DataTable that uses the [DataEvent](dataevent.md) instruction. The name of the event-driven table is inserted in place of TableName. TableName is limited to 20 characters. TableName.EventEnd = -1 (true) during a scan when the last record of the data storage event occurs and = 0 (false) during all other scans.

For additional data table access functionality, see [Data Table Access](../Info/datatableaccess.md).
