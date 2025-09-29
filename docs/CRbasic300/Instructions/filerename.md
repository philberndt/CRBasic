# FileRename (Rename File)

The FileRename function is used to change the name of a file stored on the datalogger.

## Syntax

FileRename(OldFileName,NewFileName)

In the following example program, data is written to a file namedCR300data.txt once per minute. Hourly, this file is renamed with an incrementing suffix.

```
Public PTemp, Temp, FileNum, CloseStat
Public MyFile As Long, WriteString As String * 35
Dim Time(9)

BeginProg
Scan (1,min,3,0)
PanelTemp (PTemp,4000)
TCDiff (Temp,1,mv34,1,TypeT,PTemp,True,0,4000,1.0,0)

RealTime (Time(1))

WriteString=Time(4)+":"+Time(5)+":"+Time(6)+" The Temperature is "+Temp

MyFile = FileOpen ("CPU:CR300data.txt","a",-1)
FileWrite (MyFile,WriteString,0)
FileWrite (MyFile,CHR(13),0)
FileWrite (MyFile,CHR(10),0)
CloseStat=FileClose (MyFile)

If IfTime( 0,1,hr) Then
FileNum=FileNum+1
FileRename("CPU:CR300data.txt","CPU:CR300data"&FileNum&".txt")
EndIf
NextScan
EndProg
```

## Remarks

The FileRename function returns True if the operation is successful or False if it fails. If a file with the same name already exists, the function will fail.

The FileHandle for the file must be closed ([FileClose](fileclose.md)) before the file can be renamed.

## Parameters

# OldFileName (Original File Name for File Rename)

The name of the file to be renamed. It is a string entered in the format "Device:FileName". If Device = CPU, the file is stored in datalogger memory. (CPU is the only valid device option for this datalogger.)

Type: String

# NewFileName (New File Name for File Rename)

The new name for the file. It is a string entered in the format "Device:FileName". If Device = CPU, the file is stored in datalogger memory. (CPU is the only valid device option for this datalogger.)

Type: String
