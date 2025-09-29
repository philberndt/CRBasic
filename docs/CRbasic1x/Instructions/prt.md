# PRT (Calculate Temperature from RTD Resistance)

The PRT instruction is used to calculate temperature from the resistance of an RTD.

**NOTE:** Before using this instruction, see the newer PRTCalc instruction. PRTCalc implements conversion to temperature according to a range of alternative standards, including the latest IEC standard which references to the ITS-90 temperature scale. The PRT instruction has effectively been superseded by PRTCalc.

## Syntax

PRT(Dest,Reps,Source,Mult, Offset)

The following program provides an example of measuring an RM Young PRT sensor. A half-bridge measurement is made, and the PRT instruction converts the result of that measurement into temperature in degrees C.

```
'CR1000X
'Example program to measure the 43347 RM Young PRT sensor using the
'BrHalf4W measurement instruction. The PRT instruction converts
'the measurement result to temperature.

'Declare Variables and Units
Public RTD_C
Units RTD_C=Deg C

'Define Data Tables
DataTable (Table1,True,-1)
DataInterval (0,60,Min,0)
Average (1,RTD_C,IEEE4,0)
EndTable

'Main Program
BeginProg
Scan (5,Sec,1,0)
'43347 RTD Temperature Probe (not calibrated) measurement RTD_C:
BrHalf4W (RTD_C,1,mv200C,mv200C,1,Vx1,1,2500,True,True,0,15000,1,0)

PRT(RTD_C,1,RTD_C,1.0267,0)
''Call Data Tables and Store Data
CallTable(Table1)
NextScan
EndProg
```

## Remarks

This instruction uses the result of a previous RTD bridge measurement to calculate the temperature in degrees Celsius. The input (Source) must be the ratio RS/RO, where RS is the RTD resistance and RO is the resistance of the RTD at 0 degrees Celsius.

The temperature is calculated according to the DIN 43760 specification adjusted (1980) to the International Electrotechnical Commission standard, referenced to the IPTS-68 (international temperature scale as defined in 1968). The range of linearization is -200 to 850 degrees Celsius. The error in the linearization is less than 0.001 degrees between -100 and +300 degrees Celsius, and is less than 0.003 degrees between -180 and +830 degrees Celsius. The error (T calculated - T standard) is +0.006 degrees at -200 degrees Celsius and -0.006 degrees at +850 degrees Celsius.

## Parameters

# Dest (Destination)

The Variable in which to store the results of the instruction. Right-click the parameter to display a list of defined variables.

If this instruction has a Repetitions parameter and it is greater than 1, the results are stored in an array with the variable name. The array must be dimensioned large enough to hold all of the values returned from all of the Reps.

Type: Variable or Array

# Reps (Repetitions)

The number of repetitions for the measurement or instruction.

Type: Constant integer (or expression that evaluates as a constant).

Measurements are made on consecutive channels. If the Reps parameter is greater than 1, the Dest parameter must be a variable array.

# Source

The name of the Variable that is the input for the instruction. Right-click the parameter to display a list of defined variables.

Type: Variable

# Mult, Offset (Multiplier and Offset)

Factors by which to scale the raw results of the measurement. Typically used to convert the raw measurement to engineering units or to units other than which is output. For example, the TCDiff instruction measures a thermocouple and outputs temperature in degrees C. A multiplier of 1.8 and an offset of 32 will convert the temperature to degrees F.

For temperature measurements, a multiplier (mult) of 1 and an offset of 0, would output in degrees Celsius. For analog measurements, a multiplier (mult) of 1 and an offset of 0, would output the measured voltage in millivolts divided by the excitation voltage in volts.

If Repetitions of greater than 1 are used for this instruction, Repetitions can also be used for the Multiplier and Offset.[SeeMultipliers, Offsets, and Disable Variables with Repetitionsfor more information.](../Info/multipliersoffsets.md)

Type: Constant, Variable, Array, or Expression
