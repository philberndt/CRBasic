# FileList (File List)

The FileList function writes a list of file names from the specified device into an array.

## Syntax

_Variable_ = FileList(Device,Dest)

The following example program uses FileList functions to return a list of files from the datalogger CPUdrive.

```
Public NumFiles, Files(10) As String * 25

BeginProg
Scan (1,Sec,3,0)
NumFiles=FileList("CPU",Files(1))

NextScan
EndProg
```

## Remarks

The FileList function writes a list of file names, with path, from the specified device into the Destination array. FileList returns to _Variable_ the number of files written to the array if successful. If the Device does not exist a -1 is returned. If the Destination array cannot be written to, a -2 is returned. Files opened for writing are excluded from the list of files.

## Parameters

# Device

A string that indicates the device that will be queried for files. The Device parameter may be a string literal or a string variable.The device type is CPU (datalogger CPU). (The only valid device option for this datalogger.)

Type: String

# FileListDest (File List Destination)

The Dest parameter is the string variable or string variable array in which the names of the files will be stored. Each element of the array will hold one file name. File names returned included the file path.

The Dest array will not be cleared before being written to. File names will be added to the array starting with the first element indicated by the Dest argument up through the number of file names returned or until the end of the specified array dimension is reached. This allows the same array to be utilized as the destination for multiple instructions. If the desire is to clear the array before using FileList(), consider using the Erase() or Move() instructions to clear the array or fill it with a known value first.

If the Dest argument is a two-dimensional array, the list of files will terminate at the end of the dimension specified. For example, if Dest is a 2x2 two-dimensional array, Dest(1,1) is used as the argument, and there are 4 files in the device directory, only two file names will be returned into elements Dest(1,1) and Dest(1,2).

To query more than one device type for a list of files in a program, Dest should be a two dimensional array, where the most significant array is used for the device type. For example, Dest(2, 10) would allow two FileList functions, FileList( CPU , File(1,1)) and FileList( CRD ,File(2,1)), without the second function overwriting the results of the first. Results from the first FileList function would be stored in FileList(1,1) through FileList(1,n) and results from the second FileList function would be stored in FileList(2,) through FileList(2,n)

Type: Variable array
