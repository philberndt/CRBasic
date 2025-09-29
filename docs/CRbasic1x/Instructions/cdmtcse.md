# CDM_TCSE (CDM Thermocouple Measurement on Single-Ended Channel)

The CDM_TCSE instruction is used to make a thermocouple measurement on a single-ended channel and convert the measurement to degrees Celsius. For additional information on controller area network peripherals and Campbell modules, see [CPI and CDM](https://www.campbellsci.com/news-cpi-cdm).

## Syntax

CDM_TCSE(CDMType,CPIAddress,Dest,Reps,Range,SEChan,TCType,TRef,MeasOff,SettlingTime,fN1,Mult, Offset)

The following sample code shows the use of the CDM_TCSE instruction to measure 5 single-ended thermocouples.

```
Public PTemp, TCTemp(5)

DataTable (Test,True,-1)
DataInterval (0,1,Min,10)
Sample (5,TCTemp,FP2)
EndTable

BeginProg
Scan (1,Sec,3,0)
CDM_PanelTemp (CDM_A108,1,PTemp,1,1,15000)
CDM_TCSE(CDM_A108,1,TCTemp(),5,mv200,1,TypeT,PTemp,True ,0,60,1.0,0)
calltable (Test)
NextScan
EndProg
```

## Remarks

The calculations used for the CDM_TCSE instruction are based on the thermocouple type selected (TCType). The instruction adds the measured voltage to the voltage calculated for the reference temperature relative to 0 degrees Celsius, and converts the combined voltage to temperature in degrees Celsius.

The CDM has thermisters under the wiring panel that can be used as reference temperatures for thermocouple measurements. Refer to the CDM_PanelTemp instruction.

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

For the CDM_TCSE instruction, measurements are made on consecutive channels. If the Reps parameter is greater than 1, the Dest parameter must be a variable array.

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

# SEChan (Single-Ended Channel)

The single-ended channel to use with this instruction. The number of available channels depends upon the CDM being programmed. If the Reps parameter is greater than 1, the additional measurements will be made on sequential channels. If the SEChan number is entered as a negative value, all Reps will be performed on the same channel (burst measurement). The burst mode sample frequency is controlled by the fN1parameter, which can go up to a maximum of 93750 Hz (minimum sample interval of 10.667 S). The SettlingTime parameter is used to delay once prior to beginning the burst. The total time prior to beginning the burst is the SettlingTime plus the ADC flush time (which is 810 S). The sample interval resolution is 1/93750Hz. The specified notch frequency will use the nearest multiple of (1/93750Hz) to get as close to the specified frequency as possible.

Type: Constant

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

** NOTE:**When using TEMP408 modules, the TCType selected must match the module type.

Type: Constant

# TRef (Temperature Reference)

The name of the variable that is the source of the reference temperature (or the result of the reference temperature measurement), in degrees C, for the thermocouple measurements. Right-click the parameter to display a list of defined variables.

Type: Variable, Array, or Expression

# MeasOff (Measure Offset Voltage)

Determines whether the ground offset voltage is measured before the measurement is made on the analog channel. If the ground offset is measured it is subtracted from the sensor measurement and the result is stored in Dest.

| Value | Result                                                                                                                                               |
| ----- | ---------------------------------------------------------------------------------------------------------------------------------------------------- |
| 0     | Offset voltage is corrected from background calibration.                                                                                             |
| 1     | Offset voltage is measured each scan (this option effectively increases the measurement time as if an additional rep were added to the instruction). |

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

** Caution:**Do NOT place this instruction inside a conditional statement when running inpipeline mode

** Note:**A CRBasic program execution mode wherein instructions are evaluated in groups of like instructions, with a set group prioritization.

.
