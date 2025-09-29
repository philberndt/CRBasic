# BrHalf3W (3-Wire Half Bridge Measurement)

The BrHalf3W iinstruction is used to make a 3 wire half bridge measurement.

## Syntax

```
BrHalf3W
```

(Dest,Reps,Range,SEChan,ExChan,MeasPEx,ExmV,RevEx,SettlingTime,fN1,Mult,Offset)

The following program shows the use of the BrHalf3W instruction to measure a PRT sensor.

```
'Example program to measure a 100 ohm PRT sensor using the
' BrHalf3W measurement instruction, where Rf = 10,000 ohms.
' The correct multiplier is determined using an ice bath.
' The PRT instruction converts the measurement result to temperature.

'Declare Variables and Units
Public HBr3W
Public RTD_C

Units HBr3W=mV
Units RTD_C=degC

'Define Data Tables
DataTable(Table1,True,-1)
DataInterval(0,60,Min,0)
Average(1,HBr3W,IEEE4,0)
EndTable

'Main Program
BeginProg
Scan(5,Sec,1,0)
'Generic Half Bridge, 3 Wire measurements HBr3W:
BrHalf3W(HBr3W,1,mv5000C,1,Vx1,1,2500,True,0,15000,100,0.0)
PRTCalc (RTD_C,1,HBr3W,1,1.0,0)
'Call Data Tables and Store Data
CallTable(Table1)
NextScan
EndProg
```

## Remarks

This instruction is used to determine the ratio of the sensor resistance to a known resistance using a separate voltage sensing wire from the sensor to compensate for lead wire resistance.

The measurement sequence is to apply an excitation voltage and make two voltage measurements on two adjacent single-ended channels: the first on the reference resistor and the second on the voltage sensing wire from the sensor. The two measurements are used to calculate the resulting value that is the ratio of the voltage across the sensor to the voltage across the reference resistor. The excitation voltage used by this instruction is turned off when another measurement instruction is run or the program reaches the end of the scan.

Relational Formula:

**NOTE:** Terminal assignments for this instruction must fall within the guidelines for universal terminal pairs.

## Parameters

# Dest (Destination)

The Variable in which to store the results of the instruction. Right-click the parameter to display a list of defined variables.

If this instruction has a Repetitions parameter and it is greater than 1, the results are stored in an array with the variable name. The array must be dimensioned large enough to hold all of the values returned from all of the Reps.

Type: Variable or Array

For the BrHalf3W instruction, the Dest parameter is a variable in which to store the results (X in the equation) of the measurement.

# Reps (Repetitions)

The number of repetitions for the measurement or instruction.

Type: Constant integer (or expression that evaluates as a constant).

For the BrHalf3W instruction, the Reps parameter is the number of times the measurement should be made. Measurements are made on consecutive channels. If the Reps parameter is greater than 1, the Dest parameter must be a variable array.

# Range

The voltage range for the measurement or input sensor. An alphanumeric code is entered. Right-click the parameter to display a list or enter the code directly.

| Code       | Description                                                                                                                           |
| ---------- | ------------------------------------------------------------------------------------------------------------------------------------- |
| mV5000     | +5000 mV                                                                                                                              |
| mV1000     | +1000 mV                                                                                                                              |
| mv200      | +200 mV                                                                                                                               |
| Autorange  | Datalogger tests for and uses most suitable range.                                                                                    |
| AutorangeC | Autorange, checks for open input.                                                                                                     |
| mV5000C    | +5000 mV, checks for open input, sets excitation to ~5600mV. Overrange values may go undetected; use code to detect overrange values. |
| mV1000C    | +1000 mV, checks for open input, sets excitation to ~1250 mV.                                                                         |
| mV200C     | +200 mV, checks for open input, sets excitation to ~1250 mV.                                                                          |

