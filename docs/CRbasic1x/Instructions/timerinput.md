# TimerInput (Time Between Edges on Digital Ports)

The TimerInput instruction is used to measure the time between edges (state transitions) or frequency on digital ports of the datalogger, including the time between edges or frequency that spans the scan interval.

## Syntax

TimerInput(Dest,StartChan,Edge,Function,Timeout,Timeout_Units)

The following example program measures frequency on the falling edge of C1.

```
'Declare Variables
Public TimeIn(2)
Public scan_cnt

'TimerInput(dest,startchan,edge,function,timeout,units)
'function 1 -> period
'2 -> frequency
'3 -> time since previous
BeginProg
Scan(1000,msec,10,0)
scan_cnt = scan_cnt + 1
TimerInput(TimeIn(1),C1,0,2,0,usec)
NextScan
EndProg
```

## Remarks

This instruction measures the timing of the edges on the control ports. The instruction can be used in Low Frequency Mode (<1 kHz) for edge timing, period, and frequency measurements or in High Frequency Mode (up to about2.3kHz) for edge counting only. The edge timing resolution is500nS. Time values are always returned in μsec. This instruction will return NAN if the timeout expires or if a signal on a channel is too fast.

## Parameters

# Dest

The Dest parameter is a variable array in which to store the results of the measurement. The array must be dimensioned equal to the number of ports for which results are requested.

Type: Variable or Array

# StartChan

The channel to use as a reference for edge timing for this instruction.

| Code | Description          |
| ---- | -------------------- |
| C1   | Control Port 1 and 2 |

Type: Constant

# Edge

The Edge parameter is a digit (0 or 1) that represents each of the ports, from left to right: C8 .C1. If a 1 is entered for a port, the transition will be counted on the rising edge (from <1.5V to >3.5V). If a 0 is entered, the transition will be counted on the falling edge (from >3.5V to <1.5V). For example, getting a rising edge on C8 starting at C1 would be done with: TimerInput(Dest,C1,10000000,20000000,0,usec).

| Digit | Edge    |
| ----- | ------- |
| 0     | falling |
| 1     | rising  |

Type: Constant

# Function

The Function parameter is a value where each digit represents the function for the channel associated with that digit (see Edges, above). Leading 0s do not have to be used if you are using only the first few channels for the instruction. For example, measure the frequency on the falling edge of C1 could be specified with either: TimerInput(Dest,C1,0,2,0,usec) or TimerInput(Dest,C1,0000,0002,0,usec).

| Function | Result                                                                                                                                                                                                 |
| -------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| 0        | The associated channel is not used.                                                                                                                                                                    |
| 1        | Calculate the period of the signal on the specified channel (in microseconds).                                                                                                                         |
| 2        | Calculate the frequency (Hz) for the specified channel.                                                                                                                                                |
| 3        | Calculate the time from an edge on the previous channel (1 number lower) to an edge on the specified channel (in microseconds)(even channels only, for example,time from C4 since C3, or C8 since C7). |
| 5        | Return the count of edges since the last execution.                                                                                                                                                    |

Type: Constant

# Timeout

Used to determine the results to be returned when no changes have occurred in the specified ports since the last execution of the instruction. If the timeout period has expired and no change has occurred, a null value will be returned (0 for frequency and NaN for period or time since last edge). If the timeout period has not expired the last valid result will be returned. This is useful for measuring signals with periods greater than the scan interval. The maximum Timout is 2^32 x the scan. Typically, a timeout value is at least one scan interval. However, 0 can be entered to make the calculation each time the instruction is executed. A timeout less than the scan interval will default to the scan interval.

Type: Constant (or expression that evaluates as a constant) or Variable

# Timeout_Units

The units for the Timeout parameter. Right-click the parameter to display a drop-down list box.

| Code | Description  |
| ---- | ------------ |
| μsec | microseconds |
| msec | milliseconds |
| Sec  | seconds      |
| Min  | minutes      |

Type: Constant
