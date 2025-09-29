# Therm107, Therm108, Therm109 (Thermistor Measurements)

The Therm107, Therm108, and Therm109 instructions are used to measure the 107, 108, and 109 thermistors, respectively.

## Syntax

```
Therm107
```

(Dest,Reps,SEChan,ExChan,SettlingTime,fN1,Mult, Offset)

Syntax for all three instructions is the same.

The following program shows an example of measuring two 109 Thermistors using the Therm109 instruction (the 108 and 107 thermistors are measured identically). A multiplier and offset are used in the example to store the measurements in degrees F.

```
'Declare Variables
Public Temp(2)

'Main Program
BeginProg
Scan (1,min,3,0)
Therm109(Temp(),2,1,Vx1,0,15000,1.8,32)
NextScan
EndProg
```

## Remarks

This instruction makes a half bridge voltage measurement and processes the results using the Steinhart-Hart calculation. The output is temperature in degrees C.

## Parameters

# Dest (Destination)

The Variable in which to store the results of the instruction. Right-click the parameter to display a list of defined variables.

If this instruction has a Repetitions parameter and it is greater than 1, the results are stored in an array with the variable name. The array must be dimensioned large enough to hold all of the values returned from all of the Reps.

Type: Variable or Array

# Reps (Repetitions)

The number of repetitions for the measurement or instruction.

Type: Constant integer (or expression that evaluates as a constant).

For the these instructions, measurements are made on consecutive channels. If the Reps parameter is greater than 1, the Dest parameter must be a variable array.

# SEChan

The single-ended channel number on which to make the first measurement. When Reps are used, subsequent measurements will be automatically made on the following channels.

Type: Constant

# ExChan (Excitation Channel)

Enter the excitation channel number to excite first measurement. If multiple thermistors are measured with one instruction, all repetitions will use the same excitation channel.

An alphanumeric code is entered. Right-click within the parameter to display a list.

| Alphanumeric | Numeric | Description          |
| ------------ | ------- | -------------------- |
| VX1          | 1       | Excitation channel 1 |
| VX2          | 2       | Excitation channel 2 |
| VX3          | 3       | Excitation channel 3 |
| VX4          | 2       | Excitation channel 4 |

Type: Constant

# Settling Time

The settling time is the duration (in microseconds) to allow for signal settling after setting up a measurement (switching to the channel, setting the excitation) and before making the measurement.Minimum settling time is 20 microseconds; maximum settling time is 600 ms. If 0 is entered, a default of 500 microseconds is used.

Additional settling time may be necessary to allow the signal to settle with high resistance or long lead lengths (higher capacitance). The time it will take to make the measurement will include the measurement itself, the SettlingTime, fN1, and whether or not parameters are set to remove voltage offset errors. Using either RevDiff or RevEx causes two SettlingTimes to occur per channel; four SettlingTimes will occur when using both RevDiff and RevEx.

Type: Constant (or expression that evaluates as a constant)

# fN1

Determines the lowest frequency that will be eliminated or notched out by the sinc filter. This filter notches out frequencies at integer multiples of fN1by averaging for a time equal to 1/fN1; thus, lower fN1frequencies result in longer measurement times.Set fN1to the lowest frequency that should be filtered out. For example, setting fN1to 60 filters all multiples of 60 and above (60, 120, 180, etc.) but will not effectively filter lower frequencies; for example 30 Hz.Any value between 0.5 Hz and 31.25 kHz can be entered, but common options for filtering noise are:

| Option         | Description                                                       |
| -------------- | ----------------------------------------------------------------- |
| 15000          | Performs a 0.0667 millisecond integration (for fast measurements) |
| 60 (or \_60Hz) | Performs a 16.67 millisecond integration (filters 60 Hz noise)    |
| 50 (or \_50Hz) | Performs a 20 millisecond integration (filters 50 Hz noise)       |

Type: Constant

# Mult, Offset (Multiplier and Offset)

Factors by which to scale the raw results of the measurement. Typically used to convert the raw measurement to engineering units or to units other than which is output. For example, the TCDiff instruction measures a thermocouple and outputs temperature in degrees C. A multiplier of 1.8 and an offset of 32 will convert the temperature to degrees F.

For temperature measurements, a multiplier (mult) of 1 and an offset of 0, would output in degrees Celsius. For analog measurements, a multiplier (mult) of 1 and an offset of 0, would output the measured voltage in millivolts divided by the excitation voltage in volts.

If Repetitions of greater than 1 are used for this instruction, Repetitions can also be used for the Multiplier and Offset.[SeeMultipliers, Offsets, and Disable Variables with Repetitionsfor more information.](../Info/multipliersoffsets.md)

Type: Constant, Variable, Array, or Expression