Autorange increases the time to make the measurement. For signals that do not fluctuate too rapidly, AutoRange allows the datalogger to automatically choose the input voltage range. AutoRange results in two voltage measurements being performed. The first voltage measurement is done quickly at a first notch frequency (fN1) of 50 kHz, the result of which determines the input voltage range to use for the second measurement. The second measurement is then performed using the range determined from the first measurement, along with the fN1chosen for the measurement instruction. Both measurements use the settling time chosen for the particular measurement instruction. Auto-ranging optimizes resolution but takes longer than a fixed range measurement because of the two-measurement sequence. The exception to this two-measurement sequence is if Reps are made on the same channel (Reps parameter is negative). In this instance, the test measurement is made only once. Subsequent repetitions are made with the delay between each measurement being the specified settling time. Autorange should not be used if fast measurements are required or if the analog signal could change significantly over the course of the measurement.

The C options check for an open connection on the analog input by applying a short overranging test signal to the analog input prior to making the measurement. For a voltage range of mv5000C,5.6Vis applied. For a voltage range of mv1000C or mv200C, 1.25V is applied. An open (broken) sensor will result in a measurement overrange, clearly indicating a sensor problem instead of returning a bad measured value caused by a floating input.

The open-detect option may not work properly in the presence of external leakage (< 1 MOhm) to ground, as the overrange test signal could discharge through the external leakage during the input settling time such that an overrange condition no longer exists. In this case the measurement settling time should be minimized to minimize the discharge time of the overrange test signal.

The open circuit detection (C option) can cause measurement errors for slow responding sensors, as such sensors may not have sufficient time to recover from the applied short duration (50 microseconds) test signal. For such sensors, increasing the measurement settling time beyond the default may be necessary for sufficient sensor recovery time. If measurement speed is critical with a slow responding sensor, then it may be preferable to not use the open detect (C option).

Type: Constant

# SEChan

The single-ended channel number on which to make the first measurement. When Reps are used, subsequent measurements will be automatically made on the following channels.

Type: Constant

Specify the channel to be used for the first measurement. The second measurement will be made on the next consecutive channel.

For the BRHalf3W instruction, if the SEChan number is entered as a negative value, all Reps are performed on the same channel (burst measurement). The burst measurement sampling rate is determined by the fN1(first notch frequency) parameter, which can go up to a maximum of 93750Hz (minimum sample interval of 10.667 S). The SettlingTime parameter is used to delay once prior to beginning the burst. The total time prior to beginning the burst is the SettlingTime plus theADC

**Note:** Analog to digital conversion. The process that translates analog voltage levels to digital values.

flush time (which is 810 S). The sample interval resolution is 1/93750Hz. The specified notch frequency will use the nearest multiple of (1/93750Hz) to get as close to the specified frequency as possible.

# ExChan (Excitation Channel)

Specifies theexcitationterminal to use to excite the first measurement. If the Reps parameter is greater than 1, the excitation terminal used for subsequent measurements will be incremented with each measurement based on the MeasPEx parameter.

An alphanumeric code is entered. Right-click within the parameter to display a list.

| Alphanumeric | Description          |
| ------------ | -------------------- |
| VX1          | Excitation channel 1 |
| VX2          | Excitation channel 2 |
| VX3          | Excitation channel 3 |
| VX4          | Excitation channel 4 |

Type: Constant

# MeasPEx (Excitation Channels)

The number of sensors to excite with the same excitation terminal before automatically advancing to the next excitation terminal. To excite all sensors with the same excitation terminal, the value of this parameter should be set equal to the value of the Reps parameter.

This parameter in a bridge measurement can be used when the sensors to be measured outnumber the available excitation channels. It allows multiple sensors to be excited with each excitation channel.

For example, assume it is desired to measure eight pressure transducers that have 350 ohm full bridge strain gages to be excited by 5 volts. Each transducer requires 14 mA (5 volts/350 ohms = 14 mA). Since each excitation channel can provide a maximum of 50 mA at 5 volts, a maximum of 3 pressure transducers can be excited per channel (50 mA/14 mA = 3.57). To measure the eight transducers, 8 would be entered for the Reps argument and 3 for the MeasPEx. The first three transducers would all be connected to the first excitation channel, the next 3 to the second excitation channel, and the last 2 to the third excitation channel.

Type: Constant (or expression that evaluates as a constant)

# ExmV (Excitation Voltage in mV)

