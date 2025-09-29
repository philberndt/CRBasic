# RunProgram (Run a Program)

The RunProgram instruction is used to run a datalogger program file from the active program file.

## Syntax

RunProgram("Device:FileName",Attrib)

The following program uses RunProgram to run TC-TEMP.CRB, which is stored on the datalogger's CPU, after 120 scans of the program.

```
Public RefTemp, TCTemp

DataTable (Test,True,-1)
Sample (1,TCTemp,FP2)
EndTable

BeginProg

Scan (1,Sec,3,120)
PanelTemp (RefTemp,4000)
TCDiff (TCTemp,1,mv34,1,TypeT,RefTemp,True,0,4000,1.0,0)

CallTable (Test)
NextScan

Scan (1,Sec,3,0)
RunProgram("CPU:TCTemp.CRB",4)
NextScan
EndProg
```

## Parameters

# "Device:FileName" (Device and File Name)

"Device:FileName" is the device and name of the program file that will be executed (RunProgram) or manipulated (FileManage). The Device on which the file is stored must be specified and the entire string must be enclosed in quotation marks.The only device choice for this datalogger is CPU.

Type: Variable string

# Attribute

A numeric code to determine what should happen to the file called by the RunProgram instruction. The Attribute codes are actually a bit field. The codes are as follows:

| Bit   | Decimal | Description     |
| ----- | ------- | --------------- |
| bit 1 | 2       | Run on power up |
| bit 2 | 4       | Run now         |

A program marked as "Run on power up" can be disabled when power is first applied to the datalogger by pressing and holding the DEL key.

Type: Constant
