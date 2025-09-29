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

AM25T(Temp_C(),10,mV34,1,1,TypeT,RTemp_C,C1,C2,VX1,True,0,60,1,0)

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
| mV2500 | -100 to 2500 mV |
| mV34 | -34 to +34 mV |

Type: Constant


# AM25TChan


Specifies the starting input channel for the multiplexer. If the Repetitions parameter is greater than 1, the additional measurements will be made on sequential channels. If the channel is entered as a negative number, all repetitions occur on the same channel.

Type: Constant


# DiffChan (Differential Terminal)


The number for the differential terminal to which the AM25T is connected.

Type: Constant


# TCType (Thermocouple Type)


The TCType argument is used to identify the type of thermocouple being measured. An alphanumeric or numeric code can be entered. Entering a -1 records a voltage, in millivolts, instead of a thermocouple temperature. Right-click on the parameter to display a list.

| Alphanumeric | Numeric | Type | (+Lead/-Lead) |
| --- | --- | --- | --- |
| mV | -1 | Outputs voltage in mV |
| TypeT | 0 | Copper Contstantan | Cu/Cu-Ni |
| TypeE | 1 | Chromel Constantan | Ni-Cr/Cu-Ni |
| TypeK | 2 | Chromel Alumel | Ni-Cr/Ni-Al |
| TypeJ | 3 | Iron Constantan | Fe/Cu-Ni |
| TypeB | 4 | Platinum Rhodium | Pt-30% Rh/ Pt-6% Rh |
| TypeR | 5 | Platinum Rhodium | Pt-13% Rh/Pt |
| TypeS | 6 | Platinum Rhodium | Pt-10% Rh/Pt |
| TypeN | 7 | Nicrosil-Nisil | Ni-Cr-Si/Ni-Si-Mg |

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
| VX1 | Excitation channel 1 |
| VX2 | Excitation channel 2 |

Type: Constant


# Reset Port (ResPort)


The control port that will be used to enable and reset the multiplexer. Each multiplexer must have its own unique Reset line. An alphanumeric code is entered for this argument. Right-click to display a list.

| Code | Description |
| --- | --- |
| C1 | Control Port 1 |
| C2 | Control Port 2 |
| VX1 | Excitation channel 1 |
| VX2 | Excitation channel 2 |

Type: Constant


# Excitation Channel (ExChan)


The excitation channel that will be used to provide switched excitation for the PRT reference temperature measurement. An alphanumeric code is entered. Right-click the parameter to display a list.

| Code | Description |
| --- | --- |
| 0 | Temperature measurement not made |
| VX1 | Excitation channel 1 |
| VX2 | Excitation channel 2 |

Type: Constant


# Reverse Differential (RevDiff)


A constant value is entered to determine whether the inputs are reversed and a second measurement made. This removes any voltage offset errors due to the datalogger measurement circuitry, including inputrangeerrors. Enabling this parameter doubles the measurement time. Right-click to display a list.

| Logic | Description |
| --- | --- |
| False or 0 | Signal is measured with the high side referenced to the low. Do not make second measurement. |
| True or 1 | Reverse input and make second measurement. |

Type: Constant or Variable

False (or 0) = Do not make second measurement; True (or 1) = Reverse inputs and make second measurement.


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
