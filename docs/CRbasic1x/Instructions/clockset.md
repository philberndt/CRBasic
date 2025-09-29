# ClockSet (Set the Clock)

The ClockSet instruction is used to set the datalogger clock from the values in an array.

## Syntax

ClockSet( SourceArray )

In the following sample program, the GOESStatus and RealTime instructions are used to load an array with time values. Once a day the datalogger's clock is set, using the ClockSet instruction.

```
Public GoesTime(4), Time(9), ClockValues(7)

BeginProg
Scan (1,Sec,3,0)
'Put time values into an array
RealTime (Time())
If IfTime (0,1,Day) Then
'get GPS time
GOESStatus (GoesTime(),0)
'Load ClockValues array with Realtime values
Move (ClockValues(),7,Time(),7)
'Load ClockValues(4, 5, & 6) with GoesTime values
Move (ClockValues(4),3,GoesTime(2),3)
'set Clock
ClockSet(ClockValues())
EndIf
NextScan
EndProg
```

## Remarks

A typical use of the ClockSet instruction is when the datalogger can input the time from a more accurate clock than its own (for example, a GPS receiver). The input time would periodically or conditionally be converted into the required variable array and ClockSet would be used to set the datalogger clock.

## Parameter

# SourceArray

The Source Array that holds the time values. The Source must be a seven element array in which the year, month, day, hours, minutes, seconds, and microseconds should be contained. Array(1) through Array(7) should hold the year, month, day, hours, minutes, seconds, and microseconds, respectively.

Type:Float

**Note:** Four-byte floating-point data type. Default datalogger data type for Public or Dim variables. Same format as IEEE4.

,Long

**Note:** Data type used when declaring a variable as an integer.

, orString

**Note:** A data type used when declaring a variable consisting of alphanumeric characters.

(if string format must be yyyy-mm-dd hh:mm:ss).In Operating Systems version2and later, the string format may also be yyyy/mm/dd hh:mm:ss.
