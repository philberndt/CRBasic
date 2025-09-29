# Timer (Initiate Timer)

The Timer instruction is used to initiate a timer in the datalogger program.

## Syntax

variable =Timer(TimerNo,TUnits,TOption)

This example uses Timer to measure the time required to execute the measurement instructions. The Timer is reset at the top of each scan. The time in microseconds is stored in the variable Elapse. Note that the newer [InstructionTimes](instructiontimes.md) function is an alternative method to measure processing times.

```
Public RefTemp, TCTemp, Elapsed

DataTable (Test,True,-1)
DataInterval (0,1,Min,10)
Sample (1,TCTemp,FP2)
Sample (1,Elapsed,IEEE4)
EndTable

BeginProg
Scan (1,Sec,3,0)
Timer(1,uSec,2)
PanelTemp (RefTemp,4000)
TCDiff (TCTemp,1,mv34,1,TypeT,RefTemp,True,0,4000,1.0,0)

Elapsed =Timer(1,uSec,4)
calltable (Test)
NextScan
EndProg
```

## Remarks

A typical use of the Timer is to set a variable equal to the Timer instruction (i.e., Variable = Timer(1, sec, 2). The Timer value is then stored in the defined variable. Multiple Timers can be defined in the program; each must have a unique TimerNo argument. **NOTE:** When a timer uses microseconds, the data logger cannot enter quiescent (low-power) mode, which increases power consumption. To reduce power use, stop the timer after its purpose is served.

Microsecond timers use a 32-bit counter with 1 s resolution.The maximum trackable time is approximately 2147 seconds (~35 minutes). However, results are stored as a 32-bit float with 24 bits of precision, so precision is reduced after about 8 seconds. ** NOTE:** When using Timer units other than microseconds (e.g., msec, sec, min, hr), elapsed time is measured in milliseconds internally and converted to the specified unit when the timer is read.

While the timer is running, the elapsed milliseconds are accumulated using a signed 32-bit integer, which has a maximum value of 2,147,483,647 milliseconds (approximately 24.85 days). If the timer is not reset before this limit is reached, the counter will roll over, and the timer will begin returning negative values.

To prevent rollover and ensure accurate timer values, use TimeOpt = 2 or 3 to reset the timer at least once every 24 days.

## Parameters

# TimerNo (Timer Number)

A number assigned to the timer (for example, 0, 1, 2, ). Memory is allocated for timers based on the value entered + 1 (for example, using a TimerNo of 100 will allocate memory for 101 timers even if there is only one Timer in the program). Use a low number to conserve memory.

If this value is a variable, memory for 17 (0 - 16) timers is allocated. If more than 17 timers are needed, and TimerNo also needs to be a variable, use a "dummy" Timer instruction, which does not execute at run-time, with a TimerNo of the maximum number of timers needed in the program.

Type: Integer or Variable

# TUnits (Time Units)

The TUnits argument is the time unit in which to report the Timer. A numeric or alphabetical code can be entered.

| Code | Alphanumeric Code | Description  |
| ---- | ----------------- | ------------ |
| 0    | usec              | microseconds |
| 1    | msec              | milliseconds |
| 2    | sec               | seconds      |
| 3    | min               | minutes      |
| 4    | hr                | hours        |

Type: Constant

# TimeOpt (Timer Action)

The action on the Timer. The timer instruction returns the value of the timer after the action is performed. A numeric code is entered. Right-click the parameter to display a list.

| Code | Description     |
| ---- | --------------- |
| 0    | Start           |
| 1    | Stop            |
| 2    | Reset and start |
| 3    | Stop and reset  |
| 4    | Read only       |

Type: Constant
