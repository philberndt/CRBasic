# VoltDiff (Differential Voltage Measurement)

The VoltDiff instruction is used to make a differential voltage measurement on one or more analog channels. The output is in millivolts.

## Syntax

```
VoltDiff
```

(Dest,Reps,Range,DiffChan,RevDiff,SettlingTime,fN1,Mult, Offset)

Following is a program showing the use of the VoltDiff instruction.

```
'CR350 SeriesDatalogger

Public DiffVolt

DataTable(Hourly,True,-1)
DataInterval(0,60,Min,0)
Sample(1,DiffVolt,IEEE4)
Average(1,DiffVolt,IEEE4,0)
Minimum(1,DiffVolt,IEEE4,0,0)
Maximum(1,DiffVolt,IEEE4,0,0)
EndTable

BeginProg
Scan(1,Sec,1,0)
'Generic Differential Voltage measurements DiffVolt:
VoltDiff(DiffVolt,1,mv2500,1,True,0,4000,1.0,0.0)
CallTable(Hourly)
NextScan
EndProg
```

## Remarks

This instruction measures the voltage difference between the high and low inputs of a differential channel. Both the high and low inputs must be in the range of -100 mV to +5000 mV relative to the datalogger's ground.

## Parameters

# Dest (Destination)

The Variable in which to store the results of the instruction. Right-click the parameter to display a list of defined variables.

If this instruction has a Repetitions parameter and it is greater than 1, the results are stored in an array with the variable name. The array must be dimensioned large enough to hold all of the values returned from all of the Reps.

Type: Variable or Array

# Reps (Repetitions)

The number of repetitions for the measurement or instruction.

Type: Constant integer (or expression that evaluates as a constant).

For the VoltDiff instruction, measurements are made on consecutive channels. If the Reps parameter is greater than 1, the Dest parameter must be a variable array.

# Range

The voltage range for the measurement or input sensor. An alphanumeric code is entered. Right-click the parameter to display a list or enter the code directly.

| Code   | Description     |
| ------ | --------------- |
| mV2500 | -100 to 2500 mV |
| mV34   | -34 to +34 mV   |

Type: Constant

# DiffChan (Differential Channel)

Specifies thedifferential channelon which to make the first measurement. If the Reps parameter is greater than 1, additional measurements will be made on sequential channels.

If the DiffChan number is entered as a negative value, all Reps are performed on the same channel (burst measurement). TheCR350burst measurement samplingrateis determined by the fN1(first notch frequency) parameter.

| fN1 (Hz) | Burst Sampling Rate                   |
| -------- | ------------------------------------- |
| 50 or 60 | 50 mSec (or 20 samples per second)    |
| 400      | 6.25 mSec (or 160 samples per second) |
| 4000     | 0.5 mSec (or 2000 samples per second) |

Type: Constant

# Reverse Differential (RevDiff)

A constant value is entered to determine whether the inputs are reversed and a second measurement made. This removes any voltage offset errors due to the datalogger measurement circuitry, including inputrangeerrors. Enabling this parameter doubles the measurement time. Right-click to display a list.

| Logic      | Description                                                                                  |
| ---------- | -------------------------------------------------------------------------------------------- |
| False or 0 | Signal is measured with the high side referenced to the low. Do not make second measurement. |
| True or 1  | Reverse input and make second measurement.                                                   |

Type: Constant or Variable

# Settling Time

The settling time is the duration (in microseconds) to allow for signal settling after setting up a measurement (switching to the channel, setting the excitation) and before making the measurement.The default settling time is 500 microseconds, with a minimum of 10 microseconds and a maximum of 50000 microseconds.

Type: Constant (or expression that evaluates as a constant)

# fN1

Determines the lowest frequency that will be eliminated or notched out by the sinc filter. This filter notches out frequencies at integer multiples of fN1by averaging for a time equal to 1/fN1; thus, lower fN1frequencies result in longer measurement times.

| Option | Description                                                                       |
| ------ | --------------------------------------------------------------------------------- |
| 50     | Standard measurement speed; requires 50 msec to filter 50 Hz noise                |
| 60     | Standard measurement speed; requires 50 msec to filter 60 Hz noise                |
| 400    | Medium measurement speed; performs a 6.25 msec integration to filter 400 Hz noise |
| 4000   | Fast measurement speed; performs a 0.5 msec integration to filter 4000 Hz noise   |

Type: Constant

# Mult, Offset (Multiplier and Offset)

Factors by which to scale the raw results of the measurement. Typically used to convert the raw measurement to engineering units or to units other than which is output. For example, the TCDiff instruction measures a thermocouple and outputs temperature in degrees C. A multiplier of 1.8 and an offset of 32 will convert the temperature to degrees F.

For temperature measurements, a multiplier (mult) of 1 and an offset of 0, would output in degrees Celsius. For analog measurements, a multiplier (mult) of 1 and an offset of 0, would output the measured voltage in millivolts divided by the excitation voltage in volts.

If Repetitions of greater than 1 are used for this instruction, Repetitions can also be used for the Multiplier and Offset.[SeeMultipliers, Offsets, and Disable Variables with Repetitionsfor more information.](../Info/multipliersoffsets.md)

Type: Constant, Variable, Array, or Expression
