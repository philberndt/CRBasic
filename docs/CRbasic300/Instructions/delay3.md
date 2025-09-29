# Delay

The Delay instruction is used to delay the program for a specified period of time.

## Syntax

```
Delay
```

(Option,Delay,Units)

This example uses the Delay instruction to delay the program for 20 milliseconds after a port is turned on and before a voltage measurement is made.

```
Dim Volt1

BeginProg
Scan (1,Sec,3,0)
PortSet (C1 ,1 )
Delay(0,20,mSec)
VoltDiff (Volt1,1,mv2500,1,True,200,4000,1.0,0)

PortSet (C1,0)
NextScan
EndProg
```

## Remarks

The Delay instruction is used to delay the measurement and processing instructions for the time period specified by the Delay and Units arguments, before progressing to the next measurement or processing instruction. Depending upon the option, SlowSequences can also be delayed or they can be allowed to run.The Scan Interval should be sufficiently long to process all measurements plus the delay period.

## Parameters

# Option

Used to set the option for the delay instruction. Right-click the parameter to display a list.

| Code | Description                                                      |
| ---- | ---------------------------------------------------------------- |
| 0    | Delay measurements, processing, and slow sequences.              |
| 1    | Delay measurements and processings; allow slow sequences to run. |

# Delay

The time to delay the program at the designated place (10 microsecond resolution). The time units are selected by the Units argument.

Type: Constant (or expression that evaluates as a constant).

# Units

For the Delay instruction, the Units parameter is the time units for the delay. A numeric code is entered. Right-click the parameter to display a list.

| Code | Alphanumeric Code | Description  |
| ---- | ----------------- | ------------ |
| 0    | usec              | microseconds |
| 1    | msec              | milliseconds |
| 2    | sec               | seconds      |
| 3    | min               | minutes      |
| 4    | hr                | hours        |
| 5    | day               | days         |

Type: Constant
