# CDM_SW12 (CDM Switched 12 Volt)

The CDM_SW12 instruction is used to set a switched 12 volt channel on theCDM **Note:** high or low. For additional information on controller area network peripherals and Campbell modules, see [CPI and CDM](https://www.campbellsci.com/news-cpi-cdm).

## Syntax

```
CDM_SW12
```

(CDMType,CPIAddress,Port,State,SWOption)

This example program shows the use of the CDM_SW12 instruction to power an HMP45C temperature and RH sensor.

```
'Declare Variables and Units
Public Batt_Volt
Public AirTC
Public RH

Units Batt_Volt=Volts
Units AirTC=Deg C
Units RH=%

'Define Data Tables
DataTable(Table1,True,-1)
DataInterval(0,60,Min,0)
Average(1,AirTC,FP2,False)
Sample(1,RH,FP2)
EndTable

'Main Program
BeginProg
Scan(5,Sec,1,0)
'Battery Voltage measurement Batt_Volt:
CDM_Battery( CDM_A108,1,Batt_Volt)
'HMP45C measurements AirTC and RH:
CDM_SW12(CDM_A108,1,1,1,0)
CDM_Delay(CDM_A108,1,0,150,mSec)
CDM_VoltSe(CDM_A108,1,AirTC,1,mV5000,2,0,0,60,0.1,-40.0)
CDM_VoltSe(CDM_A108,1,RH,1,mV5000,1,0,0,60,0.1,0)
CDM_SW12(CDM_A108,1,1,0)
If RH>100 And RH<108 Then RH=100
'Call Data Tables and Store Data
CallTable(Table1)
NextScan
EndProg
```

## Remarks

The CDM device has one or more switched 12 volt outputs, depending upon the device. These channels are used to provide a 12 volt supply to external peripherals under program control. At room temperature the switched 12 volt supply can source 900 mA between the SW12 terminal and Ground.

## Parameters

# CDMType (CDM Type)

Used to specify the type ofCDM **Note:** used by the instruction. This is a read-only setting in the device which cannot be changed. Right-click the parameter to display a list of valid CDM types.

Type: Constant

# CPIAddress (CPI Address)

TheCPI

** Note:**CPI is a proprietary interface for communications between Campbell Scientific dataloggers and Campbell Scientific CDM peripheral devices. It consists of a physical layer definition and a data protocol. CDM devices are similar to Campbell Scientific SDM devices in concept, but the use of the CPI bus enables higher data-throughput rates and use of longer cables. CDM devices require more power to operate in general than do SDM devices.

address configured in theCDM ** Note:** module. Valid range is 1 through 120. Note that the CPIAddress cannot be a variable because the datalogger must know all information for a given CDM module at program compile.(e.g., before the program runs).

Type: Constant integer

# Port (Switched 12 V Port)

The switched 12 volt port (1 = S12-1 or 2 = S12-2) to use with this instruction. The number of available ports depends upon the CDM being programmed. Right-click the parameter for a list of valid options.

Type: Constant

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

**NOTE:** The SW12 supply is unregulated and can supply up to 1.10 A at 20 degrees C, 640 mA at 70 degrees C, and 500 mA at 85 degrees C. A resettable polymeric fuse protects against over-current. Reset is accomplished by removing the load or turning off the SW12 for several seconds.
