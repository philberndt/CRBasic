# CurrentSE (Single-Ended Current)

The CurrentSE instruction is used to make a single-ended current measurement onSE1 or SE2. The output is in milliamps.

## Syntax

CurrentSE(Dest,Reps,Range,SEChan,MeasOff,SettlingTime,fN1,Mult,Offset)

Following is a program showing the use of the CurrentSE instruction.

```
'Declare Variables
Public Cur_SE
DataTable(Hourly,True,-1)
DataInterval(0,60,Min,0)
Sample(1,Cur_SE,IEEE4)
Average(1,Cur_SE,IEEE4,0)
Minimum(1,Cur_SE,IEEE4,0,0)
Maximum(1,Cur_SE,IEEE4,0,0)
EndTable

BeginProg
Scan(1,Sec,1,0)
'Generic CurrentSE measurement:
CurrentSE(Cur_SE,1,mv34,1,True,0,60,1.0,0.0)
CallTable(Hourly)
NextScan
EndProg
```

## Remarks

The datalogger has built-in shunt resistors onSE1 and SE2that allow the datalogger to measure a 4 to 20 mA sensor.

Terminals SE1 and SE2 each have an internal resistance of 140 Ω which is placed in the current measurement loop. This instruction measures the current of a single channel with respect to ground. The following image shows a simplified schematic of a current measurement.

The following image shows the typical wiring for a 2-wire transmitter using datalogger power.

## Parameters

# Dest (Destination)

The Variable in which to store the results of the instruction. Right-click the parameter to display a list of defined variables.

If this instruction has a Repetitions parameter and it is greater than 1, the results are stored in an array with the variable name. The array must be dimensioned large enough to hold all of the values returned from all of the Reps.

Type: Variable or Array

# Reps (Repetitions)

The number of repetitions for the measurement or instruction.

Type: Constant integer (or expression that evaluates as a constant).

For the CurrentSE instruction, measurements are made on consecutive channels. If the Reps parameter is greater than 1, the Dest parameter must be a variable array.

# Range

The expected input from the sensor. To choose the appropriate range, multiply the expected current by 100(ohms).

Right-click the parameter to display a list or enter the code directly.

| Code   | Description     |
| ------ | --------------- |
| mV2500 | -100 to 2500 mV |
| mV34   | -34 to +34 mV   |

Type: Constant

# SEChan (Singe-Ended Channel)

The single-ended channel number on which to make the first measurement. When Reps are used, subsequent measurements are made on sequential channels.

| Code | Description |
| ---- | ----------- |
| 1    | SE1         |
| 2    | SE2         |

If the SEChan number is entered as a negative value, all Reps will be performed on the same channel (burst measurement). The CR300 burst measurement sampling rate is determined by the fN1(first notch frequency) parameter.

Type: Constant

# MeasOff (Measure Offset Voltage)

Determines whether the ground offset voltage is measured before the measurement is made on the analog channel. If the ground offset is measured it is subtracted from the sensor measurement and the result is stored in Dest.

| Value | Result                                                                                                                                               |
| ----- | ---------------------------------------------------------------------------------------------------------------------------------------------------- |
| 0     | Offset voltage is corrected from background calibration.                                                                                             |
| 1     | Offset voltage is measured each scan (this option effectively increases the measurement time as if an additional rep were added to the instruction). |

Type: Constant

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

## Related Sensors

This instruction is used by the following sensors:

- 4-20 mA Current Transmitter

- CURS100

**NOTE:** This is not an all-inclusive list of sensors that require this instruction. Refer to the owners manual of your device for more specific instructions.
