# FileOpen (Open File)

The FileOpen function is used to open an ASCII text file or a binary file for writing or reading.

## Syntax

_FileHandle_ = FileOpen("FileName","Mode",SeekPoint)

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
OpenFile1 =FileOpen("CPU:.cr300data1.txt","a",-1)
FileWrite(OpenFile1,WriteString,0)
FileWrite(OpenFile1,CHR(13),0)
FileWrite(OpenFile1,CHR(10),0)
CloseStat=FileClose(OpenFile1)
EndIf
NextScan

EndProg
```

## Remarks

The FileOpen function returns a FileHandle, which can then be used by subsequent file read/write functions ([FileWrite](filewrite.md),[FileRead](fileread.md),[FileReadLine](filereadline.md),[FileClose](fileclose.md)). The FileHandle variable must be declared as a Long variable type. The file to be read from or written to can be either an ASCII text file or a binary file. If FileOpen fails, 0 is returned.

With files opened for writing in ASCII text, every line feed character is replaced with a carriage return. When reading ASCII text files, all carriage return characters (CHR(13)) are discarded and line feed characters (CHR(10)) are converted to end of line.

## Parameters

# FileName (File Name)

Used to specify the Device and FileName for the file written to or read from. FileName must be enclosed in quotes. It is entered in the format of "Device:FileName" where Device is CPU(the only valid device option for this datalogger).

Type: Variable

# Mode

Determines whether the file is being read from or written to, and whether the file format is ASCII or binary. Mode must be enclosed in quotes. Right-click the parameter to display a list of the following options:

| Mode  | Description                                                                                                                                                          |
| ----- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| "a"   | Append to ASCII file atEOF **Note:** End of file. (write). Set SeekPoint to -1 to append to end of file, or specify a value to begin writing other than end of file. |
| "ab"  | Append to binary file at EOF (write). Set SeekPoint to -1 to append to end of file, or specify a value to begin writing other than end of file.                      |
| "a+"  | Append to ASCII file at EOF (read/write). Set SeekPoint to -1 to append to end of file, or specify a value to begin writing other than end of file.                  |
| "a+b" | Append to binary file at EOF (read/write). Set SeekPoint to -1 to append to end of file, or specify a value to begin writing other than end of file.                 |
| d     | Opens a connection to a specified directory; used to determine if a drive is available.                                                                              |
| "r"   | Open ASCII file for reading at SeekPoint (read).                                                                                                                     |
| "rb"  | Open binary file for reading at SeekPoint (read).                                                                                                                    |
| "r+"  | Open ASCII file for update at SeekPoint (read/write).                                                                                                                |
| "r+b" | Open binary file for update at SeekPoint (read/write).                                                                                                               |
| "w"   | Open/overwrite ASCII file (write). SeekPoint is not valid; leave at 0.                                                                                               |
| "wb"  | Open/overwrite binary file (write). SeekPoint is not valid; leave at 0.                                                                                              |
| "w+"  | Open/overwrite ASCII file (read/write). SeekPoint is not valid; leave at 0.                                                                                          |
| "w+b" | Open/overwrite binary file (read/write). SeekPoint is not valid; leave at 0.                                                                                         |

**NOTE:** If the file is opened with a mode that specifies ASCII, when a Chr(10) (line feed) is encountered, a Chr(13) (carriage return) is inserted before the line feed.

The [MoveBytes](movebytes.md) instruction should be used to move floats into a string variable if TOB1 binary files are being written.

Type: String Variable

# SeekPoint

Specifies the byte position to begin reading from or writing to when the file is opened. The value is in bytes, and the read or write begins after the specified SeekPoint. For instance, if 100 is entered, the read or write begins at byte 101. If 0 is entered and a file is being written, existing data will be overwritten. If one of the four "a" options is being used to write data, enter -1 to append to the end of the file or enter a value to begin at a specific byte. SeekPoint has no affect with the "w" options, which always begin at byte 0, overwriting the existing data.

If one of the four "a" options is being used to write data, enter -1 to append to the end of the file or enter a value to begin at a specific byte. SeekPoint has no affect with the "w" options, which always begin at byte 0, overwriting the existing data.

Multiple reads or writes (prior to a FileClose for the FileHandle) begin where the previous file operation left off. When a FileClose instruction is executed for the FileHandle, the FileHandle is deleted.

Type: Variable
