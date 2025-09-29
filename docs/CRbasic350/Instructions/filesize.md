# FileSize (Size of File)

The FileSize function returns the size of a file stored on the datalogger.

## Syntax

FileSize(FileHandle)

OR

_Variable_ = FileSize(FileHandle)

In the following example program, FileSize() is used to compare the size of a file named picture.jpg to file named picture2.jpg. If picture.jpg is bigger than picture2.jpg, then picture.jpg is deleted with the FileManage instruction.

```
Public PTemp

BeginProg

Scan (1,Sec,3,0)
PanelTemp (PTemp,4000)
IfFileSize("CPU:picture.jpg") >FileSize("CPU:picture2.jpg") Then
FileManage("CPU:picture.jpg",8)
EndIf
NextScan
EndProg
```

In the following program, FileSize() is used to return the size of a file named picture.jpg into a variable named TestFileSize

```
Public PTemp, TestFileSize as Long

BeginProg

Scan (1,Sec,3,0)
PanelTemp (PTemp,4000)
TestFileSize =FileSize("CPU:picture.jpg")
NextScan
EndProg
```

In the following program, a file named TestFile.txt is created on the USR drive by FileOpen. The FileSize function is used to determine the size of the file, which is stored in the variable TestFileSize.

```
Public PTemp, TestFileSize as Long, TestFileHandle as Long

BeginProg

Scan (2,Sec,3,0)
PanelTemp (PTemp,4000)
TestFileHandle=FileOpen ("CPU:TestFile.txt","a+",-1)
FileWrite (TestFileHandle,PTemp,0)
TestFileSize=FileSize(TestFileHandle)
FileClose (TestFileHandle)
NextScan
EndProg
```

## Remarks

The FileHandle parameter specifies the file for which the size should be returned. If FileHandle is a string expression, then it references the location and name of the file. E.g., "Device:FileName" where Device is CPU. Else, FileHandle references the return from the [FileOpen](fileopen.md) function.

If the FileSize function fails it will return 0. The function may fail because the file is opened for writing or the file cannot be found.

If FileClose is used to close the file, FileSize must appear prior to FileClose. Once FileClose is executed, the FileHandle no longer exists.

## Parameter

# FileHandle (File Handle)

The program reference to the file being read from, written to, or otherwise affected by the function. A FileHandle is created by a FileOpen function and deleted by FileClose. Recommended variable type is Long.

Type: Variable
