# TCSE (Thermocouple Measurement on Single-Ended Channel)

The TCSE instruction is used to make a thermocouple measurement on a single-ended channel and convert the measurement to degrees Celsius.

## Syntax

TCSE(Dest,Reps,Range,SEChan,TCType,TRef,MeasOff,SettlingTime,fN1,Mult, Offset)

The following sample code shows the use of the TCSE instruction to measure a single-ended thermocouple.

```
Public PTemp, TCTemp

DataTable (Test,True,-1)
DataInterval (0,1,Min,10)
Sample (1,TCTemp,FP2)
EndTable

BeginProg
Scan (5,Sec,3,0)
PanelTemp (Ptemp,50)
TCSE(TCTemp,1,mv200C,1,TypeT,PTemp,False,0,15000,1.0,0)
Calltable (Test)
NextScan
EndProg
```

## Remarks

The calculations used for the TCSE instruction are based on the thermocouple type selected (TCType). The instruction adds the measured voltage to the voltage calculated for the reference temperature relative to 0 degrees Celsius, and converts the combined voltage to temperature in degrees Celsius.

## Parameters

# Dest (Destination)

The Variable in which to store the results of the instruction. Right-click the parameter to display a list of defined variables.

If this instruction has a Repetitions parameter and it is greater than 1, the results are stored in an array with the variable name. The array must be dimensioned large enough to hold all of the values returned from all of the Reps.

Type: Variable or Array

# Reps (Repetitions)

The number of repetitions for the measurement or instruction.

Type: Constant integer (or expression that evaluates as a constant).

For the TCSE instruction, measurements are made on consecutive channels. If the Reps parameter is greater than 1, the Dest parameter must be a variable array.

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

CR1000Xburst measurement sampling rate is determined by the fN1(first notch frequency) parameter (see following information on fN1).

# TCType (Thermocouple Type)

The TCType argument is used to identify the type of thermocouple being measured. An alphanumeric code is entered. Right-click on the parameter to display a list of valid options.

| Alphanumeric | Type               | (+Lead/-Lead)       |
| ------------ | ------------------ | ------------------- |
| TypeT        | Copper Contstantan | Cu/Cu-Ni            |
| TypeE        | Chromel Constantan | Ni-Cr/Cu-Ni         |
| TypeK        | Chromel Alumel     | Ni-Cr/Ni-Al         |
| TypeJ        | Iron Constantan    | Fe/Cu-Ni            |
| TypeB        | Platinum Rhodium   | Pt-30% Rh/ Pt-6% Rh |
| TypeR        | Platinum Rhodium   | Pt-13% Rh/Pt        |
| TypeS        | Platinum Rhodium   | Pt-10% Rh/Pt        |
| TypeN        | Nicrosil-Nisil     | Ni-Cr-Si/Ni-Si-Mg   |

**NOTE:** When using TEMP408 modules, the TCType selected must match the module type.

Type: Constant

# TRef (Temperature Reference)

The name of the variable that is the source of the reference temperature (or the result of the reference temperature measurement), in degrees C, for the thermocouple measurements. Right-click the parameter to display a list of defined variables.

Type: Variable, Array, or Expression

Note that for accurate thermocouple measurements, an external reference temperature is recommended, rather than a PanelTemp measurement. See [PanelTemp](paneltemp.md) for additional information.

# MeasOff (Measure Offset Voltage)

Determines whether the ground offset voltage is measured before the measurement is made on the analog channel. If the ground offset is measured it is subtracted from the sensor measurement and the result is stored in Dest.

| Value | Result                                                                                                                                               |
| ----- | ---------------------------------------------------------------------------------------------------------------------------------------------------- |
| 0     | Offset voltage is corrected from background calibration.                                                                                             |
| 1     | Offset voltage is measured each scan (this option effectively increases the measurement time as if an additional rep were added to the instruction). |

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
