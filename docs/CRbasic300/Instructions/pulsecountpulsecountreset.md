# PulseCount/PulseCountReset (Pulse Count)

The PulseCount instruction is used tomake measurement on a pulse channel.

## Syntax

```
PulseCount
```

(Dest,Reps,PChan,PConfig,POption,Mult, Offset)

```
PulseCountReset
```

( ) optional

The following code provides an example of using the PulseCount and PulseCountReset instructions in a program. The program measures a wind sensor and stores the result in miles per hour.

```
'Declare Variables and Units
Public Batt_Volt
Public WS_mph

Units Batt_Volt=Volts
Units WS_mph=miles/hour

'Define Data Tables
DataTable(Table1,True,-1)
DataInterval(0,1,Min,0)
Sample(1,WS_mph,FP2)
EndTable

'Main Program
BeginProg
'zero all pulse counters before starting program
PulseCountReset
Scan(5,Sec,1,0)
'Default Datalogger Battery Voltage measurement Batt_Volt:
Battery(Batt_Volt)
'014A Wind Speed Sensor measurement WS_mph:
PulseCount(WS_mph,1,P_LL,1,1,1.789,1.0)
If WS_mph<1.01 Then WS_mph=0
'Call Data Tables and Store Data
CallTable(Table1)
NextScan
EndProg
```

## Remarks

With the PulseCount instruction you can make pulse measurements on the datalogger as follows:

- A control port can be configured as high frequency input (up to 3 kHz) or a switched pulse counter (up to 150 Hz, requires 100K 12V pull-up resistor).

- Single-ended channels 1 through 4 can be configured as high frequency (up to 35 kHz).

- The pulse channel P_SW can be configured as high frequency (up to 35 kHz) or as a switched pulse counter (up to 150 Hz).

- The pulse channel P_LL can be configured as high frequency input (up to 20 kHz) or low level AC.

The PulseCount instruction must be executed once before the pulse port is ready for input. This may be of particular concern for programs with long scan intervals. For example, the PulseCount instruction will not yield a valid output until the turn of the second hour if the PulseCount instruction is used within a program with a scan interval of 1 hour.

The 24-bit counter can count up to 16,777,216.Each scan of the datalogger causes the counter to output the number of pulses accumulated since the last scan. If the scans stop, as in a program with more than one scan loop in the Main Program, or in a program that calls an external subroutine, the counter continues to accumulate counts until the Scan with the PulseCount instruction is reentered. PulseCountReset is used to reset both the pulse counter and the running average values in the pulse count instruction, though in most instances there is no need to manually reset the counter. The only use would be if two separate scans were using the same channel for a PulseCount, then the counter would need to be reset before entering each scan.

For cases involving Scans that have subroutine calls that include a Scan/NextScan sequence, the PulseCountReset must be placed in a conditional statement prior to the PulseCount instruction, and the Subroutine call must be placed after the PulseCount instruction. If possible, calls to DataTables that store the results from the PulseCount instruction should be placed prior to the Subroutine call.

**NOTE:** This instruction should NOT be placed inside a conditional statement, subroutine, subscan, or [SlowSequence](slowsequence.md) scan. To ensure all pulses are detected, it must be executed each time the main program is executed.

## Parameters

# Dest (Destination)

The Variable in which to store the results of the instruction. Right-click the parameter to display a list of defined variables.

If this instruction has a Repetitions parameter and it is greater than 1, the results are stored in an array with the variable name. The array must be dimensioned large enough to hold all of the values returned from all of the Reps.

Type: Variable or Array

# Reps (Repetitions)

The number of repetitions for the measurement or instruction.

Type: Constant integer (or expression that evaluates as a constant).

Measurements are made on consecutive channels. If the Reps parameter is greater than 1, the Dest parameter must be a variable array.

# PChan (Pulse Channel)

The number of the pulse channel for the measurement. When Reps are used, subsequent measurements will be automatically made on the following channels. Right-click to display a list.

| Code | Description            |
| ---- | ---------------------- |
| C1   | Control terminal 1     |
| C2   | Control terminal 2     |
| SE1  | Single ended channel 1 |
| SE2  | Single ended channel 2 |
| SE3  | Single ended channel 3 |
| SE4  | Single ended channel 4 |
| P_SW | Pulse Channel P_SW     |
| P_LL | Pulse Channel P_LL     |

Type: Constant

# PConfig (Pulse Channel Configuration)

A code specifying how the pulse channel should be configured. Right-click the parameter to display a list.

**NOTE:** Code 0 may be used to disable the measurement during cleaning or calibration, if the PConfig parameter is a variable.

| Code | Description                                                                                            |
| ---- | ------------------------------------------------------------------------------------------------------ |
| 0    | High frequency; all channels                                                                           |
| 1    | Low level A/C: P_LL only                                                                               |
| 2    | Switch closure; C1, C2, P_SW **NOTE:** Switch closure on C1, C2 requires a 100 kΩ 12V pull-up resistor |

Type: Constant (or Variable that evaluates to 0, 1,or 2)

# POption (Pulse Option)

A code that determines if the raw result (multiplier =1, offset = 0) is returned in counts or frequency. Right-click to display a list.

| Code | Description                                                                                                                                               |
| ---- | --------------------------------------------------------------------------------------------------------------------------------------------------------- |
| 0    | Counts                                                                                                                                                    |
| 1    | Frequency in Hz; counts/scan interval in seconds                                                                                                          |
| >1   | Running average of frequency (Hz).The value entered is the time period of the running average in milliseconds and must be a multiple of the scan interval |

Type: Constant

If an overrange occurs and running averaging is in use, the overrange value is an element of the average until it has fallen off the end of the list. If a running average were set for 1000 milliseconds then an overrange will be the value from the PulseCount instruction until 1 second has passed. Use the PulseCountReset instruction to avoid this.

# Mult, Offset (Multiplier and Offset)

Factors by which to scale the raw results of the measurement. Typically used to convert the raw measurement to engineering units or to units other than which is output. For example, the TCDiff instruction measures a thermocouple and outputs temperature in degrees C. A multiplier of 1.8 and an offset of 32 will convert the temperature to degrees F.

For temperature measurements, a multiplier (mult) of 1 and an offset of 0, would output in degrees Celsius. For analog measurements, a multiplier (mult) of 1 and an offset of 0, would output the measured voltage in millivolts divided by the excitation voltage in volts.

If Repetitions of greater than 1 are used for this instruction, Repetitions can also be used for the Multiplier and Offset.[SeeMultipliers, Offsets, and Disable Variables with Repetitionsfor more information.](../Info/multipliersoffsets.md)

Type: Constant, Variable, Array, or Expression
