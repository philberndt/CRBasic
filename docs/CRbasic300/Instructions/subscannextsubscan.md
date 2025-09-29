# SubScan NextSubScan (Control Speed for Some Measurements)

The SubScan instruction is used to measure some analog inputs at a faster rate than the program scan or control an AM16/32 multiplexer.

## Syntax

```
SubScan
```

(SubInterval,Units,Count)

```
NextSubscan
```

In the following program, the SubScan/Next SubScan instructions are used to execute the VoltDiff instruction at a different interval than the remainder of the program.

```
Public BatteryV, volt1

BeginProg
Scan (30,Min,3,0)
Battery (BatteryV)
SubScan(15,mSec,10000)
VoltDiff (Volt1,1,mv2500,1,True ,0,4000,1.0,0)

Next SubScan
NextScan
EndProg
```

## Remarks

The SubScan/NextSubScan instructions are placed within the Scan/NextScan instructions of a program. The resolution of the SubScan interval is 10 msec resolution.

## Parameters

# SubInterval (Interval Between Subscans)

Designates the time interval between the subscans. Enter 0 for no delay between subscans.

Type: Constant

# Units

The units for the interval parameter. An alphabetical code is entered. Right-click the parameter to display a list.

| Alphanumeric Code | Description  |
| ----------------- | ------------ |
| usec              | microseconds |
| msec              | milliseconds |
| sec               | seconds      |
| min               | minutes      |

Type: Constant

# Count

Determines the number of times the subscan will run each time the scan runs. Maximum number is 65,535.

Type: Constant integer

**Notes**:

- SubScans cannot be nested.

- If a SubScan is placed in a [SlowSequence](slowsequence.md), the Interval parameter must be 0 (this facilitates the measurement of multiplexers in a SlowSequence).

- [PulseCount](pulsecountpulsecountreset.md) measurements cannot be used within a SubScan.

- Measurements cannot be made in a subroutine that is called from a SubScan with an interval.
