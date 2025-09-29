# AM25T

The AM25T instruction is used to control and measure the AM25T Multiplexer.

## Syntax

AM25T(Dest,Reps,Range,AM25TChan,ChanAnlg,TCType,TRef,ClkPort,ResPort,ExChan,RevDiff,SettlingTime,fN1,Mult, Offset)

This example demonstrates using the AM25T thermocouple multiplexer with the datalogger.

```
'Declare Variables and Units
Public Batt_Volt
Public RTemp_C
Public Temp_C(10)

Units Batt_Volt=Volts
Units RTemp_C=Deg C
Units Temp_C=Deg C

'Define Data Tables
DataTable(Table1,True,-1)
DataInterval(0,60,Min,0)
Sample(10,Temp_C(),FP2)
EndTable

'Main Program
BeginProg
Scan(30,Sec,1,0)
'Default Datalogger Battery Voltage measurement Batt_Volt:
Battery(Batt_Volt)
'AM25T Multiplexer
```

AM25T(Temp_C(),10,mV200C,1,1,TypeT,RTemp_C,C1,C2,VX1,True,0,60,1,0)

'Call Data Tables and Store Data
CallTable(Table1)
NextScan
EndProg

```

## Remarks


This instruction sets up the AM25T Multiplexer to be used with the datalogger, triggers the measurements, and stores the returned measurements. This instruction also returns a measurement from the multiplexer's built-in PRT (used as a reference temperature for the thermocouple measurements). If 0 is entered for the Reps parameter, this instruction makes only the PRT measurement and stores the result in the Dest parameter.

This instruction can be run within a [SlowSequence](slowsequence.md). However, if RevDiff is true and it is necessary to slice in the SlowSequence measurements in order to run the main scan, the two measurements may be split between scans.


## Parameters



# Dest (Destination)


The Variable in which to store the results of the instruction. Right-click the parameter to display a list of defined variables.

If this instruction has a Repetitions parameter and it is greater than 1, the results are stored in an array with the variable name. The array must be dimensioned large enough to hold all of the values returned from all of the Reps.

Type: Variable or Array


# Reps (Repetitions)


The number of repetitions for the measurement or instruction.

Type: Constant integer (or expression that evaluates as a constant).

Measurements are made on consecutive channels. If the Repetitions parameter is greater than 1, the Destination parameter must be a variable array. If 0 is entered, the only measurement that is made is the reference PRT temperature measurement. In the case of Reps = 0, the reference PRT measurement is stored in the Dest variable.


# Range


The voltage range for the measurement or input sensor. An alphanumeric code is entered. Right-click the parameter to display a list or enter the code directly.

| Code | Description |
| --- | --- |
| mV5000 | +5000 mV |
| mV1000 | +1000 mV |
| mv200 | +200 mV |
| Autorange | Datalogger tests for and uses most suitable range. |
| AutorangeC | Autorange, checks for open input. |
| mV5000C | +5000 mV, checks for open input, sets excitation to ~5600mV. Overrange values may go undetected; use code to detect overrange values. |
| mV1000C | +1000 mV, checks for open input, sets excitation to ~1250 mV. |
| mV200C | +200 mV, checks for open input, sets excitation to ~1250 mV. |

Autorange increases the time to make the measurement. For signals that do not fluctuate too rapidly, AutoRange allows the datalogger to automatically choose the input voltage range. AutoRange results in two voltage measurements being performed. The first voltage measurement is done quickly at a first notch frequency (fN1) of 50 kHz, the result of which determines the input voltage range to use for the second measurement. The second measurement is then performed using the range determined from the first measurement, along with the fN1chosen for the measurement instruction. Both measurements use the settling time chosen for the particular measurement instruction. Auto-ranging optimizes resolution but takes longer than a fixed range measurement because of the two-measurement sequence. The exception to this two-measurement sequence is if Reps are made on the same channel (Reps parameter is negative). In this instance, the test measurement is made only once. Subsequent repetitions are made with the delay between each measurement being the specified settling time. Autorange should not be used if fast measurements are required or if the analog signal could change significantly over the course of the measurement.

The C options check for an open connection on the analog input by applying a short overranging test signal to the analog input prior to making the measurement. For a voltage range of mv5000C,5.6Vis applied. For a voltage range of mv1000C or mv200C, 1.25V is applied. An open (broken) sensor will result in a measurement overrange, clearly indicating a sensor problem instead of returning a bad measured value caused by a floating input.

The open-detect option may not work properly in the presence of external leakage (< 1 MOhm) to ground, as the overrange test signal could discharge through the external leakage during the input settling time such that an overrange condition no longer exists. In this case the measurement settling time should be minimized to minimize the discharge time of the overrange test signal.

The open circuit detection (C option) can cause measurement errors for slow responding sensors, as such sensors may not have sufficient time to recover from the applied short duration (50 microseconds) test signal. For such sensors, increasing the measurement settling time beyond the default may be necessary for sufficient sensor recovery time. If measurement speed is critical with a slow responding sensor, then it may be preferable to not use the open detect (C option).

Type: Constant


# AM25TChan


Specifies the starting input channel for the multiplexer. If the Repetitions parameter is greater than 1, the additional measurements will be made on sequential channels. If the channel is entered as a negative number, all repetitions occur on the same channelat a repetition rate of 1/fN1.

Type: Constant


# DiffChan (Differential Terminal)


The number for the differential terminal to which the AM25T is connected.

Type: Constant


# TCType (Thermocouple Type)


The TCType argument is used to identify the type of thermocouple being measured. An alphanumeric or numeric code can be entered. Entering a -1 records a voltage, in millivolts, instead of a thermocouple temperature. Right-click on the parameter to display a list.

| Alphanumeric | Type | (+Lead/-Lead) |
| --- | --- | --- |
| mV | Outputs voltage in mV |
| TypeT | Copper Contstantan | Cu/Cu-Ni |
| TypeE | Chromel Constantan | Ni-Cr/Cu-Ni |
| TypeK | Chromel Alumel | Ni-Cr/Ni-Al |
| TypeJ | Iron Constantan | Fe/Cu-Ni |
| TypeB | Platinum Rhodium | Pt-30% Rh/ Pt-6% Rh |
| TypeR | Platinum Rhodium | Pt-13% Rh/Pt |
| TypeS | Platinum Rhodium | Pt-10% Rh/Pt |
| TypeN | Nicrosil-Nisil | Ni-Cr-Si/Ni-Si-Mg |

Type: Constant


# TRef (Temperature Reference)


The name of the variable that is the source of the reference temperature (or the result of the reference temperature measurement), in degrees C, for the thermocouple measurements. Right-click the parameter to display a list of defined variables.

Type: Variable, Array, or Expression

If ExChan is set to a non-zero value, the measured value of the AM25T PRT is loaded into TRef and used as the reference. If ExChan is set to 0, the AM25T reference PRT will not be measured and whatever value was previously loaded into TRef is used as the reference temperature.


# ClkPort (Clock Port)


The control port used for the multiplexer clock pulse. One clock port may be used to clock several devices. An alphanumeric code is entered for this argument. Right-click to display a drop-down list.

| Code | Description |
| --- | --- |
| C1 | Control Port 1 |
| C2 | Control Port 2 |
| C3 | Control Port 3 |
| C4 | Control Port 4 |
| C5 | Control Port 5 |
| C6 | Control Port 6 |
| C7 | Control Port 7 |
| C8 | Control Port 8 |

Type: Constant


# Reset Port (ResPort)


The control port that will be used to enable and reset the multiplexer. Each multiplexer must have its own unique Reset line. An alphanumeric code is entered for this argument. Right-click to display a list.

| Code | Description |
| --- | --- |
| C1 | Control Port 1 |
| C2 | Control Port 2 |
| C3 | Control Port 3 |
| C4 | Control Port 4 |
| C5 | Control Port 5 |
| C6 | Control Port 6 |
| C7 | Control Port 7 |
| C8 | Control Port 8 |

Type: Constant


# Excitation Channel (ExChan)


The excitation channel that will be used to provide switched excitation for the PRT reference temperature measurement. An alphanumeric code is entered. Right-click the parameter to display a list.

| Code | Description |
| --- | --- |
| 0 | Temperature measurement not made |
| VX1 | Excitation channel 1 |
| VX2 | Excitation channel 2 |
| VX3 | Excitation channel 3 |
| VX4 | Excitation channel 4 |

Type: Constant


# Reverse Differential (RevDiff)


A constant value is entered to determine whether the inputs are reversed and a second measurement made. This removes any voltage offset errors due to the datalogger measurement circuitry, includingoperationalinputvoltageerrors. Enabling this parameter doubles the measurement time.Measurement time will increase four times if RevEx is used.Right-click to display a list.

| Logic | Description |
| --- | --- |
| False or 0 | Signal is measured with the high side referenced to the low. Do not make second measurement. |
| True or 1 | Reverse input and make second measurement. |

Type: Constant or Variable

False (or 0) = Do not make second measurement; True (or 1) = Reverse inputs and make second measurement.


# Settling Time


The settling time is the duration (in microseconds) to allow for signal settling after setting up a measurement (switching to the channel, setting the excitation) and before making the measurement.Minimum settling time is 20 microseconds; maximum settling time is 600 ms. If 0 is entered, a default of 500 microseconds is used.

Additional settling time may be necessary to allow the signal to settle with high resistance or long lead lengths (higher capacitance). The time it will take to make the measurement will include the measurement itself, the SettlingTime, fN1, and whether or not parameters are set to remove voltage offset errors. Using either RevDiff or RevEx causes two SettlingTimes to occur per channel; four SettlingTimes will occur when using both RevDiff and RevEx.

Type: Constant (or expression that evaluates as a constant)


# fN1


Determines the lowest frequency that will be eliminated or notched out by the sinc filter. This filter notches out frequencies at integer multiples of fN1by averaging for a time equal to 1/fN1; thus, lower fN1frequencies result in longer measurement times.Set fN1to the lowest frequency that should be filtered out. For example, setting fN1to 60 filters all multiples of 60 and above (60, 120, 180, etc.) but will not effectively filter lower frequencies; for example 30 Hz.Any value between 0.5 Hz and 31.25 kHz can be entered, but common options for filtering noise are:

| Option | Description |
| --- | --- |
| 15000 | Performs a 0.0667 millisecond integration (for fast measurements) |
| 60 (or \_60Hz) | Performs a 16.67 millisecond integration (filters 60 Hz noise) |
| 50 (or \_50Hz) | Performs a 20 millisecond integration (filters 50 Hz noise) |

Type: Constant


# Mult, Offset (Multiplier and Offset)


Factors by which to scale the raw results of the measurement. Typically used to convert the raw measurement to engineering units or to units other than which is output. For example, the TCDiff instruction measures a thermocouple and outputs temperature in degrees C. A multiplier of 1.8 and an offset of 32 will convert the temperature to degrees F.

For temperature measurements, a multiplier (mult) of 1 and an offset of 0, would output in degrees Celsius. For analog measurements, a multiplier (mult) of 1 and an offset of 0, would output the measured voltage in millivolts divided by the excitation voltage in volts.

If Repetitions of greater than 1 are used for this instruction, Repetitions can also be used for the Multiplier and Offset.[SeeMultipliers, Offsets, and Disable Variables with Repetitionsfor more information.](../Info/multipliersoffsets.md)

Type: Constant, Variable, Array, or Expression


### Measurement Time


The minimum time it will take to make the measurement will include the settling time, 450 microseconds to flush old data from the A to D converter, fN1, and whether or not the instruction removes voltage offset errors.

If the total scan interval minus the scan measurement time is less than 50 microseconds, analog power is left on and the A to D converter requires only about 1 microsecond to "wake up".

**NOTE:** This instruction must NOT be placed inside a conditional statement when running in pipeline mode.
```
