# SequentialMode, PipeLineMode (Processing Modes)

SequentialMode and PipeLineMode instructions are used to determine how the datalogger will handle instruction processing in the program.

## Syntax

SequentialMode

PipeLineMode

In the followingCR1000Xprogram, the SequentialMode instruction is used to force the datalogger to execute all instructions in the order they appear in the program. This allows the barometric pressure reading to be made only once per hour (i.e., conditionally, based on time).

This program was written for aCR1000X, but other dataloggers can use similar code (voltage ranges, channel numbers, or other parameters may need to be changed to reflect the specifications of the datalogger).

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
'Code for HMP45C measurements AirTemp and RH:
VoltSE(AirTemp,1,mv5000C,1,False,0,15000,0.1,-40)

VoltSE(WetTemp,1,mv5000C,2,False,0,15000,0.1,-40)

'Code for measuring Barometer, based on time
If TimeIntoInterval(59,60,Min) Then PortSet(C2,1)
If TimeIntoInterval(0,60,Min) Then
VoltSE(Bar_kPa,1,mv5000C,3,False,0,15000,0.184,600)

Bar_kPa=Bar_kPa*0.1
PortSet(C2,0)
EndIf
WetDryBulb (VpKPA,AirTemp,WetTemp,Bar_kPA)
CallTable EnvData
NextScan
EndProg
```

## Remarks

The datalogger has two processing modes:sequential mode

**Note:** A CRBasic program execution mode wherein each statement is evaluated in the order it is listed in the program.

andpipeline mode

**Note:** A CRBasic program execution mode wherein instructions are evaluated in groups of like instructions, with a set group prioritization.

. In sequential mode, instructions are executed by the datalogger sequentially as they occur in the program. In pipeline mode, measurement tasks and processing tasks are handled separately (in a "measurement task sequencer" and a "processing task sequencer") and executed concurrently. See [Processing Modes](../Info/ProcessingModes.md) for a list of instructions run in the "measurement task sequencer" and instructions run in the "processing task sequencer."

When running in PipelineMode, the datalogger allocates two buffers for processing tasks by default. The number of buffers can be increased in the BufferOption parameter of the Scan instruction. The buffers can be set to no fewer than two (0, 1, or 2, can be entered for the parameter, but two buffers will still be allocated). Additional buffers can be allocated if more are needed when extensive processing is taking place at certain times during program execution. Keep in mind, however, that these buffers use memory (See the BufferOption parameter in the help for the [Scan](scannextscan.md) instruction for additional information). The MaxBuffDepth value in the datalogger **Status** table can be monitored to help understand the optimum number of buffers to allocate for a program.

The default mode of operation ispipeline mode

**Note:** A CRBasic program execution mode wherein instructions are evaluated in groups of like instructions, with a set group prioritization.

. However, when the datalogger program is compiled, the datalogger analyzes the program instructions and automatically switches tosequential mode

**Note:** A CRBasic program execution mode wherein each statement is evaluated in the order it is listed in the program.

if the code requires it. The datalogger can be forced to run in either mode by placing the appropriate instruction at the beginning of the program before the BeginProg instruction.

In pipeline mode, it takes less time for the datalogger to execute each scan of the program. However, because processing can lag behind measurements, there could be instances, such as when turning on a sensor using the SW12 instruction, that the sensor might not be turned on at the correct time to make the measurement. In such instances, another instruction should be used (for example, PortSet) or the datalogger should be set to run in SequentialMode.

# See Also

[Understanding CRBasic Program Compile Modes](https://www.campbellsci.com/blog/crbasic-program-compile-modes)

[CR1000X manual](https://help.campbellsci.com/CR1000X/Content/shared/Details/PipelineSequential.htm)
