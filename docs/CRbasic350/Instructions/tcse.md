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
TCSE(TCTemp,1,mv34,1,TypeT,PTemp,False,0,4000,1.0,0)
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

| Code   | Description     |
| ------ | --------------- |
| mV2500 | -100 to 2500 mV |
| mV34   | -34 to +34 mV   |

Type: Constant

# SEChan

The single-ended channel number on which to make the first measurement. When Reps are used, subsequent measurements will be automatically made on the following channels.

Type: Constant

CR350burst measurement sampling rate is determined by the fN1(first notch frequency) parameter (see following information on fN1).

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
