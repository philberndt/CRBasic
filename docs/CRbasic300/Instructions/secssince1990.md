# SecsSince1990 (Seconds Since 1990)

The SecsSince1990 function returns the number of seconds elapsed since January 1, 1990 from a date string.

## Syntax

_Variable_ = SecsSince1990(Source,DateOption)

### Example #1

In the following example program, the SecsSince1990 function is used as the "RecsBack" parameter for the GetRecord instruction to retrieve a record 5 seconds prior to the current record. The retrieved record is stored into a second data table.

The quotes are stripped from the retrieved record string so that two sets of quotes are not stored in the data table.

```
Public RefTemp, TCTemp
Public SSDate As String * 25, SS1990 As Long
Public RecRetrieved As String * 50, RecRetrievedStripped As String * 50

DataTable (TempData,True,-1)
Sample (1,RefTemp,FP2)
Sample (1,TCTemp,FP2)
EndTable

DataTable (TempData_5,True,-1)
Sample (1,RecRetrievedStripped,String)
EndTable

BeginProg
Scan (1,Sec,3,0)
'Measurements
PanelTemp (RefTemp,4000)
TCDiff (TCTemp,1,mv34,1,TypeT,RefTemp,True,0,4000,1.0,0)

'Store current scan timestamp in variable SSDate
'Option 4 uses YYYY-MM-DD HH:MM:SS
SSDate=Status.TimeStamp(1,1)
'Use SecsSince1990 function to store SSDate as long so it can
'be used in GetRecord
SS1990=SecsSince1990(SSDate,1)
'Get record 5 seconds back
GetRecord (RecRetrieved,TempData,-(ss1990-5))
'Strip quotes off string before storing it into the data table
RecRetrievedStripped=Left(Mid(RecRetrieved,2,19)+Mid(RecRetrieved,22,20),31)

CallTable (TempData)
CallTable (TempData_5)
NextScan
EndProg
```

### Example #2

In the simple following program example, the SecsSince1990 function is used to return a date from the timestamp in the Public table. In once instance, the variable is formatted as a string, so a textual date is returned. In the other instance the variable is formatted as a long, so a serial date is returned.

```
Public SSDateString As String * 25
Public SSDateLong As Long
Public Tref

DataTable (SS1990Tab,True,-1)
Sample (1,SSDateString,String)
Sample (1,SSDateLong,Long)
EndTable

BeginProg
Scan (1,Sec,3,0)
PanelTemp (Tref,4000)
SSDateLong=SecsSince1990(Public.Timestamp,1)'variable is formatted as Long
SSDateString=SecsSince1990(Public.Timestamp,1)'variable is formatted as String
CalltAble (SS1990Tab)
NextScan
EndProg
```

## Remarks

The variable in which the number of seconds is stored should be formatted as Long or as a String. If the source parameter is a date/time string and Variable is of type Long, the number of seconds since 1990 is returned. If the source parameter is a Long containing the number of seconds since 1990 and Variable is of type String, a date/time string is returned.

One of the uses for this function is to retrieve a record from a data table using the GetRecord instruction based on the time the record was stored rather than based on a record number. (See the example program.)

The SecsSince1990 function should not be nested within another function when the expected result is a string (for example, the code _DateTime(1) = Left(SecsSince1990(Public.Timestamp(1,1),1),19)_ will not work).

## Parameters

# Source

The variable to be acted upon by the function. It can be a variable formatted as a String that contains a date and time, or a variable formatted as a Long that contains the number of seconds since January 1, 1990.

Type: Variable (String or Long)

# DateOption

Specifies the format of the date/time string used as the source or the format of the date/time string to be returned by the function. Note: The resolution of this function is 1 second. Fractional seconds are not used during conversion from the date/time string to seconds since 1990. If they are included, the timestamp will be truncated. Right-click the parameter to display a list of options:

| Option | Time Stamp Element                                                                                                                                                                              |
| ------ | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| 1      | MM/dd/yyyy HH:mm:ss                                                                                                                                                                             |
| 2      | ddd, dd MMM yyyy HH:mm:ss GMT Note: Greenwhich Mean Time if UTC Offset setting is set. If UTC Offset is not set, time returned will be datalogger time and GMT will be omitted from the string. |
| 3      | "dd/MM/yyyy HH:mm:ss"                                                                                                                                                                           |
| 4      | "yyyy-MM-dd HH:mm:ss"                                                                                                                                                                           |
| 5      | "yyyy-MM-dd_HH-mm-ss"                                                                                                                                                                           |
| 6      | "yyyy-MM-dd_HH-mm-ss.ff"                                                                                                                                                                        |

Note: Fractional seconds are always reported as ".00".

Where:

- dd = Day of Month, 01 31

- ddd = day of week (Mon, Tue, Wed, etc)

- ff = Fractional second; always reported as 00

- HH = Hour, 00 - 23

- MM = Month, 01- 12

- MMM = Abbreviated name of the month (Jan, Feb, etc.)

- mm = Minute, 00 - 59

- ss = Seconds, 00 - 59

- yyyy = Year, 0000 - 9999

- GMT = Designation that time displayed is Greenwich Mean Time

Type: Constant
