# OpenInterval (Begin a Measurement Interval)

The OpenInterval instruction is used to modify what measurements are included in the time series processing instructions of an interval driven DataTable. When this instruction is inserted in a DataTable declaration, time series processing will include all measurements since the last time data storage occurred.

## Syntax

OpenInterval

In the following example, 3 thermocouples are measured every 500 milliseconds. Every 10 seconds,**while Flag(1) is true\***,\* the averages of the reference and thermocouple temperatures are output. The user can toggle Flag(1) to enable or disable the output. Without the OpenInterval instruction, the first averages output after Flag(1) is set high would include only the measurements within the previous 10 second interval. This is the default and is what most users desire. With the Open interval in the program all the measurements made while the flag was low are included in the first averages output after the flag is set high.

```
Public RefTemp'Declare the variable used for reference temperature
Public TC(3)'Declare the variable for thermocouple measurements
Public Flag(8)
Units RefTemp=degC
Units TC=degC

DataTable (AvgTemp,Flag(1),1000)'Output when Flag(1)=true

DataInterval(0,10,sec,10)'Output every 10 seconds(while Flag(1)=true)
OpenInterval'Include data while Flag(1)=false in next average
Average(1,RefTemp,IEEE4,0)
Average(3,TC,IEEE4,0)

EndTable

BeginProg

Scan(500,mSec,0,0)
PanelTemp (RefTemp,15000)
TCDiff (TC(),3,mv200C,1,TypeT,RefTemp,True,0,15000,1.0,0)

CallTable AvgTemp
NextScan
EndProg
```

## Remarks

Typically, time series data (averages, totals, maximums, etc.) that are output to a table based on an interval only include measurements from the current interval. After each output interval, the memory that contains the measurements for the time series data is cleared. If an output interval is missed (because all criteria are not met for output to occur), the memory is cleared the next time the table is called, unless the OpenInterval instruction is contained in the DataTable declaration. The use of OpenInterval results in all measurements being included in the time series data since the last time data was stored (even though the data may span multiple output intervals)
