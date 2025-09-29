# WetDryBulb (Calculate Vapor Pressure)

The WetDryBulb instruction calculates vapor pressure (kPa) from wet and dry bulb temperatures and air pressure.

## Syntax

WetDryBulb(Dest,DryTemp,WetTemp,Pressure)

The following program uses the WetDryBulb instruction to derive vapor pressure, in kilopascals, from a temperature and pressure measurements in the program. Note that the barometric pressure is measured only once per hour, since this value changes slowly.

```
'CR1000X SeriesDatalogger
SequentialMode
Public AirTemp, WetTemp, Bar_kPA, VpKPA

DataTable (EnvData,1,-1)
DataInterval (0,1,Min,10)
Sample (1,AirTemp,FP2)
Sample (1,WetTemp,FP2)
Sample (1,Bar_kPA,FP2)
Sample (1,VpKPA,FP2)
EndTable

BeginProg
Scan (1,Sec,3,0)

VoltSE(AirTemp,1,mv5000C,1,0,0,15000,0.1,-40.0)

VoltSE(WetTemp,1,mv5000C,2,0,0,15000,0.1,-40.0)
If TimeIntoInterval(59,60,Min) Then PortSet(C2,1)
If TimeIntoInterval(0,60,Min) Then

VoltSE(Bar_kPa,1,mv5000C,3,False,0,15000,0.184,600)
Bar_kPa=Bar_kPa*0.1
PortSet(C2,0)
EndIf
WetDryBulb(VpKPA,AirTemp,WetTemp,Bar_kPA)
CallTable EnvData
NextScan
EndProg
```

## Remarks

The WetDryBulb instruction uses an equation given by List, Robert. J.: 1966, Smithsonian Meteorological Tables, Sixth Revised Edition, Smithsonian Institution, Washington, D.C., page 365. This equation is also used by the National Weather Service. The results are in kPa.

where:

- VPW = Saturation vapor pressure at the wet bulb temperature

- TW = Wet bulb temperature, degrees C

- TA = Ambient air temperature, degrees C

- P = Pressure, kilopascals

- A0 = 0.000660

- A1 = 0.00115

## Parameters

# Dest (Destination)

The Variable in which to store the results of the instruction. Right-click the parameter to display a list of defined variables.

If this instruction has a Repetitions parameter and it is greater than 1, the results are stored in an array with the variable name. The array must be dimensioned large enough to hold all of the values returned from all of the Reps.

Type: Variable or Array

# DryTemp (Ambient Air Temperature Value)

The program variable that contains the value for the ambient air temperature sensor. The temperature measurement must be in degrees C.

Type: Variable

# WetTemp (Wet Bulb Temperature Value)

The program variable that contains the value for the wet bulb temperature sensor. The temperature measurement must be in degrees C.

Type: Variable

# Pressure (Air Pressure Value)

The program variable that contains the value for the air pressure sensor. The air pressure measurement must be in kilopascals. If an air pressure sensor is not available, for most applications a constant pressure value for the site can be used without significantly affecting the resulting vapor pressure calculation.

Type: Variable or Constant
