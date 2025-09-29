# FileReadLine (Read Line in File)

The FileReadLine function reads a line in a file referenced by a FileHandle and stores the result in a variable or variable array.

## Syntax

FileReadLine(FileHandle,Destination,Length)

The following program shows the use of FileRead and FileReadLine in a program.

```
Public FileHandle1, ReadFile1, ReadDest1 As String
Public FileHandle2, ReadFile2, ReadDest2 As String

DataTable (ReadDat1,True,-1)
Sample (1,ReadDest1,String)
EndTable

DataTable (ReadDat2,True,-1)
Sample (1,ReadDest2,String)
EndTable

BeginProg

Scan (1,Sec,3,1)

'FileRead example
FileHandle1=FileOpen ("CPU:read.txt","r",0)
ReadFile1=FileRead(FileHandle1,ReadDest1,4)
FileClose (FileHandle1)
CallTable (ReadDat1)

'FileReadLine example
FileHandle2=FileOpen ("CPU:read.txt","r",0)
Do
ReadFile2=FileReadLine(FileHandle2,ReadDest2,10)
CallTable (ReadDat2)
Loop Until ReadFile2=-1
FileClose (FileHandle2)

NextScan

EndProg
```

## Remarks

The FileReadLine function reads to the end of a line (as indicated by a carriage return or line feed) or until the maximum number of bytes is reached (specified by Length). The FileReadLine function returns the number of bytes successfully read, -1 if the end of the file is reached, or 0 if the file handle is not good (for example, if the file to read from is not there). To read multiple lines or an entire file, use [FileRead](fileread.md).

With files opened for writing in ASCII text, every line feed character is replaced with a carriage return. When reading ASCII text files, all carriage return characters (CHR(13)) are discarded and line feed characters (CHR(10)) are converted to end of line. FileReadLine is terminated after the end of line is encountered.

## Parameters

# FileHandle (File Handle)

The program reference to the file being read from, written to, or otherwise affected by the function. A FileHandle is created by a FileOpen function and deleted by FileClose. Recommended variable type is Long.

Type: Variable

# Destination

The variable in which the results of the read should be stored.

Type: Variable declared as a string

# Length

Specifies the maximum number of characters to be read in to the Destination variable. If Destination is an array, Length must equal to at least the total of the number of bytes for all elements in the array. For example, if you are reading 3 elements of an array and each element is 4 bytes, Length must be at least 12.

Type: Variable