The excitation, in millivolts, to apply to the excitation channel. The allowable range is4000 mV.

Type: Constant. For ExciteV() only, this parameter can also be a Variable.

# RevEx (Reverse Excitation)

A constant is entered for the RevEx argument to determine whether the excitation should be reversed and applied to the sensor after the first measurement is made. False (or 0) = Do not reverse excitation ; True (or 1) = Reverse excitation and make a second measurement (requires more time to complete). Use of RevEx cancels out voltage offsets due to the sensor, wiring, and excitation circuitry, but does not compensate for operational input voltage errors.

Type: Constant

# Settling Time

The settling time is the duration (in microseconds) to allow for signal settling after setting up a measurement (switching to the channel, setting the excitation) and before making the measurement.Minimum settling time is 20 microseconds; maximum settling time is 600 ms. If 0 is entered, a default of 500 microseconds is used.

Additional settling time may be necessary to allow the signal to settle with high resistance or long lead lengths (higher capacitance). The time it will take to make the measurement will include the measurement itself, the SettlingTime, fN1, and whether or not parameters are set to remove voltage offset errors. Using either RevDiff or RevEx causes two SettlingTimes to occur per channel; four SettlingTimes will occur when using both RevDiff and RevEx.

Type: Constant (or expression that evaluates as a constant)

# fN1

Determines the lowest frequency that will be eliminated or notched out by the sinc filter. This filter notches out frequencies at integer multiples of fN1by averaging for a time equal to 1/fN1; thus, lower fN1frequencies result in longer measurement times.Set fN1to the lowest frequency that should be filtered out. For example, setting fN1to 60 filters all multiples of 60 and above (60, 120, 180, etc.) but will not effectively filter lower frequencies; for example 30 Hz.Any value between 0.5 Hz and 31.25 kHz can be entered, but common options for filtering noise are:

| Option         | Description                                                       |
| -------------- | ----------------------------------------------------------------- |
| 15000          | Performs a 0.0667 millisecond integration (for fast measurements) |
| 60 (or \_60Hz) | Performs a 16.67 millisecond integration (filters 60 Hz noise)    |
| 50 (or \_50Hz) | Performs a 20 millisecond integration (filters 50 Hz noise)       |

Type: Constant

# Mult, Offset (Multiplier and Offset)

Factors by which to scale the raw results of the measurement. Typically used to convert the raw measurement to engineering units or to units other than which is output. For example, the TCDiff instruction measures a thermocouple and outputs temperature in degrees C. A multiplier of 1.8 and an offset of 32 will convert the temperature to degrees F.

For temperature measurements, a multiplier (mult) of 1 and an offset of 0, would output in degrees Celsius. For analog measurements, a multiplier (mult) of 1 and an offset of 0, would output the measured voltage in millivolts divided by the excitation voltage in volts.

If Repetitions of greater than 1 are used for this instruction, Repetitions can also be used for the Multiplier and Offset.[SeeMultipliers, Offsets, and Disable Variables with Repetitionsfor more information.](../Info/multipliersoffsets.md)

Type: Constant, Variable, Array, or Expression

## Measurement Time

The minimum time it will take to make the measurement will include the settling time, 450 microseconds to flush old data from theADC

**Note:** Analog to digital conversion. The process that translates analog voltage levels to digital values.

, fN1, and whether or not the instruction removes voltage offset errors.

When the datalogger is making burst measurements (designated by negating theChannel (Diff or SE) parameter), the sinc, flush, fN1, and settling time are performed on the first measurement, and fN1is used as the burst frequency for subsequent measurements.

If the total scan interval minus the scan measurement time is less than 50 microseconds, analog power is left on and theADC

**Note:** Analog to digital conversion. The process that translates analog voltage levels to digital values.

requires only about 1 microsecond to "wake up".

**NOTE:** This instruction must NOT be placed inside a conditional statement when running in pipeline mode.

## Related Sensors

This instruction is used by the following sensors:

- Varioius thermistors, RTDs, and PRTs.

**NOTE:** This is not an all-inclusive list of sensors that require this instruction. Refer to the owners manual of your device for more specific instructions.
