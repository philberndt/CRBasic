# FileClose (Close File)

The FileClose function closes a FileHandle created by [FileOpen](fileopen.md).

## Syntax

FileClose(FileHandle)

In the following program, the WriteString variable is written to a file when Flag(1) is high. WriteString includes the time, a string with the text "The Temperature is", and the temperature variable. A carriage return and line feed are inserted (CHR(13) and CHR(10)), so that new writes are written on the next line.

The SetStatus instruction is used to set up the UserDrive where the file is stored.

To write each file using a unique name, refer to [FilesManager](filesmanager2.md).

```
Public PTemp, Temp
Public OpenFile1 AS Long, WriteString AS String * 35
Public Flag(1),CloseStat
Dim Time(9)

BeginProg

SetStatus ("USRDriveSize", 16384)

Scan (1,Sec,3,0)
PanelTemp (PTemp,15000)
TCDiff (Temp,1,mv200C,1,TypeT,PTemp,True,0,15000,1.0,0)

RealTime (Time(1))

WriteString=Time(4)+":"+Time(5)+":"+Time(6)+" The Temperature is "+Temp

If Flag(1) Then
OpenFile1 =FileOpen("USR:CR1000Xdata1.txt","a",-1)
FileWrite(OpenFile1,WriteString,0)
FileWrite(OpenFile1,CHR(13),0)
FileWrite(OpenFile1,CHR(10),0)
CloseStat=FileClose(OpenFile1)
EndIf
NextScan

EndProg
```

## Remarks

This function returns 0 if successful or -1 if the FileHandle referenced is not valid. FileHandle is the variable that was created by the FileOpen instruction. FileClose will return -1 if the handle specified was not opened by FileOpen or the file was already closed.

Note that this instruction does not reset the original value assigned to FileHandle.

## Parameter

# FileHandle (File Handle)

The program reference to the file being read from, written to, or otherwise affected by the function. A FileHandle is created by a FileOpen function and deleted by FileClose. Recommended variable type is Long.

Type: Variable
