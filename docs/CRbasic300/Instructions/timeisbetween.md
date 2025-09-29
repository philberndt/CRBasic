# TimeIsBetween (Datalogger Clock Time is Between)

The TimeIsBetween function is used to determine if the datalogger's real-time clock falls within a range of time.

## Syntax

TimeIsBetween(BeginTime,EndTime,Interval,Units)

### Example #1

This simple example uses the TimeIsBetween function to set a flag high when the datalogger s clock falls between the top of a minute and 15 seconds after the minute.

```
Public MyFlag As Boolean

BeginProg
Scan (1,Sec,3,0)

IfTimeIsBetween(0,15,60,sec) Then
MyFlag = True
Else
MyFlag = False
EndIf

NextScan
EndProg
```

### Example #2

In this exampleCR300program, TimeIsBetween is used as the trigger for data table storage. Data is stored to the table for the first 5 minutes of every hour.

While the following code is for theCR300, similar code could be used for other dataloggers (the measurement channel and voltage ranges may need to be changed).

```
Public RefTemp, TCTemp

DataTable (First5,TimeIsBetween(0,5,60,min),9999 )
Sample (1,RefTemp,FP2)
Sample (1,TCTemp,FP2)
EndTable

BeginProg
Scan (1,Sec,3,0)
PanelTemp (RefTemp,4000)
TCDiff (TCTemp,1,mv34,1,TypeT,RefTemp,True,0,4000,1.0,0)

CallTable First5
NextScan
EndProg
```

## Remarks

The TimeIsBetween function returns True (-1) if the datalogger's real-time clock falls within the specified range; otherwise, the function returns False (0). TimeIsBetween can be used to control a process or task within the program (for instance, to control power to a device).

In this example, a modem is set to turn on between the hours of 9:00 a.m. and 5:00 p.m.

See [Use Time Intervals for More than Storing Data](https://www.campbellsci.com/blog/do-more-than-store-data) for more information.

## Parameters

# BeginTime (Begin Time)

Enter the start of the time range to be used for the function. BeginTime is included in the range of time that will return True.

Type: Constant or Variable

# EndTime (End Time)

Enter the end of the time range to be used for the function. EndTime is not included in the range of time that will return True.

Type: Constant or Variable

# Interval

The Interval parameter is used to define how frequently the TimeIsBetween statement will evaluate as True, based on the datalogger's real-time clock. This parameter must be an integer. If a variable is used in this parameter, it is recommended to define it as a Long. Use of non-integers may result in the interval not evaluating as True when expected.

Type: Constant or Variable (declared as Long, recommended)

# Units

Defines the unit of time to be used for the BeginTime, EndTime, and Interval. Right-click the parameter to display a list.

**NOTE:** When days are used, the first interval starts on a Monday. For example, TimeIsBetween(0,5,7,day) would perform a function every Monday through Friday.

| Code | Description  |
| ---- | ------------ |
| usec | microseconds |
| msec | milliseconds |
| sec  | seconds      |
| min  | minutes      |
| hr   | hours        |
| day  | days         |

Type: Constant
