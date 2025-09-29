# ClockChange (Clock Change)

The ClockChange function returns the number of milliseconds that the datalogger's clock has changed since the last time the function was executed. ClockChange can be used to monitor drift in the datalogger clock, or changes to the datalogger clock that occur due to clock sets by an external device, such as a GPS or a personal computer running software that connects to the datalogger, such as LoggerNet.

## Syntax

_Variable_ = ClockChange

In the following example program the ClockChange instruction is executed daily to determine if a clock change has occurred over the 24 hour period.

```
Public BattVolt, Changemsec As Long

DataTable (Clock,True,-1)
Sample (1,Changemsec,Long)
EndTable

BeginProg
Scan (1,Sec,3,0)
Battery (BattVolt)

If IfTime (0,24,Hr) Then
Changemsec=ClockChange
CallTable Clock
EndIf

NextScan
EndProg
```

## Remarks

If the datalogger's clock has not changed since the function's last execution, this function will return 0.
