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
VoltDiff (Volt1,1,mv5000C,1,True,200,15000,1.0,0)

PortSet (C1,0)
NextScan
EndProg
```

## Remarks

The Delay instruction is used to delay the measurement task sequence or the processing instructions for the time period specified by the Delay and Units arguments, before progressing to the next measurement or processing instruction.

The Scan Interval should be sufficiently long to process all measurements plus the delay period. If the delay is applied to the measurement task sequence and the scan interval is not long enough to process all measurements plus the delay, the program will not compile when sent to the datalogger. If the delay is applied to the processing task sequence, the program will compile but scans will be skipped.

## Parameters

# Option

When a program is compiled in Pipelinemode, the Option parameter determines whether the delay should apply to the measurement task sequence or the processing instructions. In Sequentialmode, instructions are executed sequentially as they occur in the program, regardless of Option. Right-click the parameter to display a list of options.

| Code | Description                                                                                                                                                                                                           |
| ---- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| 0    | Delay will affect the measurement task sequence. Processing will continue to take place as needed in the background. When this option is chosen, the Delay instruction must not be placed in a conditional statement. |
| 1    | Delay will affect processing. Measurements will continue as called for by the task sequencer.                                                                                                                         |
| 2    | Delay will affect digital measurements. This option is used to insert a delay between successive accesses to an SDM device.                                                                                           |

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
