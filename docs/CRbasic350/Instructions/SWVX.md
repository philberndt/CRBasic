# SWVX (Switched Voltage Excitation)

The SWVX instruction is used to set an excitation channel high or low. To set a digital I/O port high or low, use the [PortSet](portset.md) instruction.

## Syntax

SWVX (ExChan,State,Voltage)

This example program shows the use of the SWVX instruction to power a sensor that is measured with a voltage measurement.

```
'Declare Variables and Units
Public Batt_Volt
Public AirTC

Units Batt_Volt=Volts
Units AirTC=Deg C

'Define Data Tables
DataTable(Table1,True,-1)
DataInterval(0,60,Min,0)
Average(1,AirTC,FP2,False)
EndTable

'Main Program
BeginProg
Scan(5,Sec,1,0)
'Battery Voltage measurement
Battery(Batt_Volt)
'Sensor measurement
SWVX(Vx1,1,1)'Set excitation channel 1 to 5V
Delay(0,150,mSec)
VoltSe(AirTC,1,mv2500,2,0,0,4000,0.1,-40.0)
SWVX(VX1,0,1)'Set excitation channel 1 low
'Call Data Tables and Store Data
CallTable(Table1)
NextScan
EndProg
```

## Remarks

CR350dataloggers have2excitation channels. These channels can be used to provide a regulated 5 V or 3.3 V supply to external peripherals under program control. If more specific voltage levels are desired, consider using [ExciteV](excitev.md) to power peripherals.

## Parameters

# ExChan (Excitation Channel)

The EXCHan argument specifies the excitation channel that should be set by the instruction. An alpha-numeric code is entered:

| Alphanumeric Code | Description          |
| ----------------- | -------------------- |
| VX1               | Excitation channel 1 |
| VX2               | Excitation channel 2 |

Type: Constant

# State

Determines whether to set the port high or low. Right-click to display a list.

| Value | State |
| ----- | ----- |
| 0     | Low   |
| ≠0    | High  |

Type: Constant, Variable, or Expression

# Voltage

Sets the voltage for the VX channel.

| Option | Description  |
| ------ | ------------ |
| 0      | 3.3V         |
| 1      | 5V (default) |

Type: Numeric
