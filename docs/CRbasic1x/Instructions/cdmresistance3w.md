# CDM_Resistance3W (CDM 3-Wire Resistance Measurement)

The CDM_Resistance3W instruction is used to make a 3-wire resistance measurement on aCDM **Note:** . For additional information on controller area network peripherals and Campbell modules, see [CPI and CDM](https://www.campbellsci.com/news-cpi-cdm).

## Syntax

```
CDM_Resistance3W
```

(CDMType,CPIAddress,Dest,Reps,Range,SEChan,IexChan,MeasPEx,EXuA,RevEx,SettlingTime,fN1,Mult, Offset,MeasCurrent[optional] )

```
Dim Tblk1'Block #1 dimensioned source
Dim Tref'Declare reference temperature variable
Public Flag'Flag dimension
Public R

DataTable(TEST,Flag,1000)
DataInterval(0,1,Sec,100)
Sample(1,Tblk1,FP2,0)
EndTable

BeginProg
Scan(1,Sec,1,0)
CDM_PanelTemp(CDM_A108,1,Tref,1,1,60)
CDM_TCDiff(CDM_A108,1,Tblk1,1,mV5000,1,TypeT,Tref,0,0,60,1.8,32)
'Next instruction measure Resistance on Diff Chnl 1
CDM_Resistance3W(CDM_A108,1,R,1,mV5000,1,X1,1,2500,True,0,60,1.0,0)
CallTable TEST
NextScan
EndProg
```

## Remarks

This instruction makes three measurements. First, to compute the circuit current, the specified excitation current is applied and the voltage across the internal precision resistor is measured differentially (Vi). The inputs are then reversed and a second measurement is made. This removes any voltage offset errors due to the datalogger measurement circuitry, including operational input voltage errors. Next, the specified channel is measured as a single ended measurement (V1). Finally, the next higher channel is measured as a single ended measurement (V2).

The instruction returns Rs = (2\*V2-V1) Ri / Vi

where Ri is the precision internal resistor value that is saved as part of the factory calibration procedure and Rs is the sense resistance.

If reverse excitation (RevEx) is specified, it is performed on all three measurements.

The following image shows the measurements. Rw is the resistance of the lead wires. These are assumed to be matched.

Rs = (2\* V2-V1) Ri / Vi

## Parameters

# CDMType (CDM Type)

Used to specify the type ofCDM **Note:** used by the instruction. This is a read-only setting in the device which cannot be changed. Right-click the parameter to display a list of valid CDM types.

Type: Constant

# CPIAddress (CPI Address)

TheCPI

** Note:**CPI is a proprietary interface for communications between Campbell Scientific dataloggers and Campbell Scientific CDM peripheral devices. It consists of a physical layer definition and a data protocol. CDM devices are similar to Campbell Scientific SDM devices in concept, but the use of the CPI bus enables higher data-throughput rates and use of longer cables. CDM devices require more power to operate in general than do SDM devices.

address configured in theCDM ** Note:** module. Valid range is 1 through 120. Note that the CPIAddress cannot be a variable because the datalogger must know all information for a given CDM module at program compile.(e.g., before the program runs).

Type: Constant integer

# Dest (Destination)

The Variable in which to store the results of the instruction. Right-click the parameter to display a list of defined variables.

If this instruction has a Repetitions parameter and it is greater than 1, the results are stored in an array with the variable name. The array must be dimensioned large enough to hold all of the values returned from all of the Reps.

Type: Variable or Array

# Reps (Repetitions)

The number of repetitions for the measurement or instruction.

Type: Constant integer (or expression that evaluates as a constant).

For the CDM_Resistance measurement, measurements are made on consecutive channels. If the Reps parameter is greater than 1, the Dest parameter must be a variable array.

# Range

The expected voltage range of the input from the sensor. An alphanumeric code is entered:

| Code   | Description |
| ------ | ----------- |
| mV5000 | 5000 mV     |
| mV1000 | 1000 mV     |
| mV200  | 200 mV      |

Type: Constant

# SEChan (Single-Ended Channel)

The single-ended channel to use with this instruction. The number of available channels depends upon the CDM being programmed. If the Reps parameter is greater than 1, the additional measurements will be made on sequential channels. If the SEChan number is entered as a negative value, all Reps will be performed on the same channel (burst measurement). The burst mode sample frequency is controlled by the fN1parameter, which can go up to a maximum of 93750 Hz (minimum sample interval of 10.667 S). The SettlingTime parameter is used to delay once prior to beginning the burst. The total time prior to beginning the burst is the SettlingTime plus the ADC flush time (which is 810 S). The sample interval resolution is 1/93750Hz. The specified notch frequency will use the nearest multiple of (1/93750Hz) to get as close to the specified frequency as possible.

Type: Constant

# IexChan (Excitation Channel)

The excitation channel (X1, X2, etc.) to use for the first measurement. The number of available channels depends upon the CDM being programmed. Right-click the parameter for a list of valid options.

If the Reps parameter is greater than 1, the excitation channel used for subsequent measurements will be incremented with each measurement based on the MeasPEx parameter.

# MeasPEx (Excitation Channels)

The number of sensors to excite with the same excitation terminal before automatically advancing to the next excitation terminal. To excite all sensors with the same excitation terminal, the value of this parameter should be set equal to the value of the Reps parameter.

This parameter in a bridge measurement can be used when the sensors to be measured outnumber the available excitation channels. It allows multiple sensors to be excited with each excitation channel.

For example, assume it is desired to measure eight pressure transducers that have 350 ohm full bridge strain gages to be excited by 5 volts. Each transducer requires 14 mA (5 volts/350 ohms = 14 mA). Since each excitation channel can provide a maximum of 50 mA at 5 volts, a maximum of 3 pressure transducers can be excited per channel (50 mA/14 mA = 3.57). To measure the eight transducers, 8 would be entered for the Reps argument and 3 for the MeasPEx. The first three transducers would all be connected to the first excitation channel, the next 3 to the second excitation channel, and the last 2 to the third excitation channel.

Type: Constant (or expression that evaluates as a constant)

# ExuA (Current Excitation in μA)

The ExuA argument is the current excitation, in μAmps, to apply to the sensor. The allowable range is 2500 μA. Reducing the excitation current may allow additional sensors to be excited with one excitation channel. For example, in the example above for MeasPEx, if we decrease the excitation current to 2 mA , (V = IR), 0.002 \* 350 = 0.7 V per resistor. Then divide the maximum compliance voltage for an excitation channel (5 V) by the volts per sensor: 5 V/0.7 V per resistor = 7.14 resistors. So, by decreasing the excitation current to 2 mA instead of 2.5 mA, you could measure up to seven 350-ohm resistors per excitation channel. Note that because output for this instruction is ratometric, output is scaled to the excitation current.

# RevEx (Reverse Excitation)

A constant is entered for the RevEx argument to determine whether the excitation should be reversed and applied to the sensor after the first measurement is made. False (or 0) = Do not reverse excitation ; True (or 1) = Reverse excitation and make a second measurement (requires more time to complete). Use of RevEx cancels out voltage offsets due to the sensor, wiring, and excitation circuitry, but does not compensate for operational input voltage errors.

Type: Constant

# SettlingTime

The amount of time to delay, in microseconds, after setting up a measurement and before making the measurement on the CDM. Minimum settling time is 100 microseconds; maximum settling time is 100 ms. If 0 is entered, a default of 500 microseconds is used.

Type: Constant

# fN1 (First Notch Frequency)

Determines the lowest frequency that will be eliminated or notched out by the sinc filter. This filter notches out frequencies at integer multiples of fN1by averaging for a time equal to 1/fN1; thus, lower fN1frequencies result in longer measurement times. Any value between 2.5 Hz and 30,000 Hz can be entered, but the value entered will be rounded to the nearest of the frequencies below. The three most common noise filtering options are listed first.

| Option | Description                                                       |
| ------ | ----------------------------------------------------------------- |
| 30000  | Performs a 0.0333 millisecond integration (for fast measurements) |
| 60     | Performs a 16.67 millisecond integration (filters 60 Hz noise)    |
| 50     | Performs a 20 millisecond integration (filters 50 Hz noise)       |
| 15000  | Performs a 0.0667 millisecond integration                         |
| 7500   | Performs a 0.0133 millisecond integration                         |
| 3750   | Performs a 0.2667 millisecond integration                         |
| 2000   | Performs a 0.5 millisecond integration                            |
| 1000   | Performs a 1 millisecond integration                              |
| 500    | Performs a 2 millisecond integration                              |
| 100    | Performs a 10 millisecond integration                             |
| 30     | Performs a 33.33 millisecond integration                          |
| 25     | Performs a 40 millisecond integration                             |
| 15     | Performs a 66.67 millisecond integration                          |
| 10     | Performs a 100 millisecond integration                            |
| 5      | Performs a 200 millisecond integration                            |
| 2.5    | Performs a 400 millisecond integration                            |

Type: Constant

# Mult, Offset (Multiplier and Offset)

Factors by which to scale the raw results of the measurement. Typically used to convert the raw measurement to engineering units or to units other than which is output. For example, the TCDiff instruction measures a thermocouple and outputs temperature in degrees C. A multiplier of 1.8 and an offset of 32 will convert the temperature to degrees F.

For temperature measurements, a multiplier (mult) of 1 and an offset of 0, would output in degrees Celsius. For analog measurements, a multiplier (mult) of 1 and an offset of 0, would output the measured voltage in millivolts divided by the excitation voltage in volts.

If Repetitions of greater than 1 are used for this instruction, Repetitions can also be used for the Multiplier and Offset.[SeeMultipliers, Offsets, and Disable Variables with Repetitionsfor more information.](../Info/multipliersoffsets.md)

Type: Constant, Variable, Array, or Expression

## Optional Parameter

# MeasCurrent

An optional parameteradded with OS3which, when set to 1, instructs the datalogger to return the current measurement as the last measurement in the array. Dest must be dimensioned appropriately to store the additional measurement.
