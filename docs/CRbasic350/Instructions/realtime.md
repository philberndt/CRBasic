# RealTime (Datalogger Clock Time)

The RealTime instruction is used to derive the year, month, day, hour, minute, second, microsecond, day of week, and day of year from the datalogger clock and store the results in an array.

## Syntax

RealTime(Dest)

This example uses RealTime to place all time segments in the VALUES table. If the remark ( ' ) is removed from the first 9 Sample statements and the last Sample statement is remarked, the results will be exactly the same.

```
Public rTime(9)'declare as public and dimension rTime to 9
Alias rTime(1) = Year'assign the alias Year to rTime(1)
Alias rTime(2) = Month'assign the alias Month to rTime(2)
Alias rTime(3) = DOM'assign the alias DOM to rTime(3)
Alias rTime(4) = Hour'assign the alias Hour to rTime(4)
Alias rTime(5) = Minute'assign the alias Minute to rTime(5)
Alias rTime(6) = Second'assign the alias Second to rTime(6)
Alias rTime(7) = uSecond'assign the alias uSecond to rTime(7)
Alias rTime(8) = WeekDay'assign the alias WeekDay to rTime(8)
Alias rTime(9) = Day_of_Year'assign the alias Day_of_Year to rTime(9)

DataTable ( VALUES, 1, 100 ) 'set up data table
DataInterval( 0, 10, mSec, 0 )'set up data table
'Sample( 1, Year, IEEE4 )'place Year in VALUES table
'Sample( 1, Month, IEEE4 )'place Month in VALUES table
'Sample( 1, DOM, IEEE4 )'place DOM in VALUES table
'Sample( 1, Hour, IEEE4 )'place Hour in VALUES table
'Sample( 1, Minute, IEEE4 )'place Minute in VALUES table
'Sample( 1, Second, IEEE4 )'place Second in VALUES table
'Sample( 1, uSecond, IEEE4 )'place uSecond in VALUES table
'Sample( 1, WeekDay, IEEE4 )'place WeekDay in VALUES table
'Sample( 1, Day_of_Year, IEEE4 )'place Day_of_Year in VALUES table
Sample( 9, rTime( ), IEEE4 )'place all 9 segments in VALUES table
EndTable

BeginProg
Scan ( 10, mSec, 1, 0 )
RealTime( rTime )
CallTable VALUES
NextScan
EndProg
```

## Remarks

The RealTime instruction loads the destination array (Dest argument) with the current time values in the following order: (1) year, (2) month, (3) day of month, (4) hour of day, (5) minutes, (6) seconds, (7) microseconds, (8) day of week (1-7; Sunday = 1), and (9) day of year. The destination array must be dimensioned to 9. The time returned is the time of the datalogger's clock at the beginning of the scan in which the RealTime instruction occurs.

If RealTime is run within a scan, the time reflected by the instruction will be the time of the datalogger's clock when the scan was started. If RealTime is run outside of a scan (for instance, within a Do Loop), the time reflected by the instruction will be based on the system clock, which has a 10 msec resolution.

**NOTE:\*\***TableName.Timestamp\*\* syntax can be used to return the timestamp of a data table record, expressed either as a time into an interval (for example seconds since 1970 or seconds since 1990) or as a date/time string.[For more information, seeTableName.TimeStamp (Table Timestamp).](tablenametimestamp.md)

## Parameter

# Dest (Destination)

The variable array in which to store the results of the RealTime instruction. The array must be dimensioned to 9 to include all of the current time values in the following order: (1) year, (2) month, (3) day of month, (4) hour of day, (5) minutes, (6) seconds, (7) microseconds, (8) day of week (1-7; Sunday = 1), and (9) day of year. The destination array must be dimensioned to 9. The time returned is the time of the datalogger's clock at the beginning of the scan in which the RealTime instruction occurs. Right-click the parameter to display a list of defined variables.

Type: Variable or Array
