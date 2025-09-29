# DaylightSaving, DaylightSavingUS

The DaylightSaving and DaylightSavingUS functions are used to determine whether or not the current datalogger time has just crossed into or out of daylight saving time (United States or custom) and set the new time automatically if desired.

## Syntax

_variable _ = DaylightSaving(DSTSet,DSTnStart,DSTDayStart,DSTMonthStart,DSTnEnd,DSTDayEnd,DSTMonthEnd,DSTHour)

or

- variable\* = DaylightSavingUS(DSTSet)

The following example program shows the use of the DaylightSaving function. The time is adjusted automatically for the daylight saving period, based upon the parameters of the function (start = second Sunday in March; end = first Sunday in November; transition time is 2 AM).

```
Public DSTStatus as Long
Const Second = 2
Const Sunday = 1
Const March = 3
Const First = 1
Const November = 11
Const TwoAM = 2

BeginProg
Scan (1,Sec,3,0)
'enter other program instructions

DSTStatus=DaylightSaving(-1,Second,Sunday,March,First,Sunday,November,TwoAM)
'Comment out the preceding line and uncomment the following line to use DaylightSavingUS instead of DaylightSaving.
'DSTStatus=DaylightSavingUS(-1)

NextScan
EndProg
```

## Remarks

These two functions return 3600 when the datalogger crosses over into the daylight saving time window recognized by the region; otherwise, the functions return 0. The parameters for the DaylightSaving function are used to specify the daylight saving period. The DaylightSavingUS function assumes the period as recognized in the U.S. (and takes into account the changes made for 2007). These functions will also adjust the UTC Offset setting in the datalogger, if it is being used.

These functions adjust the clock only when the datalogger crosses the daylight saving time boundary. Downloading a program with this instruction to a datalogger running on standard time when it should be running on daylight saving time will not change the datalogger time.

## Parameters

# DSTSet (Automatically Adjust for Daylight Savings Time)

Used to specify whether or not the datalogger should automatically adjust its clock for daylight savings time. Enter -1 to adjust the clock automatically; otherwise, enter 0.

Type: Constant, integer, or variable evaluating to -1 or 0

# DSTnStart (Daylight Saving Time Occurrence of Day Start)

Specifies the occurrence of the day (DSTDayStart) that daylight saving begins. Right-click the parameter to display a list of options:

| Code | Occurrence |
| ---- | ---------- |
| 1    | First      |
| 2    | Second     |
| 3    | Third      |
| 4    | Fourth     |
| 5    | Last       |

Type: Constant, integer, or variable

# DSTDayStart (Daylight Saving Time Start Day)

Specifies the day of the week on which daylight saving begins. Right-click the parameter to display a list of options:

| Code | Day       |
| ---- | --------- |
| 1    | Sunday    |
| 2    | Monday    |
| 3    | Tuesday   |
| 4    | Wednesday |
| 5    | Thursday  |
| 6    | Friday    |
| 7    | Saturday  |

Type: Constant, integer, or variable

# DSTMonthStart (Daylight Saving Time Month Start)

Specifies the month on which daylight saving begins. Right-click the parameter to display a list of options.

| Code | Description |
| ---- | ----------- |
| 1    | January     |
| 2    | February    |
| 3    | March       |
| 4    | April       |
| 5    | May         |
| 6    | June        |
| 7    | July        |
| 8    | August      |
| 9    | September   |
| 10   | October     |
| 11   | November    |
| 12   | December    |

Type: Constant, integer, or variable

# DSTnEnd (Daylight Saving Time Occurrence of Day End)

Specifies the occurrence of the day that daylight saving ends. Right-click the parameter to display a list of options:

| Code | Occurrence |
| ---- | ---------- |
| 1    | First      |
| 2    | Second     |
| 3    | Third      |
| 4    | Fourth     |
| 5    | Last       |

Type: Constant, integer, or variable

# DSTDayEnd (Daylight Saving Time Day End)

Specifies the day of the week on which daylight saving ends. Right-click the parameter to display a list of options:

| Code | Day       |
| ---- | --------- |
| 1    | Sunday    |
| 2    | Monday    |
| 3    | Tuesday   |
| 4    | Wednesday |
| 5    | Thursday  |
| 6    | Friday    |
| 7    | Saturday  |

Type: Constant, integer, or variable

# DSTMonthEnd (Daylight Saving Time Month End)

Specifies the month on which daylight saving ends. Right-click the parameter to display a list of options.

| Code | Description |
| ---- | ----------- |
| 1    | January     |
| 2    | February    |
| 3    | March       |
| 4    | April       |
| 5    | May         |
| 6    | June        |
| 7    | July        |
| 8    | August      |
| 9    | September   |
| 10   | October     |
| 11   | November    |
| 12   | December    |

Type: Constant, integer, or variable

# DSTHour (Daylight Saving Time Hour Begin/End)

Specifies the hour at which daylight saving begins/ends. Right-click the parameter to display a list of options:

| Code | Time                       |
| ---- | -------------------------- |
| 0    | midnight, beginning of day |
| 1    | 1 AM                       |
| 2    | 2 AM                       |
| 3    | 3 AM                       |
| 4    | 4 AM                       |
| 5    | 5 AM                       |
| 6    | 6 AM                       |
| 7    | 7 AM                       |
| 8    | 8 AM                       |
| 9    | 9 AM                       |
| 10   | 10 AM                      |
| 11   | 11 AM                      |
| 12   | 12 AM                      |
| 13   | 1 PM                       |
| 14   | 2 PM                       |
| 15   | 3 PM                       |
| 16   | 4 PM                       |
| 17   | 5 PM                       |
| 18   | 6 PM                       |
| 19   | 7 PM                       |
| 20   | 8 PM                       |
| 21   | 9 PM                       |
| 22   | 10 PM                      |
| 23   | 11 PM                      |
| 24   | midnight, end of day       |
