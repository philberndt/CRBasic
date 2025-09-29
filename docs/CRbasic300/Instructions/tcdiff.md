# TCDiff (Thermocouple Measurement on Differential Channel)

The TCDiff instruction is used to make a thermocouple measurement on a differential channel and convert the measurement to degrees Celsius.

## Syntax

```
TCDiff
```

(Dest,Reps,Range,DiffChan,TCType,TRef,RevDiff,SettlingTime,fN1,Mult, Offset)

The following sample code shows the use of the TCDiff instruction to measure two differential thermocouples.

```
Public PTemp, TCTemp(2)
```

DataTable (TCTemp,True,-1)
DataInterval (0,1,Min,10)
Average (2,TCTemp(),FP2,False)
Maximum (2,TCTemp(),FP2,False,True)
Minimum (2,TCTemp(),FP2,False,True)
EndTable

BeginProg
Scan (1,Sec,3,0)
PanelTemp (PTemp,4000)
TCDiff(TCTemp(),2,mv34,1,TypeT,PTemp,True,0,4000,1.0,0)
CallTable (TCTemp)
NextScan
EndProg

```

## Remarks


The calculations used for the TCDiff instruction are based on the thermocouple type selected (TCType). The instruction adds the measured voltage to the voltage calculated for the reference temperature relative to 0 degrees Celsius, and converts the combined voltage to temperature in degrees Celsius.Note that for accurate thermocouple measurements, an external reference temperature is recommended, rather than a PanelTemp measurement. See the [PanelTemp](paneltemp.md) help for details.


## Parameters



# Dest (Destination)


The Variable in which to store the results of the instruction. Right-click the parameter to display a list of defined variables.

If this instruction has a Repetitions parameter and it is greater than 1, the results are stored in an array with the variable name. The array must be dimensioned large enough to hold all of the values returned from all of the Reps.

Type: Variable or Array


# Reps (Repetitions)


The number of repetitions for the measurement or instruction.

Type: Constant integer (or expression that evaluates as a constant).

For the TCDiff instruction, measurements are made on consecutive channels. If the Reps parameter is greater than 1, the Dest parameter must be a variable array.


# Range


The voltage range for the measurement or input sensor. An alphanumeric code is entered. Right-click the parameter to display a list or enter the code directly.

| Code | Description |
| --- | --- |
| mV2500 | -100 to 2500 mV |
| mV34 | -34 to +34 mV |

Type: Constant


# DiffChan (Differential Channel)


Specifies thedifferential channelon which to make the first measurement. If the Reps parameter is greater than 1, additional measurements will be made on sequential channels.

If the DiffChan number is entered as a negative value, all Reps are performed on the same channel (burst measurement). TheCR300burst measurement samplingrateis determined by the fN1(first notch frequency) parameter.

| fN1 (Hz) | Burst Sampling Rate |
| --- | --- |
| 50 or 60 | 50 mSec (or 20 samples per second) |
| 400 | 6.25 mSec (or 160 samples per second) |
| 4000 | 0.5 mSec (or 2000 samples per second) |

Type: Constant

TheCR300burst measurement sampling rate is determined by the fN1(first notch frequency) parameter (described in following section on fN1).


# TCType (Thermocouple Type)


The TCType argument is used to identify the type of thermocouple being measured. An alphanumeric code is entered. Right-click on the parameter to display a list of valid options.

| Alphanumeric | Type | (+Lead/-Lead) |
| --- | --- | --- |
| TypeT | Copper Contstantan | Cu/Cu-Ni |
| TypeE | Chromel Constantan | Ni-Cr/Cu-Ni |
| TypeK | Chromel Alumel | Ni-Cr/Ni-Al |
| TypeJ | Iron Constantan | Fe/Cu-Ni |
| TypeB | Platinum Rhodium | Pt-30% Rh/ Pt-6% Rh |
| TypeR | Platinum Rhodium | Pt-13% Rh/Pt |
| TypeS | Platinum Rhodium | Pt-10% Rh/Pt |
| TypeN | Nicrosil-Nisil | Ni-Cr-Si/Ni-Si-Mg |

**NOTE:** When using TEMP408 modules, the TCType selected must match the module type.

Type: Constant


# TRef (Temperature Reference)


The name of the variable that is the source of the reference temperature (or the result of the reference temperature measurement), in degrees C, for the thermocouple measurements. Right-click the parameter to display a list of defined variables.

Type: Variable, Array, or Expression

Note that for accurate thermocouple measurements, an external reference temperature is recommended, rather than a [PanelTemp](paneltemp.md) measurement.


# Reverse Differential (RevDiff)


A constant value is entered to determine whether the inputs are reversed and a second measurement made. This removes any voltage offset errors due to the datalogger measurement circuitry, including inputrangeerrors. Enabling this parameter doubles the measurement time. Right-click to display a list.

| Logic | Description |
| --- | --- |
| False or 0 | Signal is measured with the high side referenced to the low. Do not make second measurement. |
| True or 1 | Reverse input and make second measurement. |

Type: Constant or Variable


# Settling Time


The settling time is the duration (in microseconds) to allow for signal settling after setting up a measurement (switching to the channel, setting the excitation) and before making the measurement.The default settling time is 500 microseconds, with a minimum of 10 microseconds and a maximum of 50000 microseconds.

Type: Constant (or expression that evaluates as a constant)


# fN1


Determines the lowest frequency that will be eliminated or notched out by the sinc filter. This filter notches out frequencies at integer multiples of fN1by averaging for a time equal to 1/fN1; thus, lower fN1frequencies result in longer measurement times.

| Option | Description |
| --- | --- |
| 50 | Standard measurement speed; requires 50 msec to filter 50 Hz noise |
| 60 | Standard measurement speed; requires 50 msec to filter 60 Hz noise |
| 400 | Medium measurement speed; performs a 6.25 msec integration to filter 400 Hz noise |
| 4000 | Fast measurement speed; performs a 0.5 msec integration to filter 4000 Hz noise |

Type: Constant


# Mult, Offset (Multiplier and Offset)


Factors by which to scale the raw results of the measurement. Typically used to convert the raw measurement to engineering units or to units other than which is output. For example, the TCDiff instruction measures a thermocouple and outputs temperature in degrees C. A multiplier of 1.8 and an offset of 32 will convert the temperature to degrees F.

For temperature measurements, a multiplier (mult) of 1 and an offset of 0, would output in degrees Celsius. For analog measurements, a multiplier (mult) of 1 and an offset of 0, would output the measured voltage in millivolts divided by the excitation voltage in volts.

If Repetitions of greater than 1 are used for this instruction, Repetitions can also be used for the Multiplier and Offset.[SeeMultipliers, Offsets, and Disable Variables with Repetitionsfor more information.](../Info/multipliersoffsets.md)

Type: Constant, Variable, Array, or Expression
```
