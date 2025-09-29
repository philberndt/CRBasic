# TableName.TimeStamp (Table Timestamp)

The TableName.TimeStamp syntax is used to return the timestamp of a data table record, expressed either as a time into an interval or as a date/time string.

## Syntax

_Variable_ = TableName.TimeStamp(m,n)

The following program stores a sample of each timestamp option available in the Tablename.Timestamp program syntax.

```
Public DateTime(6) As String * 32
Public TintoInt(8) As Long
Dim I

DataTable (Test,1,1000)
DataInterval (0,1,Sec,10)
Sample (6,DateTime,String)
Sample (8,TintoInt,Long)
EndTable

BeginProg
Scan (1,Sec,0,0)
'Get a sample of each timestamp format
'when destination is of type String
For I = 6 To 1 Step -1
DateTime(I) =Public.Timestamp(I,1)
Next I
'Get a sample of each timestamp format
'when destination is of type Long
For I = 0 To 7
TintoInt(I+1) =Public.Timestamp(I,1)
Next I
CallTable Test
NextScan
EndProg
```

## Remarks

The TableName.TimeStamp(m,n) syntax returns the time into an interval or a timestamp for the record "n" number of records ago. The name of the DataTable is entered in place of the TableName parameter. The type of timestamp returned is based on the option specified for "m" and whether the format of the destination variable is of type Float/Long or type String. The timestamp returned has a 10 msec resolution.

### Destination Variable Formatted as Type Float or Long

The Long data type should be used to prevent loss of precision when working with large integer values.

| m   | Timestamp Format                     |
| --- | ------------------------------------ |
| 0   | seconds since 1970                   |
| 1   | seconds since 1990                   |
| 2   | seconds into the current year        |
| 3   | seconds into the current month       |
| 4   | seconds into the current day         |
| 5   | seconds into the current hour        |
| 6   | seconds into the current minute      |
| 7   | microseconds into the current second |

### Destination Variable Formatted as Type String

The destination variable should be formatted as a string large enough to hold the contents of the formatted timestamp.

| m   | Timestamp Format as Date/Time String                                                                                                                                                 |
| --- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| 1   | MM/dd/yyyy HH:mm:ss.ff                                                                                                                                                               |
| 2   | ddd, dd MMM yyyy HH:mm:ss GMT Note: Greenwich Mean Time if UTC Offset setting is set. If UTC Offset is not set, time returned is datalogger time and GMT is omitted from the string. |
| 3   | "dd/MM/yyyy HH:mm:ss.ff"                                                                                                                                                             |
| 4   | "yyyy-MM-dd HH:mm:ss.ff" Note: fractional seconds are only included if non-zero.                                                                                                     |
| 5   | "yyyy-MM-dd_HH-mm-ss.ff" Note: fractional seconds are only included if non-zero.                                                                                                     |
| 6   | "yyyy-MM-dd_HH-mm-ss.ff" Note: fractional seconds are always included.                                                                                                               |

Where:

- dd = Day of Month, 01 31

- ddd = Day of Week, Sun - Sat

- ff = Fractional second, 00 - 99

- HH = Hour, 00 - 23

- MM = Month, 01- 12

- MMM = Abbreviated name of the month (Jan, Feb, etc.)

- mm = Minute, 00 - 59

- ss = Seconds, 00 - 59

- yyyy = Year, 0000 - 9999

- GMT = Designation that time displayed is Greenwich Mean Time

For additional data table access functionality, see [Data Table Access](../Info/datatableaccess.md).
