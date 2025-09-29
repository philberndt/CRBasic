# BrFull (Full Bridge Measurement)

The BrFull instruction is used to make a full bridge measurement.

## Syntax

```
BrFull
```

(Dest,Reps,Range,DiffChan,ExChan,MeasPEx,ExmV,RevEx,RevDiff,SettlingTime,fN1,Mult, Offset)

The following program provides an example of measuring a Druck CS420 pressure transducer using a BrFull measurement.

```
'CR1000X Series
'Example program to measure the CS420 Druck pressure transducer using the
' BrFull measurement instruction.

'Declare Variables and Units
Public Lvl_ft
Units Lvl_ft=feet

'Define Data Tables
DataTable(Table1,True,-1)
DataInterval(0,60,Min,0)
Average(1,Lvl_ft,IEEE4,0)
EndTable

'Main Program
BeginProg
Scan(5,Sec,1,0)
'CS420/CS425 Druck PDCR 1830/1230 Pressure Transducer (4-wire)
'measurementLvl_ft:
BrFull(Lvl_ft,1,mv5000C,1,Vx1,1,2500,True,True,0,15000,2.3067,0)
'Call Data Tables and Store Data
CallTable(Table1)
NextScan
EndProg
```

## Remarks

This instruction applies an excitation voltage to a full bridge and makes a differential voltage measurement of the bridge output. The resulting value is the measured voltage in millivolts divided by the excitation voltage in volts.

**NOTE:** The 1000 indicates that the mV excitation has been converted to Volts, with final result in units of mV per V.

Relational Formula:

**NOTE:** Terminal assignments for this instruction must fall within the guidelines for universal terminal pairs.

## Parameters

# Dest (Destination)

The Variable in which to store the results of the instruction. Right-click the parameter to display a list of defined variables.

If this instruction has a Repetitions parameter and it is greater than 1, the results are stored in an array with the variable name. The array must be dimensioned large enough to hold all of the values returned from all of the Reps.

Type: Variable or Array

For the BrFull instruction, the Dest parameter is a variable in which to store the results (X in the equation) of the measurement.

# Reps (Repetitions)

The number of repetitions for the measurement or instruction.

Type: Constant integer (or expression that evaluates as a constant).

For the BrFull instruction, measurements are made on consecutive channels. If the Reps parameter is greater than 1, the Dest parameter must be a variable array.

# Range

The voltage range for the measurement. An alphanumeric code is entered. Right-click the parameter to display a list or enter the code directly.

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

For signals that do not fluctuate too rapidly, AutoRange allows the datalogger to automatically choose the input voltage range. AutoRange results in two voltage measurements being performed. The first voltage measurement is done quickly at a first notch frequency (fN1) of 50 kHz, the result of which determines the input voltage range to use for the second measurement. The second measurement is then performed using the range determined from the first measurement, along with the fN1chosen for the measurement instruction. Both measurements use the settling time chosen for the particular measurement instruction. Auto-ranging optimizes resolution but takes longer than a fixed range measurement because of the two-measurement sequence. The exception to this two-measurement sequence is if Reps are made on the same channel (Reps parameter is negative). In this instance, the test measurement is made only once. Subsequent repetitions are made with the delay between each measurement being the specified settling time. Autorange should not be used if fast measurements are required or if the analog signal could change significantly over the course of the measurement.

The C options check for an open connection on the analog input by applying a short overranging test signal to the analog input prior to making the measurement. For a voltage range of mv5000C,5.6Vis applied. For a voltage range of mv1000C or mv200C, 1.25V is applied. An open (broken) sensor will result in a measurement overrange, clearly indicating a sensor problem instead of returning a bad measured value caused by a floating input.

The open-detect option may not work properly in the presence of external leakage (< 1 MOhm) to ground, as the overrange test signal could discharge through the external leakage during the input settling time such that an overrange condition no longer exists. In this case the measurement settling time should be minimized to minimize the discharge time of the overrange test signal.

The open circuit detection (C option) can cause measurement errors for slow responding sensors, as such sensors may not have sufficient time to recover from the applied short duration (50 microseconds) test signal. For such sensors, increasing the measurement settling time beyond the default may be necessary for sufficient sensor recovery time. If measurement speed is critical with a slow responding sensor, then it may be preferable to not use the open detect (C option).

Type: Constant

# DiffChan (Differential Channel)

Specifies thedifferential channelon which to make the first measurement. If the Reps parameter is greater than 1, additional measurements will be made on sequential channels.

If the DiffChan number is entered as a negative value, all Reps are performed on the same channel (burst measurement). TheCR1000Xburst measurement samplingfrequencyis determined by the fN1(first notch frequency) parameterwhich can go up to a maximum of 31250 Hz (minimum sample interval of 32 uS). The SettlingTime parameter is used to delay once prior to beginning the burst. The total time prior to beginning the burst is the SettlingTime plus the ADC flush time (which is 450 uS). The sample interval resolution is 1/31250 Hz. The specified notch frequency will use the nearest multiple of (1/31250 Hz) to get as close to the specified frequency as possible.

| Code | Description                           |
| ---- | ------------------------------------- |
| 1    | Differential Channel 1 (SE 1 and 2)   |
| 2    | Differential Channel 2 (SE 3 and 4)   |
| 3    | Differential Channel 3 (SE 5 and 6)   |
| 4    | Differential Channel 4 (SE 7 and 8)   |
| 5    | Differential Channel 5 (SE 9 and 10)  |
| 6    | Differential Channel 6 (SE 11 and 12) |
| 7    | Differential Channel 7 (SE 13 and 14) |
| 8    | Differential Channel 8 (SE 15 and 16) |

Type: Constant

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

# Reverse Differential (RevDiff)

A constant value is entered to determine whether the inputs are reversed and a second measurement made. This removes any voltage offset errors due to the datalogger measurement circuitry, includingoperationalinputvoltageerrors. Enabling this parameter doubles the measurement time.Measurement time will increase four times if RevEx is used.Right-click to display a list.

| Logic      | Description                                                                                  |
| ---------- | -------------------------------------------------------------------------------------------- |
| False or 0 | Signal is measured with the high side referenced to the low. Do not make second measurement. |
| True or 1  | Reverse input and make second measurement.                                                   |

Type: Constant or Variable

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

- ST350 Strain Transducer

- CS420 Pressure Transducer

- Various other accelerometers, scales, load cells, pressure transducers, and foil-bonded strain gages.

**NOTE:** This is not an all-inclusive list of sensors that require this instruction. Refer to the owners manual of your device for more specific instructions.
