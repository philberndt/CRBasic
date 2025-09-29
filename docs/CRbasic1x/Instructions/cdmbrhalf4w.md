# CDM_BrHalf4W (CDM 4-Wire Half Bridge Measurement)

The CDM_BrHalf4W instruction is used to make a 4 wire half bridge measurement on aCDM **Note:** . For additional information on controller area network peripherals and Campbell modules, see [CPI and CDM](https://www.campbellsci.com/news-cpi-cdm).

## Syntax

```
CDM_BrHalf4W
```

(CDMType,CPIAddress,Dest,Reps,Range1, Range2,DiffChan,ExChan,MeasPEx,ExmV,RevEx,RevDiff,SettlingTime,fN1,[Mult, Offset](../parameters/multoffset.md),ReturnV1[optional] )

The following code is an example of using the CDM_BrHalf4W instruction to measure a 43347 RTD Temperature Probe.

```
'Declare Variables and Units
Public Batt_Volt
Public RTD_C

Units Batt_Volt=Volts
Units RTD_C=Deg C

'Define Data Tables
DataTable(Table1,True,-1)
DataInterval(0,1,Min,0)
Sample(1,RTD_C,FP2)
EndTable

'Main Program
BeginProg
Scan(5,Sec,1,0)
'Default Datalogger Battery Voltage measurement Batt_Volt:
Battery(Batt_Volt)
'43347 RTD Temperature Probe (not calibrated) measurement RTD_C:
CDM_BrHalf4W(CDM_A108,1,RTD_C,1,mV1000,mV1000,1,X1,1,2500,True,True,0,60,1,0)
PRT(RTD_C,1,RTD_C,1.0267,0)
'Call Data Tables and Store Data
CallTable(Table1)
NextScan
EndProg
```

## Remarks

This instruction applies an excitation voltage and makes two differential voltage measurements, then reverses the polarity of the excitation and repeats the measurements. The measurements are made on sequential channels. The result is the voltage measured on the second channel (V2) divided by the voltage measured on the first (V1). The connections are made so that V1is the voltage drop across the fixed resistor (Rf), and V2is the drop across the sensor (Rs). The result is V2/ V1which equals Rs/ Rf.

Relational Formula:

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

For the CDM_BrHalf4W instruction, measurements are made on consecutive channels. If the Reps parameter is greater than 1, the Dest parameter must be a variable array.

# Range

The expected voltage range of the input from the sensor. An alphanumeric code is entered. Note that not all ranges are valid for all CDM types. Right-click the parameter to display a list of valid ranges.

| Code       | Description                                                                                                                          |
| ---------- | ------------------------------------------------------------------------------------------------------------------------------------ |
| mV5000     | 5000 mV                                                                                                                              |
| mV1000     | 1000 mV                                                                                                                              |
| mV200      | 200 mV                                                                                                                               |
| Autorange  | Datalogger tests for and uses most suitable range.                                                                                   |
| AutorangeC | Autorange, checks for open input.                                                                                                    |
| mV5000C    | 5000 mV, checks for open input, sets excitation to ~5600mV. Overrange values may go undetected; use code to detect overrange values. |
| mV1000C    | 1000 mV, checks for open input, sets excitation to ~1250 mV.                                                                         |
| mV200C     | 200 mV, checks for open input, sets excitation to ~1250 mV.                                                                          |

For signals that do not fluctuate too rapidly, AutoRange allows the CDM to automatically choose the input voltage range. AutoRange results in two voltage measurements being performed. The first voltage measurement is done quickly at a first notch frequency (fN1) of 50 kHz, the result of which determines the input voltage range to use for the second measurement. The second measurement is then performed using the range determined from the first measurement, along with the fN1chosen for the measurement instruction. Both measurements use the settling time chosen for the particular measurement instruction. Auto-ranging optimizes resolution but takes longer than a fixed range measurement because of the two-measurement sequence. The exception to this two-measurement sequence is if Reps are made on the same channel (Reps parameter is negative). In this instance, the test measurement is made only once. Subsequent repetitions are made with the delay between each measurement being the specified settling time. Autorange should not be used if fast measurements are required or if the analog signal could change significantly over the course of the measurement.

The C options check for an open connection on the analog input by applying a short overranging test signal to the analog input prior to making the measurement. For a voltage range of mv5000C,5.6Vis applied. For a voltage range of mv1000C or mv200C, 1.25V is applied. An open (broken) sensor will result in a measurement overrange, clearly indicating a sensor problem instead of returning a bad measured value caused by a floating input.

The open-detect option may not work properly in the presence of external leakage (< 1 MOhm) to ground, as the overrange test signal could discharge through the external leakage during the input settling time such that an overrange condition no longer exists. In this case the measurement settling time should be minimized to minimize the discharge time of the overrange test signal.

The open circuit detection (C option) can cause measurement errors for slow responding sensors, as such sensors may not have sufficient time to recover from the applied short duration (50 microseconds) test signal. For such sensors, increasing the measurement settling time beyond the default may be necessary for sufficient sensor recovery time. If measurement speed is critical with a slow responding sensor, then it may be preferable to not use the open detect (C option).

Type: Constant

Range1 is for the first channel measured; Range2 is for the second channel measured. A

# DiffChan (Differential Channel)

The number of the differential channel pair on which to make the first measurement. The number of available channels depends upon the CDM being programmed. If the Reps parameter is greater than 1, the additional measurements will be made on sequential channels.

If the DiffChan number is entered as a negative value, all Reps will be performed on the same channel (burst measurement). The burst mode sample frequency is controlled by the fN1 parameter, so there are 16 sample frequencies with a maximum of 30 kHz. The SettlingTime parameter is used to delay once prior to beginning the burst. The total time prior to beginning the burst is the SettlingTime plus 180 microseconds.

Type: Constant

# ExChan (Excitation Channel)

Enter the excitation channel number (X1, X2...) used to excite the first measurement. The number of available channels depends upon the CDM being programmed. An alphanumeric code is entered. Right-click within the parameter to display a list of valid options.

Type: Constant

# MeasPEx (Excitation Channels)

The number of sensors to excite with the same excitation terminal before automatically advancing to the next excitation terminal. To excite all sensors with the same excitation terminal, the value of this parameter should be set equal to the value of the Reps parameter.

This parameter in a bridge measurement can be used when the sensors to be measured outnumber the available excitation channels. It allows multiple sensors to be excited with each excitation channel.

For example, assume it is desired to measure eight pressure transducers that have 350 ohm full bridge strain gages to be excited by 5 volts. Each transducer requires 14 mA (5 volts/350 ohms = 14 mA). Since each excitation channel can provide a maximum of 50 mA at 5 volts, a maximum of 3 pressure transducers can be excited per channel (50 mA/14 mA = 3.57). To measure the eight transducers, 8 would be entered for the Reps argument and 3 for the MeasPEx. The first three transducers would all be connected to the first excitation channel, the next 3 to the second excitation channel, and the last 2 to the third excitation channel.

Type: Constant (or expression that evaluates as a constant)

# ExmV (Excitation Voltage in mV)

Excitation, in millivolts, to apply to the sensor. The allowable range is5000 mV.

Type: Constant

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

# ReturnV1

Optional parameter that determines whether or not to return the V1 voltage as shown in the following image. If ReturnV1 is non-zero, V1 will be returned. In this case, the Destination parameter must be a two-element (or greater) array. The first element will hold V2/V1 (the normal output for this instruction). The second element will hold V1.

| Value | State            |
| ----- | ---------------- |
| 0     | Do not return V1 |
| ≠0    | Return V1        |

** Caution:**Do NOT place this instruction inside a conditional statement when running inpipeline mode

** Note:**A CRBasic program execution mode wherein instructions are evaluated in groups of like instructions, with a set group prioritization.

.
