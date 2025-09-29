# PRTCalc (Calculate Temperature from RTD)

The PRTCalc instruction is used to calculate temperature from the resistance of anRTD

**Note:** Resistance temperature detector

. Vaious RTD types are supported (refer to the PRTType parameter, following).

## Syntax

PRTCalc(Dest,Reps,Source,PRTType,Mult, Offset)

The following program provides an examples a standard IEC60751 100 ohm platinum resistance sensor (PRT).

A half-bridge measurement Is made, AND the PRTCalc instruction converts the result of that measurement into temperature in degrees C.

```
'CR1000X
'Example program to measure a standard IEC60751 100 ohm PRT sensor
'Uses the BrHalf4W measurement instruction. The PRTCalc instruction converts
'the measurement result to temperature.

'Declare Variables and Units
Public RRo, RTD_C
Units RTD_C=Deg C

'Define Data Tables
DataTable(Hourly,True,-1)
DataInterval(0,60,Min,0)
Average(1,RTD_C,IEEE4,0)
EndTable

'Main Program
BeginProg
Scan(5,Sec,1,0)

'Standard IEC PRT measured in a 4-wire half bridge, for example using a 4WPB100 TIM
'This assumes the bridge resistor used exactly matches the resistance of the sensor at zero Celsius.
'If not, the multiplier in BRHalf4W should be changed 'from 1.0
'The excitation and input ranges assume a 4WPB100 and the max temp to measure is '+ 40 C.
'The output is the ratio of the PRT resistance to its resistance at zero Celsius
BrHalf4W(RRo,1,mv200C,mv200C,1,Vx1,1,2100,False,True,0,60,1.0,0)

'Call PRTCalc with the sensor type set 1 for the IEC 60751 type probe
'Output to the variable RTD_C, in degrees Celsius
PRTCalc(RTD_C,1,RRo,1,1,0)

'Call Data Tables and Store Data
CallTable(Hourly)
NextScan
EndProg
```

## Remarks

This instruction uses the result of a previous RTD bridge measurement to calculate the temperature in degrees Celsius. The input (Source) must be the ratio RS/RO, where RS is the RTD resistance and RO is the resistance of the RTD at 0 degrees Celsius.

A number of different sensor types are supported. The correct sensor type should be chosen to match the standard to which the sensor is said to conform and/or the alpha value for the sensor which is a fundamental measure of the change of resistance for a given temperature change.

For industrial grade RTDs the relationship between temperature and resistance are characterized by a formula called the Callendar-Van Dusen (CVD) equation. The parameters for different sensor types are given in the standards or by the manufacturers for non-standard types. Temperature is now referenced to the ITS-90 temperature scale. PRTCalc follows the principles given in the US ASTM E1137-04 standard for conversion back from resistance to temperature. For the temperature range of 0 to +850 degrees Celsius a direct solution to the CVD equation is used resulting in errors <+/-0.0005 Celsius (caused by rounding errors in the datalogger math). For the range of -200 to 0 Celsius a 4th order polynomial is used to convert from resistance to temperature resulting in errors of <+/-0.003 Celsius.

Note these errors are only the errors in approximating the relationships between temperature and resistance given in the relevant standards. The CVD equations and the tables published from them are in reality an approximation to the true linearity of an RTD, but are deemed adequate for industrial use. Errors in that approximation can be several hundredths of a degrees Celsius at different points in the temperature range and will vary from sensor to sensor. In addition individual sensors have errors relative to the standard, which can be up to +/-0.3 Celsius at 0 Celsius with increasing errors away from 0 Celsius, depending on the grade of sensor. To achieve the highest accuracy it is usually best to calibrate individual sensors over the range of use and apply corrections to the RS/RO value input to the instruction (by using the calibrated value of RO) and the multiplier and offset parameters of PRTCalc.

## Parameters

# Dest (Destination)

The Variable in which to store the results of the instruction. Right-click the parameter to display a list of defined variables.

If this instruction has a Repetitions parameter and it is greater than 1, the results are stored in an array with the variable name. The array must be dimensioned large enough to hold all of the values returned from all of the Reps.

Type: Variable or Array

# Reps (Repetitions)

The number of repetitions for the measurement or instruction.

Type: Constant integer (or expression that evaluates as a constant).

The Reps parameter is the number of times the calculation should be made. Repetitions are made on consecutive elements in the Source array. If the Reps parameter is greater than 1, the Dest parameter must be a variable array.

# Source

The name of the Variable that is the input for the instruction. Right-click the parameter to display a list of defined variables.

Type: Variable

For the PRTCalc instruction, the Source parameter is the variable or array that contains the resistance (RS/RO) of the PRT.

# Type

Specifies the platinum resistance sensor type.

| Code | Description                                                                                                                                                                                                                                                                                                                                           |
| ---- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| 0    | FachnormenausschuB Elektrotchnek im Deutschen NormenausschuB, DIN 43760, alpha = 0.003850                                                                                                                                                                                                                                                             |
| 1    | IEC 60751:2008 (formally known as IEC 751), alpha = 0.00385. Now internationally adopted and written into national standards, for example ASTM E1137-04, JIS 1604:1997, EN 60751 and others. This should be used with any probes claiming compliance with those or older standards where the probe has alpha = 0.00385, for example DIN43760, BS1904. |
| 2    | US Industrial Standard, alpha = 0.00392                                                                                                                                                                                                                                                                                                               |
| 3    | US Industrial Standard, alpha = 0.00391                                                                                                                                                                                                                                                                                                               |
| 4    | Old Japanese Standard JIS C 1604:1981, alpha = 0.003916                                                                                                                                                                                                                                                                                               |
| 5    | Honeywell Industrial Sensors, alpha = 0.00375                                                                                                                                                                                                                                                                                                         |
| 6    | ITS-90 SPRT, alpha = 0.003926                                                                                                                                                                                                                                                                                                                         |

Type: Constant

# Mult, Offset (Multiplier and Offset)

Factors by which to scale the raw results of the measurement. Typically used to convert the raw measurement to engineering units or to units other than which is output. For example, the TCDiff instruction measures a thermocouple and outputs temperature in degrees C. A multiplier of 1.8 and an offset of 32 will convert the temperature to degrees F.

For temperature measurements, a multiplier (mult) of 1 and an offset of 0, would output in degrees Celsius. For analog measurements, a multiplier (mult) of 1 and an offset of 0, would output the measured voltage in millivolts divided by the excitation voltage in volts.

If Repetitions of greater than 1 are used for this instruction, Repetitions can also be used for the Multiplier and Offset.[SeeMultipliers, Offsets, and Disable Variables with Repetitionsfor more information.](../Info/multipliersoffsets.md)

Type: Constant, Variable, Array, or Expression
