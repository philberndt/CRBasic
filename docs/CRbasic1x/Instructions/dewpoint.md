# DewPoint (Dew Point)

The DewPoint instruction is used to calculate the dew point temperature from dry bulb temperature and relative humidity measurements in the program.[For more information, seeEstimation of Dew Point.](estimationofdewpoint.md)

## Syntax

DewPoint(Dest,Temp,RH)

The following program uses the DewPoint instruction to derive dew point temperature from temperature and RH measurements in the program.

```
Public Temp, RH, DewPt

DataTable (EnvData,1,-1)
DataInterval (0,1,Min,10)
Sample (1,Temp,FP2)
Sample (1,RH,FP2)
Sample (1,DewPt,FP2)
EndTable

BeginProg
Scan (1,Sec,1,0)
'Begin Code for HMP45C AirTemp and RH
PortSet(C1,1)
Delay(0,150,mSec)
VoltSE(Temp,1,mv5000C,2,0,0,60,0.1,-40.0)
VoltSE(RH,1,mv5000C,1,0,0,60,0.1,0)

PortSet(C1,0)
'End code for HMP45C
DewPoint(DewPt,Temp,RH)
CallTable EnvData
NextScan
EndProg
```

## Remarks

The dew point temperature is found from the vapor pressure, relative humidity, and the saturation vapor pressure. The saturation vapor pressure is found from the dry bulb temperature, and the vapor pressure from saturation vapor pressure and relative humidity.

The dew point is found from Tetens' equation solved for dew point with coefficients optimized for the temperature range of -35 to +50 degrees C. .

where:

Td= dew point temperature

A1= 0.61078

A2= 17.558

A3= 241.88

The result is in degrees C. Error rate with this formula is less than 0.1 degrees C.

## Parameters

# Dest (Destination)

The Variable in which to store the results of the instruction. Right-click the parameter to display a list of defined variables.

If this instruction has a Repetitions parameter and it is greater than 1, the results are stored in an array with the variable name. The array must be dimensioned large enough to hold all of the values returned from all of the Reps.

Type: Variable or Array

# Temp (Temperature)

The variable in the program that contains the measurement, in degrees C, for temperature.

Type: Variable

# RH (Relative Humidity)

The variable in the program that contains the measurement for relative humidity.

Type: Variable

For the DewPoint instruction, the RH parameter must be in percent. For additional information, see [Estimation of Dew Point](estimationofdewpoint.md).
