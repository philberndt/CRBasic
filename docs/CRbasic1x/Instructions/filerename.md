# FileRename (Rename File)

The FileRename function is used to change the name of a file stored on the datalogger.

## Syntax

FileRename(OldFileName,NewFileName)

In the following example program, data is written to a file namedCR1000Xdata.txt once per minute. Hourly, this file is renamed with an incrementing suffix.

```
Public PTemp, Temp, FileNum, CloseStat
Public MyFile As Long, WriteString As String * 35
Dim Time(9)

BeginProg
Scan (1,min,3,0)
PanelTemp (PTemp,15000)
TCDiff (Temp,1,mv200C,1,TypeT,PTemp,True,0,15000,1.0,0)

RealTime (Time(1))

WriteString=Time(4)+":"+Time(5)+":"+Time(6)+" The Temperature is "+Temp

MyFile = FileOpen ("CPU:CR1000Xdata.txt","a",-1)
FileWrite (MyFile,WriteString,0)
FileWrite (MyFile,CHR(13),0)
FileWrite (MyFile,CHR(10),0)
CloseStat=FileClose (MyFile)

If IfTime( 0,1,hr) Then
FileNum=FileNum+1
FileRename("CPU:CR1000Xdata.txt","CPU:CR1000Xdata"&FileNum&".txt")
EndIf
NextScan
EndProg
```

## Remarks

The FileRename function returns True if the operation is successful or False if it fails. If the drive location (Device) for the OldFileName and NewFileName are the same and a file with the same name already exists, the function will fail.

If the drive location (Device) for the OldFileName and NewFileName are different, the new file is copied to the NewFileName and the OldFileName is deleted. If a file with the same name exists on the new drive, the operation acts like a file copy and replace (i.e., the existing file is overwritten rather than the operation failing).

The FileHandle for the file must be closed ([FileClose](fileclose.md)) before the file can be renamed.

## Parameters

# OldFileName (Original File Name for File Rename)

The name of the file to be renamed. It is a string entered in the format "Device:FileName". If Device = CPU, the file is stored in datalogger memory. If Device = CRD, the file is stored on a memory card. If Device = USB, the file is stored onthe SC115. If Device = USR, the file is stored in a user-created memory space.

The USR drive is an area of memory that can be set up by the user by assigning a value to the datalogger's UsrDriveSize setting in the Status table. This drive must be set to at least 8192 bytes, in 512 byte increments (if the value entered is not a multiple of 512 bytes, the size will be rounded up). If a Device is not specified, the CPU drive will be assumed.

Type: String

# NewFileName (New File Name for File Rename)

The new name for the file. It is a string entered in the format "Device:FileName". If Device = CPU, the file is stored in datalogger memory. If Device = CRD, the file is stored on a memory card. If Device = USB, the file is stored onthe SC115. If Device = USR, the file is stored in a user-created memory space. If a Device is not specified, the CPU drive will be assumed.

Type: String
