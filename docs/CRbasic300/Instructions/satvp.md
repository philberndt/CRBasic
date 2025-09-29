# SatVP (Saturation Vapor Pressure)

The SatVP instruction is used to calculate the vapor pressure (in kilopascals) over water from a temperature measurement.

## Syntax

SatVP(Dest,Temp)

The following program uses the SatVP instruction to derive saturation vapor pressure, in kilopascals, from a temperature measurement in the program.

```
Public Temp, SatVPkPa

DataTable (EnvData,1,-1)
DataInterval (0,1,Min,10)
Sample (1,Temp,FP2)
Sample (1,SatVPkPa,FP2)
EndTable

BeginProg
Scan (1,Sec,3,0)
VoltSE(Temp,1,mv34,1,0,0,4000,0.1,-40.0)

SatVP(SatVPkPa,Temp)
CallTable EnvData
NextScan
EndProg
```

## Remarks

The SatVP instruction will find the saturation vapor pressure if the dry bulb temperature is used or vapor pressure if the dew point temperature is used.

The saturation vapor pressure is derived using the following polynomial (reference Lowe, Paul R.: 1977, "An approximating polynomial for computation of saturation vapor pressure," Journal of Applied Meteorology, 16, 100-103), adjusted from units of millibars to kilopascals:

- where:

- T = temperature

- A0 = 6.107799961

- A1 = 4.436518521E-01

- A2 = 1.428945805E-02

- A3 = 2.650648471E-04

- A4 = 3.031240396E-06

- A5 = 2.034080948E-08

- A6 = 6.136820929E-11

This instruction can also be used to calculate [saturation vapor pressure over ice](satvpoverice.md).

## Parameters

# Dest (Destination)

The Variable in which to store the results of the instruction. Right-click the parameter to display a list of defined variables.

If this instruction has a Repetitions parameter and it is greater than 1, the results are stored in an array with the variable name. The array must be dimensioned large enough to hold all of the values returned from all of the Reps.

Type: Variable or Array

# Temp (Temperature)

The variable in the program that contains the measurement, in degrees C, for temperature.

Type: Variable

For the SatVP instruction, the Temp parameter is the program variable that contains the value for the temperature sensor.
