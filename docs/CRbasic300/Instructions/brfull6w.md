# BrFull6W (6-Wire Full Bridge Measurement)

The BrFull6W instruction is used to make a 6 wire full bridge measurement.

## Syntax

```
BrFull6W
```

(Dest,Reps,Range1,Range2,DiffChan,ExChan,MeasPEx,ExmV,RevEx,RevDiff,SettlingTime,fN1,Mult,Offset,ReturnV1[optional] )

The following program provides an example of measuring a Druck CS420 pressure transducer using a BrFull6W measurement.

```
'Example program to measure the CS420 Druck pressure transducer using the
' BrFull6W measurement instruction.

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
'CS420/CS425 Druck PDCR 1830/1230 Pressure Tansducer (6-wire)
'measurementLvl_ft:
BrFull6W(Lvl_ft,1,mv2500,mv2500,1,Vx1,1,2500,False,True,0,4000,2.3067,0)
'Call Data Tables and Store Data
CallTable(Table1)
NextScan
EndProg
```

## Remarks

This instruction applies an excitation voltage and makes two differential voltage measurements. The measurements are made on sequential channels. The result is 1000 \* the voltage measured on the second channel (V2) divided by the voltage measured on the first (V1) (or millivolts output per volt of excitation). The connections are made so that V1is the measurement of the voltage drop across the full bridge, and V2is the measurement of the bridge output. The excitation voltage used by this instruction is turned off when another measurement instruction is run or the program reaches the end of the scan. **Relational Formula:** ## Parameters

# Dest (Destination)

The Variable in which to store the results of the instruction. Right-click the parameter to display a list of defined variables.

If this instruction has a Repetitions parameter and it is greater than 1, the results are stored in an array with the variable name. The array must be dimensioned large enough to hold all of the values returned from all of the Reps.

Type: Variable or Array

For the BrFull6W instruction, the Dest parameter is a variable in which to store the results (X in the equation) of the measurement.

# Reps (Repetitions)

The number of repetitions for the measurement or instruction.

Type: Constant integer (or expression that evaluates as a constant).

For the BrFull6W instruction, the Reps parameter is the number of times the measurement should be made. Measurements are made on consecutive channels. If the Reps parameter is greater than 1, the Dest parameter must be a variable array.

# Range

The voltage range for the measurement or input sensor. An alphanumeric code is entered. Right-click the parameter to display a list or enter the code directly.

| Code   | Description     |
| ------ | --------------- |
| mV2500 | -100 to 2500 mV |
| mV34   | -34 to +34 mV   |

Type: Constant

Range1 is for the first channel measured; Range2 is for the second channel measured. An alphanumeric code is entered.

# DiffChan (Differential Channel)

Specifies thedifferential channelon which to make the first measurement. If the Reps parameter is greater than 1, additional measurements will be made on sequential channels.

If the DiffChan number is entered as a negative value, all Reps are performed on the same channel (burst measurement). TheCR300burst measurement samplingrateis determined by the fN1(first notch frequency) parameter.

| fN1 (Hz) | Burst Sampling Rate                   |
| -------- | ------------------------------------- |
| 50 or 60 | 50 mSec (or 20 samples per second)    |
| 400      | 6.25 mSec (or 160 samples per second) |
| 4000     | 0.5 mSec (or 2000 samples per second) |

Type: Constant

# ExChan (Excitation Channel)

Specifies theexcitationterminal to use to excite the first measurement. If the Reps parameter is greater than 1, the excitation terminal used for subsequent measurements will be incremented with each measurement based on the MeasPEx parameter.

When a series of sensors are measured with the same instruction, the Meas/Ex parameter is used to determine how many sensors to excite with one excitation terminal before advancing to the next.

An alphanumeric code is entered. Right-click within the parameter to display a list.

| Alphanumeric | Description          |
| ------------ | -------------------- |
| VX1          | Excitation channel 1 |
| VX2          | Excitation channel 2 |

Type: Constant

# MeasPEx (Excitation Channels)

The number of sensors to excite with the same excitation terminal before automatically advancing to the next excitation terminal. To excite all sensors with the same excitation terminal, the value of this parameter should be set equal to the value of the Reps parameter.

This parameter in a bridge measurement can be used when the sensors to be measured outnumber the available excitation channels. It allows multiple sensors to be excited with each excitation channel.

For example, assume it is desired to measure eight pressure transducers that have 350 ohm full bridge strain gages to be excited by 5 volts. Each transducer requires 14 mA (5 volts/350 ohms = 14 mA). Since each excitation channel can provide a maximum of 50 mA at 5 volts, a maximum of 3 pressure transducers can be excited per channel (50 mA/14 mA = 3.57). To measure the eight transducers, 8 would be entered for the Reps argument and 3 for the MeasPEx. The first three transducers would all be connected to the first excitation channel, the next 3 to the second excitation channel, and the last 2 to the third excitation channel.

Type: Constant (or expression that evaluates as a constant)

# ExmV (Excitation Voltage in mV)

The excitation voltage in millivolts. The allowable range is +150 to +2500 mV for all bridge measurements. For the ExciteV() instruction, the range is +150 to +5000 when powered via an external battery (when powered via USB, it is +150 to +2500 mV).

Type: Constant. For ExciteV() only, this parameter can also be a Variable.

# RevEx (Reverse Excitation)

This datalogger is not capable of reverse excitation, so this parameter is not used. Set to False.

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

## Optional Parameter

# ReturnV1

Optional parameter that determines whether or not to return the V1 voltage as shown in the following image. If ReturnV1 is non-zero, V1 will be returned. In this case, the Destination parameter must be a two-element (or greater) array. The first element will hold 1000\* V2/V1 (the normal output for this instruction). The second element will hold V1.

| Value | State            |
| ----- | ---------------- |
| 0     | Do not return V1 |
| ≠0    | Return V1        |

## Related Sensors

This instruction is used by the following sensors:

- Various accelerometers, scales, load cells, pressure transducers, and foil-bonded strain gages.

**NOTE:** This is not an all-inclusive list of sensors that require this instruction. Refer to the owners manual of your device for more specific instructions.
