# BrHalf (Half Bridge Measurement)

The BrHalf instruction is used to make a half bridge measurement.

## Syntax

```
BrHalf
```

(Dest,Reps,Range,SEChan,ExChan,MeasPEx,ExmV,RevEx,SettlingTime,fN1,Mult,Offset)

This program provides an example use of the BRHalf instruction. It is used to measure an RM Young Wind Monitor.

```
'Example program to measure the 05103 RM Young Wind Monitor using the
'BrHalf measurement instruction.

'Declare Variables and Units
Public WS_ms
Public WindDir
Units WS_ms=meters/second
Units WindDir=Degrees

'Define Data Tables
DataTable(Table1,True,-1)
DataInterval(0,60,Min,0)
WindVector (1,WS_ms,WindDir,IEEE4,0,0,0,0)
FieldNames("WS_ms_S_WVT,WindDir_D1_WVT,WindDir_SD1_WVT")
EndTable

'Main Program
BeginProg
Scan(5,Sec,1,0)
'05103 Wind Speed & Direction Sensor measurements WS_ms and WindDir:
PulseCount(WS_ms,1,P_LL,1,1,0.098,0)
BrHalf(WindDir,1,mv2500,1,Vx1,1,2500,False,0,4000,355,0)
'Call Data Tables and Store Data
CallTable(Table1)
NextScan
EndProg
```

## Remarks

This instruction applies an excitation voltage, delays a specified amount of time, and then makes a single ended voltage measurement. With a multiplier of 1 and an offset of 0 the result is the ratio of the measured voltage divided by the excitation voltage.The excitation voltage used by this instruction is turned off when another measurement instruction is run or the program reaches the end of the scan.

Relational Formula:

## Parameters

# Dest (Destination)

The Variable in which to store the results of the instruction. Right-click the parameter to display a list of defined variables.

If this instruction has a Repetitions parameter and it is greater than 1, the results are stored in an array with the variable name. The array must be dimensioned large enough to hold all of the values returned from all of the Reps.

Type: Variable or Array

For the BrHalf instruction, the Dest parameter is a variable in which to store the results (X in the equation) of the measurement.

# Reps (Repetitions)

The number of repetitions for the measurement or instruction.

Type: Constant integer (or expression that evaluates as a constant).

For the BrHalf instruction, the Reps parameter is the number of times the measurement should be made. Measurements are made on consecutive channels. If the Reps parameter is greater than 1, the Dest parameter must be a variable array.

# Range

The voltage range for the measurement. An alphanumeric code is entered. Right-click the parameter to display a list or enter the code directly.

| Code   | Description     |
| ------ | --------------- |
| mV2500 | -100 to 2500 mV |
| mV34   | -34 to +34 mV   |

Type: Constant

# SEChan

The single-ended channel number on which to make the first measurement. When Reps are used, subsequent measurements will be automatically made on the following channels.

Type: Constant

For the BRHalf instruction, if the SEChan number is entered as a negative value, all Reps are performed on the same channel (burst measurement). The burst measurement sampling rate is determined by the fN1(first notch frequency) parameter.

| fN1(Hz)  | Burst Sampling Rate                   |
| -------- | ------------------------------------- |
| 50 or 60 | 50 mSec (or 20 samples per second)    |
| 400      | 6.25 mSec (or 160 samples per second) |
| 4000     | 0.5 mSec (or 2000 samples per second) |

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

For the BrHalf instruction, the ExmV argument is the excitation, in millivolts, to apply to the sensor.

# RevEx (Reverse Excitation)

This datalogger is not capable of reverse excitation, so this parameter is not used. Set to False.

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

- 110PU-L Thermistor

- 3002 RMYoung Wind Sentry Set

- 3101 RMYoung Wind Sentry Anemometer

- 3301 RM Young Wind Sentry Vane

- Wind Monitor: 05103, 05103-45, 05106, 05108, 05108-45, 05305

- ICEFREE3A

- ICEFREE3V

- 43347 RTD Temperature Probe

- 43502 Radiation Shield

- 4103-5 Radiation Shield

- 253 Soil Matric Potential Sensors

- 257 Soil Matric Potential Sensors

- Various other thermistors, RTDs, and string potentiometers.

**NOTE:** This is not an all-inclusive list of sensors that require this instruction. Refer to the owners manual of your device for more specific instructions.
