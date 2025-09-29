# VaporPressure (Calculate Vapor Pressure)

The VaporPressure instruction is used to calculate the vapor pressure from temperature and relative humidity measurements in the program.

## Syntax

VaporPressure(Dest,Temp,RH)

The following program uses the VaporPressure instruction to derive vapor pressure, in kilopascals, from temperature and RH measurements in the program.

```
Public Temp, RH, VPkPa

DataTable (EnvData,1,-1)
DataInterval (0,1,Min,10)
Sample (1,Temp,FP2)
Sample (1,RH,FP2)
Sample (1,VPkPa,FP2)
EndTable

BeginProg
Scan (1,Sec,1,0)
'Begin Code for HMP45C AirTemp and RH
PortSet(C1,1)
Delay(0,150,mSec)
VoltSE(Temp,1,mv5000C,2,0,0,15000,0.1,-40.0)

VoltSE(RH,1,mv5000C,1,0,0,15000,0.1,0)

PortSet(C1,0)
```

'End code for HMP45C
VaporPressure(VPkPa,Temp,RH)
CallTable EnvData
NextScan
EndProg

```

## Remarks


The vapor pressure is found from relative humidity and saturation vapor pressure using the following equation. The saturation vapor pressure is found from the dry bulb temperature.

VP = RH\*SVP/100

where VP = vapor pressure and SVP = saturation vapor pressure.

The result is in kilopascals.


## Parameters



# Dest (Destination)


The Variable in which to store the results of the instruction. Right-click the parameter to display a list of defined variables.

If this instruction has a Repetitions parameter and it is greater than 1, the results are stored in an array with the variable name. The array must be dimensioned large enough to hold all of the values returned from all of the Reps.

Type: Variable or Array


# Temp (Temperature)


The variable in the program that contains the measurement, in degrees C, for temperature.

Type: Variable

For the VaporPressure instruction, the Temp parameter is the program variable that contains the value for the temperature sensor.


# RH (Relative Humidity)


The variable in the program that contains the measurement for relative humidity.

Type: Variable

For the VaporPressure instruction, the RH measurement must be in percent of RH.
```
