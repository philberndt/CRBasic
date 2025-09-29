# WatchdogTimer (Enable User-Programmed Watchdog Timer)

The WatchdogTimer function allows the CRBasic program to guard itself against lockup by enabling a user-programmed watchdog timer. The value returned by the WatchdogTimer is the remaining time in seconds until the expiration of the last programmed interval.

## Syntax

WatchdogTimer(Interval,Units)

The following program demonstrates the use of the WatchdogTimer instruction.

```
'Declare Variables
Public wdog_timer As Long
Public strobe_wdog As Boolean
Public disable_wdog As Boolean
BeginProg
strobe_wdog = True
Scan(1,sec,0,0)
If strobe_wdog Then
wdog_timer =WatchdogTimer(5,sec)
EndIf
If disable_wdog Then
wdog_timer = WatchdogTimer(0,sec)
EndIf
NextScan
EndProg
```

## Remarks

The Datalogger has several internal WatchdogTimers that monitor communication and measurement tasks and in the event that a task takes too long (i.e., something has gone wrong), the datalogger will watchdog and reboot. This guards against lock ups and keeps the system running. However, programs may contain user-programmed tasks that are not monitored by the built-in internal watchdog timers. If an unmonitored task gets stuck, the datalogger could lock up and not be able to recover. The WatchdogTimer function may be used to safeguard against this scenario.

To implement the Watchdog timer function, the function is called with the interval parameter set to a non-zero value. If the function is not called again before the last programmed time expires, a watchdog error will occur, and the system will reset. If a zero interval is programmed, the watchdog timer is disabled, and no watchdog will occur.

The WatchdogTimer returns the time in Seconds to the expiration of the last programmed WatchdogTimer interval. This is useful for the programmer to establish the desired WatchdogTimer interval and find the optimal set point to prevent lock ups.

If WatchdogTimer is called with no parameters:

Wdog_Sec = WatchdogTimer()

It will return the time left before expiring, in seconds, without resetting the timer.

**WARNING:** The WatchdogTimer function should be used with caution. Care should be taken to not put the datalogger into an endless loop of watchdogging.

## Parameters

# Interval

The Interval parameter is a constant or variable that designates the time interval between WatchdogTimer scans. Enter 0 to disable the WatchdogTimer and no timeout will occur.

Type: Integer, Constant or Variable

# Units

Specifies units for the WatchdogTimer interval. Valid units are

| Code | Description |
| ---- | ----------- |
| Sec  | seconds     |
| Min  | minutes     |
| Hr   | hours       |
| Day  | days        |
| Mon  | months      |

Type: Constant
