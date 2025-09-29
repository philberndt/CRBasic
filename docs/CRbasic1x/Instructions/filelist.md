# FileList (File List)

The FileList function writes a list of file names from the specified device into an array.

## Syntax

_Variable_ = FileList(Device,Dest)

The following example program uses FileList functions to return a list of files from the datalogger CPU, CRD, and USR drives.

```
Public NumFiles(3), Files(3,10) As String * 25

BeginProg
Scan (1,Sec,3,0)
NumFiles(1)=FileList("CPU",Files(1,1))
NumFiles(2)=FileList("CRD",Files(2,1))
NumFiles(3)=FileList("USR",Files(3,1))
NextScan
EndProg
```

The following example program shows how FileList can be used to send files stored on a card as an email attachment. Note that the routine for sending the emails is placed in a slow sequence scan so that other instructions in the program will not be delayed while waiting on the email send to succeed.

```
Public Attach(10) as String * 50,
Public Message As String * 25, Result(10) As String * 50
Public NumFiles, USRFiles(10) As String * 50, ESuccess(10)as Boolean, Flag(2) As Boolean
Dim i

'Constants for Email instruction
Const ServerAddr="mymailserver.com"
Const ToAddr="ToEmail@gmail.com"
Const FromAddr="CR1000X@campbellsci.com"
Const Subject="Email Message Test"
Const UserName="MyUserName"
Const Password="MyPassword"

BeginProg
Scan (1,Sec,3,0)
'Other program instructions
NextScan

SlowSequence
Scan(1,Sec,3,0)
If Flag(1) Then
'Retrieve files stored on card
NumFiles=FileList("CRD",USRFiles())
'Use loop to send each file as an attachment to an email
For i = 1 To NumFiles
Message="File " + i + " attached"
ESuccess(i)=EMailSend(ServerAddr,ToAddr,FromAddr,Subject,Message,Attach(i),UserName,Password,Result(i))
Next i
Flag(1) = False
EndIf
NextScan

EndProg
```

## Remarks

The FileList function writes a list of file names, with path, from the specified device into the Destination array. FileList returns to _Variable_ the number of files written to the array if successful. If the Device does not exist a -1 is returned. If the Destination array cannot be written to, a -2 is returned. Files opened for writing are excluded from the list of files.

## Parameters

# Device

A string that indicates the device that will be queried for files. The Device parameter may be a string literal or a string variable.The options are "CPU" (datalogger CPU), "USR" (user-defined drive), "CRD" (memory card),or "USB" (SC115).

Type: String

# FileListDest (File List Destination)

The Dest parameter is the string variable or string variable array in which the names of the files will be stored. Each element of the array will hold one file name. File names returned included the file path.

The Dest array will not be cleared before being written to. File names will be added to the array starting with the first element indicated by the Dest argument up through the number of file names returned or until the end of the specified array dimension is reached. This allows the same array to be utilized as the destination for multiple instructions. If the desire is to clear the array before using FileList(), consider using the Erase() or Move() instructions to clear the array or fill it with a known value first.

If the Dest argument is a two-dimensional array, the list of files will terminate at the end of the dimension specified. For example, if Dest is a 2x2 two-dimensional array, Dest(1,1) is used as the argument, and there are 4 files in the device directory, only two file names will be returned into elements Dest(1,1) and Dest(1,2).

To query more than one device type for a list of files in a program, Dest should be a two dimensional array, where the most significant array is used for the device type. For example, Dest(2, 10) would allow two FileList functions, FileList( CPU , File(1,1)) and FileList( CRD ,File(2,1)), without the second function overwriting the results of the first. Results from the first FileList function would be stored in FileList(1,1) through FileList(1,n) and results from the second FileList function would be stored in FileList(2,) through FileList(2,n)

Type: Variable array
