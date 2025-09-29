# CDM_SWPower (CDM Switched Power)

The CDM_SWPower instruction is used to set switched 12V and switched 5V power high or low on VOLT 408 isolation modules.

## Syntax

CDM_SWPower(CDMType,CPIAddress,State,SWOption)

This example program shows the use of the CDM_SWPower instruction to power a temperature sensor.

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
'Battery Voltage measurement Batt_Volt:
CDM_Battery( VOLT408V60,1,Batt_Volt)
CDM_SWPower(VOLT408V60,1,1,0)'set power high
CDM_Delay(VOLT408V60,1,0,150,mSec)
CDM_VoltDiff(VOLT408V60,1,AirTC,1,Auto,2,0,0,60,0.1,-40.0)
CDM_SWPower(VOLT408V60,1,0,0)'set power low
'Call Data Tables and Store Data
CallTable(Table1)
NextScan
EndProg
```

## Remarks

Each Volt 408 isolation module has one switched 12V port and one switched 5V port. The 12V port and the 5V port are switched together with a single CDM_SWPower() instruction. The switched 12V port and the switched 5V port cannot be set independently. These ports are used to provide a 12V or a 5V supply to external peripherals under program control. The current limit of the 12 V port is 200 mA. The current limit of the 5V port is 300 mA.

## Parameters

# CDMType (CDM Type)

Used to specify the type ofCDM **Note:** used by the instruction. This is a read-only setting in the device which cannot be changed. Right-click the parameter to display a list of valid CDM types.

Type: Constant

# CPIAddress (CPI Address)

TheCPI

** Note:**CPI is a proprietary interface for communications between Campbell Scientific dataloggers and Campbell Scientific CDM peripheral devices. It consists of a physical layer definition and a data protocol. CDM devices are similar to Campbell Scientific SDM devices in concept, but the use of the CPI bus enables higher data-throughput rates and use of longer cables. CDM devices require more power to operate in general than do SDM devices.

address configured in theCDM ** Note:** module. Valid range is 1 through 120. Note that the CPIAddress cannot be a variable because the datalogger must know all information for a given CDM module at program compile.(e.g., before the program runs).

Type: Constant integer

# State

Determines whether to set the port high or low. Right-click to display a list.

| Value | State |
| ----- | ----- |
| 0     | Low   |
| ≠0    | High  |

Type: Constant, Variable, or Expression

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
