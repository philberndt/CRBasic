# StationName (Datalogger Station Name)

The StationName declaration is used to assign a name to the datalogger station.

## Syntax

StationName( StaName )

In the following example, the StationName instruction is used to assign a station name to the datalogger. The data table access function Tablename.Fieldname is used to retrieve the name from the datalogger and store it in a variable defined as a string, which can then be output to a data table.

```
Public LoggerID as String * 20
StationName=MtLoganCR300

DataTable (TestData,1,-1)
Sample (1, LoggerID, String)
EndTable

BeginProg
Scan (1,Sec,3,0)
LoggerID=Status.StationName(1,1)
CallTable (TestData)
NextScan
EndProg
```

## Remarks

The assigned StationName is stored in the datalogger's status table. StationName is limited to 64 characters. This instruction should be placed with the other declarations at the beginning of the datalogger program.

**NOTE:** This station name is different than the station name that is stored in the header of a collected data file. The station name found in the \*.dat file header is the name assigned to the station in the datalogger communication software's device map.
