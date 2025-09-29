# HydraProbe (Process Hydra Probe Data)

The HydraProbe instruction is used to process the values returned from a Stevens Vitel Hydra Probe sensor.

## Syntax

HydraProbe(Dest,SourceVolts,ProbeType,SoilType,Multiplier,Offset)

The following example program measures a Stevens Vitel Hydra Probe and uses the HydraProbe instruction to convert the raw voltages to Soil data. The Alias instruction is used to assign descriptive names to the 11 values that are returned by the instruction. After the probe is turned on (using the SW12 instruction) it requires a 3 second warm-up period before the measurement is made.

```
Public V(4), HydraP(11)
Alias HydraP(1) = SoilType
Alias HydraP(2) = eR
Alias HydraP(3) = eI
Alias HydraP(4) = Temp_C
Alias HydraP(5) = eR_tc
Alias HydraP(6) = eI_tc
Alias HydraP(7) = wfv
Alias HydraP(8) = NaCI
Alias HydraP(9) = SoilCond
Alias HydraP(10) = SoilCond_tc
Alias HydraP(11) = SoilWaCond_tc

DataTable (HydraDat,True,-1)
DataInterval (0,60,Min,10)
Sample (11,HydraP(),IEEE4
EndTable

BeginProg
Scan (1,Min,3,0)
'turn on power to sensor, delay 3 sec for warm-up
SW12 (SW12_1,1)
Delay (0,3,Sec)
VoltSe (V(),4,mv5000C,1,1,0,15000,0.001,0)
SW12 (SW12_1,0)
'sensor powered off
'process V()
HydraProbe(HydraP(),V(),0,2,1,0)
CallTable (HydraDat)
NextScan
EndProg
```

## Remarks

The sensor is first measured using a single ended voltage measurement ([VoltSE](voltse.md)). The measurements returned by the sensor must be stored in an array dimensioned to 4. This instruction returns the following 11 data values: soil type (1 = sand, 2 = silt, 3 = clay, 4 = loam), real dielectric constant, imagined dielectric constant, temperature (degrees C), real dielectric constant with temperature correction, imagined dielectric constant with temperature correction, water content (fraction by volume), salinity (grams of NaCl per liter), soil conductivity (S/m), soil conductivity with temperature correction (S/m), soil water conductivity with temperature correction (S/m).

## Parameters

# HydraDestination

The variable array (dimensioned to 11) that will hold the values returned from the Hydra Probe sensor. The sensor returns the following measurements: soil type (1 = sand, 2 = silt, 3 = clay, 4 = loam), real dielectric constant, imagined dielectric constant, temperature, real dielectric constant with temperature correction, imagined dielectric constant with temperature correction, water content (fraction by volume), salinity (grams of NaCl per liter), soil conductivity (S/m), soil conductivity with temperature correction (S/m), soil water conductivity with temperature correction (S/m).

Type: Variable dimensioned to 11

# SourceVolts

A variable array that will hold the voltages returned by the sensor (V1, V2, V3, and V4).

Type: Variable dimensioned to 4

# ProbeType

Identifies which version of the Hydra Probe is being measured. 0 = Standard Probe (5V output for V4); 1 = Probe Type A (2.5 V output for V4). Right-click the parameter to display a drop-down list box.

Type: Constant

# SoitType

Indicates the type of soil being measured: 1 = sand, 2 = silt, 3 = clay, and 4 = loam. Right-click the parameter to display a drop-down list box.

Type: Constant

# HydraMult, HydraOffset

Factors by which to scale the results of the Temperature measurement. A multiplier of 1 and offset of 0 will leave the temperature measurement at degrees C. A multiplier of 1.8 and an offset of 32 will convert it to degrees F.

Type: Constant, Variable, Array, or Expression
