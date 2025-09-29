# FileTime (File Timestamp)

The FileTime function returns the last modified timestamp of a file.

## Syntax

_Variable_ = FileTime(FileHandle)

In the following example program, the instructions to write a file to the CPU drive are contained in a slow sequence. When processing time is available, the file is written. The FileTime instruction is used in the main program sequence to return the time the file was last saved. This variable is then sampled into a data table with an NSEC data type, which will store the value as an 8-byte timestamp. Note that this data table is saved only when the variable TimeofFile is not a NAN value. A NAN is returned if the FileHandle is still open when the time is read.

This program was written for aCR350, but other dataloggers can use similar code (voltage ranges, channel numbers, or other parameters may need to be changed to reflect the specifications of the datalogger).

```
Public PTemp, Temp, CloseStat, TimeofFile As Long, Flag(1) As Boolean
Public MyFile As Long, WriteString As String * 35
Dim Time(9)

DataTable (Table,TimeofFile<>NAN,-1)
DataInterval (0,1,Sec,10)
Sample (1,TimeofFile,Nsec)
EndTable

BeginProg
Scan (1,Sec,3,0)
PanelTemp (PTemp,4000)
TCDiff (Temp,1,mv34,1,TypeT,PTemp,True,0,4000,1.0,0)

RealTime (Time(1))
WriteString=Time(4)+":"+Time(5)+":"+Time(6)+" The Temperature is "+Temp
TimeofFile=FileTime(MyFile)
CallTable (Table)
NextScan
SlowSequence
Scan (1,Sec,3,0)
If Flag(1) Then
MyFile = FileOpen ("CPU:CR350data.txt","a",-1)
FileWrite (MyFile,WriteString,0)
FileWrite (MyFile,CHR(13),0)
FileWrite (MyFile,CHR(10),0)
CloseStat=FileClose (MyFile)
If Flag(1) = False Then Exit
EndIf
NextScan

EndProg
```

The following example program uses the FileList instruction to return a list of files on the datalogger's CPU drive, and the FileTime instruction to return the timestamp for the files. The filenames and timestamps are stored into a data table (the program runs for only 10 scans).

```
Const Num = 20
Public FileListDest(Num) As String * 50, Time(Num) As Long, i

DataTable (FileTimeTab,True,1000)
Sample (Num,Time(),Nsec)
Sample (Num,FileListDest(),String)
EndTable

BeginProg
Scan (5,Sec,3,10)
FileList ("CPU",FileListDest())
For i = 1 To Num
Time(i) =FileTime(FileListDest(i))
Next i
CallTable (FileTimeTab)
NextScan

EndProg
```

## Remarks

The FileHandle parameter specifies the file for which a timestamp should be returned. The value returned is the last modified timestamp, in seconds since January 1, 1990, with a resolution of 2 seconds. . If FileHandle is a string expression, then it references the location and name of the file. E.g., "Device:FileName" where Device is CPU. (The only valid device option for this datalogger.)Else, FileHandle references the return from the [FileOpen](fileopen.md) function.

If Variable is declared as Long, it can be sampled into a data table using the NSEC data format to return a formatted timestamp (mm/dd/7777 hh:mm:ss). If the function fails it will return -2,147,483,648, which, if sampled using NSEC results in a date of 12/13/1921 20:45:52. The function may fail because the file is opened for writing or the file cannot be found. If FileHandle is formatted as a string, make sure the string is sized large enough to accommodate the entire file name.

## Parameter

# FileHandle (File Handle)

The program reference to the file being read from, written to, or otherwise affected by the function. A FileHandle is created by a FileOpen function and deleted by FileClose. Recommended variable type is Long.

Type: Variable
