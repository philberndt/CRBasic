# CDM_Resistance (CDM Resistance Measurement)

The CDM_Resistance instruction is used to measure resistance of a basic or full-bridge circuit on aCDM **Note:** . For additional information on controller area network peripherals and Campbell modules, see [CPI and CDM](https://www.campbellsci.com/news-cpi-cdm).

## Syntax

```
CDM_Resistance
```

(CDMType,CPIAddress,Dest,Reps,Range,DiffChan,IexChan,MeasPEx,EXuA,RevEx,RevDiff,SettlingTime,fN1,Mult, Offset,MeasCurrent[optional] )

The following code shows the use of the CDM_Resistance instruction to measure resistance on a differential channel.

```
Dim Tblk1'Block #1 dimensioned source
Dim Tref'Declare reference temperature variable
Public Flag'Flag dimension
Public R

DataTable( TEST, Flag, 1000 )
DataInterval( 0,1,Sec, 100 )
Average( 1, Tblk1(), FP2, 0 )
EndTable

BeginProg
Scan( 1,Sec, 1, 0 )
CDM_PanelTemp(CDM_A108,1, Tref,1 ,1,60)
CDM_TCDiff (CDM_A108,1, Tblk1, 1,mV5000,1,TypeT,Tref,0,0,60,1.8,32 )
'Next instruction measure Resistance on Diff Chnl 1
CDM_Resistance(CDM_A108,1, R,1,mV5000,1,X1,1,2500,True ,True ,0,60,1.0,0)
CallTable TEST
NextScan
EndProg
```

Example 2:

```
'This program example demonstrates the measurement of a 100-ohm PRT (PT100)
'with current excitation.

Const RS0 = 100000
Public X'Raw output from the bridge
Public RS'Calculated PT100 resistance
Public RS_RS0'Calculted ratio RS/RS0
Public DegC'Calculated tmeperature

BeginProg
Scan(1,Sec,0,0)
'Measure X:
CDM_Resistance(CDM_A108,1,X,1,mV200,1,X1,1,2500,True,True,0,60,1,0)
'Calculate RS and RS_RS0
RS = X
RS_RS0 = RS/RS0
'Calculate temperature from RS_RS0
'PrtCalc(Dest,Reps,Source,PRTType,Mult,Offset)
PRTCalc(DegC,1,RS_RS0,1,1.0,0)
NextScan
EndProg
```

## Remarks

The resistance is determined by applying a known excitation current to a circuit and dividing the resultant voltage by the excitation current. The maximum excitation current is 2500 μA. If multiple sensors are measured during current excitation, the sensors should be connected in series, rather than in parallel. The sum of the voltages that develops across each resistor when the excitation current is applied cannot exceed the specified compliance voltage for each excitation channel (+/- 5V). The MeasPEx and ExuA arguments are used to specify the number sensors to exicite with a given excitation channel. The output of the measurement is a ratio of the measured voltage and the known current excitation. Example calculations follow.

### Resistance, Basic Circuit

### Resistance, Full-Bridge Circuit

Relational Formulas:

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

# DiffChan (Differential Channel)

The number of the differential channel pair on which to make the first measurement. The number of available channels depends upon the CDM being programmed. If the Reps parameter is greater than 1, the additional measurements will be made on sequential channels.

If the DiffChan number is entered as a negative value, all Reps will be performed on the same channel (burst measurement). The burst mode sample frequency is controlled by the fN1 parameter, so there are 16 sample frequencies with a maximum of 30 kHz. The SettlingTime parameter is used to delay once prior to beginning the burst. The total time prior to beginning the burst is the SettlingTime plus 180 microseconds.

Type: Constant

# IexChan (Excitation Channel)

The excitation channel (X1, X2, etc.) to use for the first measurement. The number of available channels depends upon the CDM being programmed. Right-click the parameter for a list of valid options.

If the Reps parameter is greater than 1, the excitation channel used for subsequent measurements will be incremented with each measurement based on the MeasPEx parameter.

# MeasPEx (Excitation Channels)

The MeasPEx argument specifies the number of sensors to excite with the same excitation channel before automatically advancing to the next excitation channel. To excite all sensors with the same excitation channel, the value of MeasPEx should be set equal to the Reps parameter.

If multiple sensors are measured during current excitation, the sensors should be connected in series, rather than in parallel. For example, instead of connecting the low side of the resistor to ground, the low side connects to the high side of the next resistor being measured. The last resistor in the chain will tie to ground. With the sensors connected in series, you need to consider the voltage that develops across each resistor when the excitation current is applied (V = IR). The sum of these voltages cannot exceed the specified compliance voltage for each excitation channel (+/- 5V). For example, if you re exciting a string of 350-ohm resistors with the full 2.5 mA of current excitation, the voltage across each resistor is 0.0025 \* 350 = 0.875 V per resistor. Then, 5 V compliance voltage/ 0.875 V per resistor = 5.714 resistors. So, you could measure up to five 350-ohm resistors per excitation channel. Hence, to measure eight of these sensors, 8 would be entered for the Reps argument and 5 for the MeasPEx argument. The first 5 sensors would all be connected to the first excitation channel, and the next 3 to the second excitation channel.

# ExuA (Current Excitation in μA)

The ExuA argument is the current excitation, in μAmps, to apply to the sensor. The allowable range is 2500 μA. Reducing the excitation current may allow additional sensors to be excited with one excitation channel. For example, in the example above for MeasPEx, if we decrease the excitation current to 2 mA , (V = IR), 0.002 \* 350 = 0.7 V per resistor. Then divide the maximum compliance voltage for an excitation channel (5 V) by the volts per sensor: 5 V/0.7 V per resistor = 7.14 resistors. So, by decreasing the excitation current to 2 mA instead of 2.5 mA, you could measure up to seven 350-ohm resistors per excitation channel. Note that because output for this instruction is ratometric, output is scaled to the excitation current.

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

**Caution:** Do NOT place this instruction inside a conditional statement when running inpipeline mode

**Note:** A CRBasic program execution mode wherein instructions are evaluated in groups of like instructions, with a set group prioritization.

.
