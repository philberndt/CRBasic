# VoltSE (Single-Ended Voltage Measurement)

The VoltSE instruction is used to make a single-ended voltage measurement on one or more analog channels. The output is in millivolts.

## Syntax

```
VoltSE
```

(Dest,Reps,Range,SEChan,MeasOff,SettlingTime,fN1,Mult, Offset )

The following program measures a CS105 barometric pressure sensor using a single-ended voltage measurement. The results are stored in inches of mercury, and the elevation correction is at 4500 ft.

```
'Declare Variables and Units
Public Batt_Volt
Public Bar_inHg

Units Batt_Volt=Volts
Units Bar_inHg=Inches of Mercury

'Define Data Tables
DataTable(Table1,True,-1)
DataInterval(0,1440,Min,0)
Minimum(1,Batt_Volt,FP2,False,False)
Sample(1,Bar_inHg,FP2)
EndTable

'Main Program
BeginProg
Scan(5,Sec,1,0)
'Default Datalogger Battery Voltage measurement Batt_Volt:
Battery(Batt_Volt)
'CS105 Barometric Pressure Sensor measurement Bar_inHg:
If IfTime(59,60,Min) Then PortSet(C1,1)'warmup time
If IfTime(0,60,Min) Then'after 1 min, measure:
VoltSE(Bar_inHg,1,mv2500,1,1,0,4000,0.184,754.286)
Bar_inHg=Bar_inHg*0.02953
PortSet(C1,0)
EndIf
'Call Data Tables and Store Data
CallTable(Table1)
NextScan
EndProg
```

## Remarks

This instruction measures the voltage of a single ended analog channel with respect to ground.

## Parameters

# Dest (Destination)

The Variable in which to store the results of the instruction. Right-click the parameter to display a list of defined variables.

If this instruction has a Repetitions parameter and it is greater than 1, the results are stored in an array with the variable name. The array must be dimensioned large enough to hold all of the values returned from all of the Reps.

Type: Variable or Array

# Reps (Repetitions)

The number of repetitions for the measurement or instruction.

Type: Constant integer (or expression that evaluates as a constant).

For the VoltSE instruction, measurements are made on consecutive channels. If the Reps parameter is greater than 1, the Dest parameter must be a variable array.

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

For the VoltSE instruction, if the SEChan number is entered as a negative value, all Reps are performed on the same channel (burst measurement). TheCR350burst measurement samplingrateis determined by the fN1(first notch frequency) parameter.

| fN1 (Hz) | Burst Sampling Rate                   |
| -------- | ------------------------------------- |
| 50 or 60 | 50 mSec (or 20 samples per second)    |
| 400      | 6.25 mSec (or 160 samples per second) |
| 4000     | 0.5 mSec (or 2000 samples per second) |

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

- HMP60

- HMP50

**NOTE:** This is not an all-inclusive list of sensors that require this instruction. Refer to the owners manual of your device for more specific instructions.
