# SWVX (Switched Voltage Excitation)

The SWVX instruction is used to set an excitation channel high or low. To set a digital I/O port high or low, use the [PortSet](portset.md) instruction.

## Syntax

SWVX (ExChan,State,Voltage,SWOption)

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
VoltSe(AirTC,1,mv5000C,2,0,0,15000,0.1,-40.0)
SWVX(VX1,0,1)'Set excitation channel 1 low
'Call Data Tables and Store Data
CallTable(Table1)
NextScan
EndProg
```

## Remarks

CR1000Xdataloggers have4excitation channels. These channels can be used to provide a regulated 5 V or 3.3 V supply to external peripherals under program control. If more specific voltage levels are desired, consider using [ExciteV](excitev.md) to power peripherals.

## Parameters

# ExChan (Excitation Channel)

The EXCHan argument specifies the excitation channel that should be set by the instruction. An alpha-numeric code is entered:

| Alphanumeric Code | Description          |
| ----------------- | -------------------- |
| VX1               | Excitation channel 1 |
| VX2               | Excitation channel 2 |
| VX3               | Excitation channel 3 |
| VX4               | Excitation channel 4 |

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

**NOTE:** When the datalogger is powered by USB only, the voltage is ~ 2.5 V for either voltage option.

# Option (Run Mode Option)

An optional parameter that determines whether the instruction will run in the measurement task sequence or the processing task sequence, and also affects whether the program will compile and run in [SequentialMode or PipelineMode](sequentialmodepipeli2.md):

| Option  | Description                                                                                                |
| ------- | ---------------------------------------------------------------------------------------------------------- |
| Omitted | Instruction is run within the measurement task sequence; program will compile in SequentialMode            |
| 0       | Instruction is run within the measurement task sequence; program will attempt to compile in PipelineMode\* |
| 1       | Instruction is run within the processing task sequence; program will attempt to compile in PipelineMode\*  |

\* other programming may force the program into SequentialMode

Running this instruction in the processing task when the program is run inpipeline mode

**Note:** A CRBasic program execution mode wherein instructions are evaluated in groups of like instructions, with a set group prioritization.

can prove to be problematic if you are using the instruction to power a sensor. Because processing tasks can lag behind measurement tasks in PipelineMode, the instruction may be processed by the device after the measurement has already been made. To avoid this scenario, program the datalogger to operate in SequentialMode.

Type: Constant
