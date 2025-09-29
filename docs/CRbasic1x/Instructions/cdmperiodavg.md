# CDM_PeriodAvg (CDM Period Average)

The CDM_PeriodAvg instruction is used to measure the period (in microseconds) or the frequency (in Hz) of a signal on a single-ended channel of aCDM **Note:** . For additional information on controller area network peripherals and Campbell modules, see [CPI and CDM](https://www.campbellsci.com/news-cpi-cdm).

## Syntax

CDM_PeriodAvg(CDMType,CPIAddress,Dest,Reps,Gain,SEChan,Threshold,Option,Cycles,Timeout,Mult, Offset)

The following program instructions demonstrate the use of CDM_PeriodAvg to measure a CS615 water content reflectometer that outputs a 0.6 to 1.5 kHz square wave that can be measured by the datalogger. The measured period is converted to volumetric water content using a polynomial.

```
'Period Average Example
Public H2Operiod'declaration
Public H2Opercent'declaration
Units H2Operiod = mS
Units H2Opercent = %
BeginProg
Scan (5,Sec,3,0)'5 second scan rate
CDM_SW5(CDM_A108,1,1 ,1 ,0 )'Turn on Sensor by setting port high
CDM_PeriodAvg(CDM_A108,1,H2Operiod,1,0,1,0,0,10,50,.001,0)'period option (mS)
CDM_SW5 (CDM_A108,1,1 ,0,0)'Turn off sensor by setting port low
'Run through a polynomial to calculate percent
H2Opercent=100*((-0.187)+(0.037*H2Operiod)+(0.335*(H2Operiod)^2))
NextScan
EndProg
```

## Remarks

The CDM_PeriodAvg instruction returns the measurement of a signal in either microseconds (period) or Hertz (frequency). Processing instructions can then be used to convert the output to engineering units.

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

For the CDM_PeriodAvg instruction, when the Source is not an array, or only a single variable in the array should be averaged, Reps should be 1.

# Gain

Used to set the voltage gain for the input signal prior to going into the zero crossing comparator. Enter the code for the desired option.

| Code | Gain | Min Signal (peak-to-peak) | Max Signal (peak-to-peak) |
| ---- | ---- | ------------------------- | ------------------------- |
| 0    | 1    | 500                       | 20                        |
| 1    | 3.8  | 50                        | 20                        |
| 2    | 19   | 10                        | 20                        |
| 3    | 66   | 2                         | 20                        |

Type: Constant

# SEChan (Single-Ended Channel)

The single-ended channel to use with this instruction. The number of available channels depends upon the CDM being programmed. If the Reps parameter is greater than 1, the additional measurements will be made on sequential channels. If the SEChan number is entered as a negative value, all Reps will be performed on the same channel (burst measurement). The burst mode sample frequency is controlled by the fN1parameter, which can go up to a maximum of 93750 Hz (minimum sample interval of 10.667 S). The SettlingTime parameter is used to delay once prior to beginning the burst. The total time prior to beginning the burst is the SettlingTime plus the ADC flush time (which is 810 S). The sample interval resolution is 1/93750Hz. The specified notch frequency will use the nearest multiple of (1/93750Hz) to get as close to the specified frequency as possible.

Type: Constant

# Threshold

Determines the threshold, in millivolts, (input referred) at which the comparator will trigger to count transitions. This allows the user to measure signals not centered around the default threshold of zero Volts. This parameter should be set to the average DC voltage of the signal relative to datalogger ground.

# Option

Specifies whether to output the frequency or the period of the signal.

| Code | Description                         |
| ---- | ----------------------------------- |
| 0    | Period of the signal is returned    |
| 1    | Frequency of the signal is returned |

# Cycles

The Cycles parameter specifies the number of cycles to average each scan. The specified number of cycles are timed with a resolution of 135 ns, making the resolution of the period measurement 135 ns divided by the number of Cycles measured.

Type: Constant

# Timeout

The maximum time duration, in milliseconds, that the logger will wait for the number of Cycles to be measured for the average calculation. An overrange value will be stored if the Timeout period is exceeded. The maximum Timeout is 1000 ms.

Type: Constant

# Mult, Offset (Multiplier and Offset)

Factors by which to scale the raw results of the measurement. Typically used to convert the raw measurement to engineering units or to units other than which is output. For example, the TCDiff instruction measures a thermocouple and outputs temperature in degrees C. A multiplier of 1.8 and an offset of 32 will convert the temperature to degrees F.

For temperature measurements, a multiplier (mult) of 1 and an offset of 0, would output in degrees Celsius. For analog measurements, a multiplier (mult) of 1 and an offset of 0, would output the measured voltage in millivolts divided by the excitation voltage in volts.

If Repetitions of greater than 1 are used for this instruction, Repetitions can also be used for the Multiplier and Offset.[SeeMultipliers, Offsets, and Disable Variables with Repetitionsfor more information.](../Info/multipliersoffsets.md)

Type: Constant, Variable, Array, or Expression
