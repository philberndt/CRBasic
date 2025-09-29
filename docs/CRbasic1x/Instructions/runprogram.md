# RunProgram (Run a Program)

The RunProgram instruction is used to run a datalogger program file from the active program file.

## Syntax

RunProgram("Device:FileName",Attrib)

The following program uses RunProgram to run TC-TEMP.cr1x, which is stored on the datalogger's CPU, after 120 scans of the program.

```
Public RefTemp, TCTemp

DataTable (Test,True,-1)
Sample (1,TCTemp,FP2)
EndTable

BeginProg

Scan (1,Sec,3,120)
PanelTemp (RefTemp,15000)
TCDiff (TCTemp,1,mv200C,1,TypeT,RefTemp,True,0,15000,1.0,0)

CallTable (Test)
NextScan

Scan (1,Sec,3,0)
RunProgram("CPU:TCTemp.cr1x",4)
NextScan
EndProg
```

## Parameters

# "Device:FileName" (Device and File Name)

"Device:FileName" is the device and name of the program file that will be executed (RunProgram) or manipulated (FileManage). The Device on which the file is stored must be specified and the entire string must be enclosed in quotation marks.Valid devices are:

| Device | Description          |
| ------ | -------------------- |
| CPU:   | Internal CPU         |
| CRD:   | External Memory Card |
| USR:   | User-Defined Drive   |
| USB:   | SC115                |

The USR device is an area of memory that can be set up by the user by assigning a value to the datalogger's UsrDriveSize setting in the Status table. This drive must be set to at least 8192 bytes, in 512-byte increments (if the value entered is not a multiple of 512 bytes, the size will be rounded up).

Type: Variable string

# Attribute

A numeric code to determine what should happen to the file called by the RunProgram instruction. The Attribute codes are actually a bit field. The codes are as follows:

| Bit   | Decimal | Description     |
| ----- | ------- | --------------- |
| bit 1 | 2       | Run on power up |
| bit 2 | 4       | Run now         |

A program marked as "Run on power up" can be disabled when power is first applied to the datalogger by pressing and holding the DEL key.

Type: Constant
