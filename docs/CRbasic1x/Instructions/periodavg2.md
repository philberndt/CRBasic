# PeriodAvg (Period Average)

The PeriodAvg instruction is used to measure the period (in microseconds) or the frequency (in Hz) of a signal on a single-ended channel.

## Syntax

PeriodAvg(Dest,Reps,Gain,Chan,Threshold,Option,Cycles,Timeout,Mult, Offset)

The following program instructions demonstrate the use of PeriodAvg to measure a CS615 water content reflectometer that outputs a 0.6 to 1.5 kHz square wave that can be measured by the datalogger. The measured period is converted to volumetric water content using a polynomial.

```
'Period Average Example
Public H2Operiod'declaration
Public H2Opercent'declaration
Units H2Operiod = mS
Units H2Opercent = %

BeginProg
Scan (5,Sec,3,0)'5 second scan rate
Portset (C1 ,1 )'Turn on Sensor by setting port 1 high (C1)
PeriodAvg(H2Operiod,1,mV5000,1,0,0,10,50,.001,0)'period option (mS)

Portset (C1 ,0)'Turn off sensor by setting port 1 (C1) low
'Run through a polynomial to calculate percent
H2Opercent=100*((-0.187)+(0.037*H2Operiod)+(0.335*(H2Operiod)^2))
NextScan
EndProg
```

## Remarks

The PeriodAvg instruction returns the measurement of a signal in either microseconds (period) or Hertz (frequency). Processing instructions can then be used to convert the output to engineering units.

## Parameters

# Dest (Destination)

The Variable in which to store the results of the instruction. Right-click the parameter to display a list of defined variables.

If this instruction has a Repetitions parameter and it is greater than 1, the results are stored in an array with the variable name. The array must be dimensioned large enough to hold all of the values returned from all of the Reps.

Type: Variable or Array

# Reps (Repetitions)

The number of repetitions for the measurement or instruction.

Type: Constant integer (or expression that evaluates as a constant).

When the Source is not an array, or only a single variable in the array should be averaged, Reps should be 1.

# Gain

Used to set the voltage gain for the input signal prior to going into the zero crossing comparator. Maximum frequency decreases with increasing gain. Enter the code for the desired option.

| Code | Gain | Min Signal (peak-to-peak) | Max Signal (peak-to-peak) | Min Pulse-Width ( s) | Max Frequency (kHz) |
| ---- | ---- | ------------------------- | ------------------------- | -------------------- | ------------------- |
| 0    | 1    | 500 mV                    | 10 V                      | 2.5                  | 200                 |
| 1    | 2.5  | 50 mV                     | 2 V                       | 10                   | 50                  |
| 2    | 12.5 | 10 mV                     | 2 V                       | 62                   | 8                   |
| 3    | 64   | 2 mV                      | 2 V                       | 100                  | 5                   |

Type: Constant

# Chan (Channel)

The channel number on which to make the first measurement. When Reps are greater than 1, subsequent measurements will be automatically made on sequential channels. Right-click the parameter to display a list.

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
