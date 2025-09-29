# FileCopy (Copy File)

The FileCopy function is used to copy a file from one drive on the datalogger to another.

## Syntax

FileCopy(FromFileName,ToFileName)

In the following program example, FileList is used to store a list of CPU files to an array. FileCopy is then used to copy these files to theUSR (user)drive.

The files stored into the FileList destination variable are stored as CPU:filename.ext. Thus, in the FileCopy instruction, an expression is used to create a filename comprised of "USR:" plus the string that begins five characters in to the FileList destination string (the drive designation uses the first 4 characters of the destination string. The filename begins at the fifth character). Thus, a filename such as CPU:mydata.txt is copied asUSR:mydata.txt.

````
'Declare Variables
Public FileListDest(10) As String * 30, Listtest
Public Flag As Boolean, CopyTest(10) As Boolean
Dim i

BeginProg
SetStatus ("USRDriveSize",35840)
Scan (1,Sec,3,0)
If Flag Then
Listtest=FileList ("CPU",FileListDest(1))
For i = 1 to 10
Copytest(i)=FileCopy(FileListDest(i),"USR:" + FileListDest(i,1,5))
Next i
Flag = False
EndIf
NextScan
EndProg
``` **NOTE:** Note: Strings can be dimensioned only up to 2 dimensions instead of the 3 allowed for other data types. This is because the least significant dimension is actually used as the size of the string. To begin reading or modifying a string at a particular location into the string, enter the location as a third dimension; e.g., String(x,y,n) where n is the desired character. In the example program, 5 indicates starting at the fifth character in theFileListDeststring.


## Remarks


The FileCopy function returns True if the operation is successful or False if it fails. If a file with the same name already exists, the existing file will be overwritten. The FileHandle for the file must be closed ([FileClose](fileclose.md)) before the file can be copied.


## Parameters



# FromFileName


The name of the file to be copied. It is a string entered in the format "Device:FileName". If a Device is not specified, the CPU drive will be assumed. Valid devices are:

| Device | Description |
| --- | --- |
| CPU: | Internal CPU |
| CRD: | External Memory Card |
| USR: | User-Defined Drive |
| USB: | SC115 |

The USR device is an area of memory that can be set up by the user by assigning a value to the datalogger's UsrDriveSize setting in the Status table. This drive must be set to at least 8192 bytes, in 512-byte increments (if the value entered is not a multiple of 512 bytes, the size will be rounded up).


# ToFileName


The destination (drive) and name for the copied file. Like the FromFileName parameter, it is a string entered in the format "Device:FileName". If a Device is not specified, the CPU drive will be assumed.
````
