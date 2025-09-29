# TableName.EventEnd (End Data Table Storage Event)

The TableName.EventEnd syntax is used to determine when a data storage event ends for a DataTable.

## Syntax

TableName.EventEnd(1,1)

This program uses the TableName.EventEnd syntax to determine when a data storage event has reached completion. Three type T thermocouples are measured. The trigger for the start of a data event is when TC(1) exceeds 30 degrees C, after which the stop trigger is set immediately true.

```
Public EventEnded as Boolean

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

PanelTemp(RefTemp,4000)
TCDiff (TC,3,mv34,1,TypeT,RefTemp,True,0,4000,1.0,0)

CallTable Evnt
EventEnded =Evnt.EventEnd(1,1)

NextScan
EndProg
```

## Remarks

TableName.EventEnd can only be used for a DataTable that uses the [DataEvent](dataevent.md) instruction. The name of the event-driven table is inserted in place of TableName. TableName is limited to 20 characters. TableName.EventEnd = -1 (true) during a scan when the last record of the data storage event occurs and = 0 (false) during all other scans.

For additional data table access functionality, see [Data Table Access](../Info/datatableaccess.md).
