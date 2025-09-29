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
Therm109(Temp(),2,1,Vx1,0,4000,1.8,32)
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
