# TimeIntoInterval/IfTime (Time into Interval)

The TimeIntoInterval instruction is used to return a logic level of True or False based on the datalogger's real-time clock at the start of a scan. This instruction is also known as IfTime. Either keyword can be used within the program.

## Syntax

Variable="TimeIntoInterval(TintoInt,Interval,Units)

or

IfTimeIntoInterval(TintoInt,Interval,Units)

The following example uses the TimeIntoInterval instruction as the condition to be evaluated to determine whether to set Control Port 1 High or Low. When the time is 0 into a 120 second interval (or every 2 minutes) the control port is set High. The port is High for 60 seconds, until the second TimeIntoInterval instruction evaluates True and the port is set Low (i.e., 60 seconds into a 120 second interval).

### Example #1

This program was written for aCR300, but other dataloggers can use similar code (voltage ranges, channel numbers, or other parameters may need to be changed to reflect the specifications of the datalogger).

```
Public PortOn

BeginProg
Scan (1,Sec,3,0)
IfTimeIntoInterval(0,120,sec) Then PortOn=1'at 2 minute intervals, set PortOn to 1
IfTimeIntoInterval(60,120, sec) Then PortOn=0'at 1 minute into 2 minute intervals, set PortOn to 0
Portset (C1, PortOn) 'When PortOn = 1, port is set high
NextScan
EndProg
```

### Example #2

The following program uses the IFTime instruction to send data to a device with PakBus ID 4094 each hour.

```
Public PTemp, TCTemp

DataTable (Test,True,-1)
DataInterval (0,1,Min,10)
Sample (1,TCTemp,FP2)
EndTable

BeginProg
Scan (1,Sec,3,0)
PanelTemp (PTemp,4000)
TCDiff (TCTemp,1,mv34,1,TypeT,PTemp,True,0,4000,1.0,0)

IFIFTime(0,1,Hr) Then
SendData (ComRS232,0,4094,Test)
EndIf
calltable (Test)
NextScan
EndProg
```

## Remarks

When encountered by the datalogger program, the TimeIntoInterval statement is evaluated True (-1) or False (0) based on the datalogger's real-time clock at the start of a scan. Time is kept internally by the datalogger as the elapsed time since January 1, 1990, at 00:00:00 hours (thus, a weekly interval; i.e., an offset into 168 hours, begins Monday at 0000 hours). When the Interval divides evenly into this elapsed time, the TimeIntoInterval is set True. These instructions should always be placed within a Scan/NextScan or within a subroutine or function that is called from within a Scan/NextScan in order to function properly.

The TimeIntoInterval instruction can be used to set the value of a variable to -1 or 0 (first syntax example), or it can be used as an expression for a Condition (second syntax example). Note that this function will return true only once at the start of the period when the time condition is met; for example, with a 1 second scan rate, a 0 into a 5 minute interval is set to true once at the top of every 5 minutes (not 60 times at the beginning of the 5 minute period). An exception to this is when the interval is greater than 60 minutes with subsecond scans. In this case, resolution is 1 second so all subsecond scans during the second in which TimeIntoInterval/IfTime is true will return true.

**NOTE:** TimeIntoInterval must be placed within a scan to function.

## Parameters

# TintoInt (Time into Interval)

Allows an offset to the specified Interval. The Units for time are the same as for the Interval. This parameter must be an integer. Use of non-integers may result in the interval not evaluating as True when expected. In the DataInterval instruction, this parameter must be a constant (a variable is not allowed). If a variable is used in this parameter, it is recommended to define it as a Long.

TintoInt parameter example usage: if the Interval is set at 60 minutes, and TintoInt is set to 5, then data storage will occur at 5 minutes into the hour, every hour, based on the datalogger's real-time clock. If the TintoInt is set to 0, data storage will occur at the top of the hour.

Type: Constant (required for DataInterval instruction) or Variable (declared as Long, recommended).

# Interval

How frequently the TimeIntoInterval (or IfTime) statement will be evaluated True, based on the datalogger's real-time clock. This parameter must be an integer. If a variable is used in this parameter, it is recommended to define it as a Long. Use of non-integers may result in the interval not evaluating as True when expected.

Type: Constant or Variable (declared as Long, recommended)

# Units

The Units parameter specifies the units on which the TintoInt and Interval arguments will be based. The options are msec (milliseconds), sec (seconds), min (minutes), hr (hours), day (days), or mon (month).

When days are used, TintoInt starts on a Monday (since January 1, 1990 fell on a Monday). For example, TimeIntoInterval(0,7,days) would perform a function every Monday. TimeIntoInterval(1,7,days) would perform a function every Tuesday, and so on.

When month (mon) is used for the Units parameter, the Interval parameter is specified as an integer ranging from 0 (January) to 11 (December), and the TIntoInt parameter is specified as seconds into the month. The month Interval is synchronized to the beginning of the year (January at 00:00 hours). When the dataloggers real-time clock reaches 00:00 on the last day of the specified month, the month increments by 1, at which point the month interval condition is True. For example, consider the case where the month interval is specified as 9 (October). When the dataloggers real-time clock reaches 00:00 on October 1st, month changes to 10. 10 mod 9 = 1, so the True condition is met.

The TintoInt parameter with mon chosen as the units denotes either seconds before the next month, or seconds into the next month, depending on whether a negative or positive value is entered. For instance, TimeIntoInterval(-60, 9, Mon) is true for one second, one minute before October 1st. Conversely, TimeIntoInterval(60, 9, Mon) is true for one second, one minute after October 1st begins. Note that TintoInt values must align with your scan interval. If your scan interval is one minute, TintoInt should be set to 60 seconds or a multiple thereof to trigger a true condition.

**NOTE:** When using month as the Interval unit, any month other than January results in the True condition occurring twice a year; once at the start of January and once at the end of the specified month interval.

Type: Constant
