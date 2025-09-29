# FileWrite (Write File)

The FileWrite function performs an ASCII or binary write to an open data file, referenced by a FileHandle. FileWrite behavior is dependent on the mode and seek point used by FileOpen and the source data type.

## Syntax

FileWrite(FileHandle,Source,Length)

In the following program written, the WriteString variable is written to a file when Flag(1) is high. WriteString includes the time, a string with the text "The Temperature is", and the temperature variable. A carriage return and line feed are inserted (CHR(13) and CHR(10)) so that new writes are written on the next line.

To write each file using a unique name, refer to [FilesManager](filesmanager2.md).

```
Public PTemp, Temp
Public OpenFile1 AS Long, WriteString AS String * 35
Public Flag(1),CloseStat
Dim Time(9)

BeginProg

Scan (1,Sec,3,0)
PanelTemp (PTemp,4000)
TCDiff (Temp,1,mv34,1,TypeT,PTemp,True,0,4000,1.0,0)

RealTime (Time(1))

WriteString=Time(4)+":"+Time(5)+":"+Time(6)+" The Temperature is "+Temp

If Flag(1) Then
OpenFile1 =FileOpen("CPU:CR350data1.txt","a",-1)
FileWrite(OpenFile1,WriteString,0)
FileWrite(OpenFile1,CHR(13),0)
FileWrite(OpenFile1,CHR(10),0)
CloseStat=FileClose(OpenFile1)

EndIf
NextScan

EndProg
```

## Remarks

This function writes the data in the Source variable to a FileHandle created by FileOpen. This function returns the number of bytes successfully written to the file.

With files opened for writing in ASCII text, every line feed character is replaced with a carriage return. When reading ASCII text files, all carriage return characters (CHR(13)) are discarded and line feed characters (CHR(10)) are converted to end of line.

## Parameters

# FileHandle (File Handle)

The program reference to the file being read from, written to, or otherwise affected by the function. A FileHandle is created by a FileOpen function and deleted by FileClose. Recommended variable type is Long.

Type: Variable

# Source

The constant, variable, or array of variables to be written to file.

Type: Constant, variable, or variable array

# Length

The maximum number of characters of Source to be written to the file. Length should be equal to or less than the number of bytes in Source. If Length is set to 0, Source should be a string. Note that in this instance, all bytes up to the first null will be written.

Type: Variable

To write each file using a unique name, refer to [FilesManager](filesmanager2.md).
