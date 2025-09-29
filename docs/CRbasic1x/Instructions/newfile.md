# NewFile (File Status)

The NewFile instruction is used to determine if a file stored on the datalogger has been updated since the instruction was last run.

## Syntax

NewFile(NewFileVar,FileName,NewFileName[optional] )

### Example #1

In the following example program, a PC is used to monitor the FileStatus variable. Initially, the PC sets the FileStatus variable to 1. In the program, if this variable is set to 1, then a port is pulsed to trigger a camera. When the file, a JPG, has been written to the CPU, the variable is set to 0. The PC then knows that the file storing process is complete and it is safe to retrieve the file. The PC can then set the variable back to 1, when it is time to trigger another camera image.

```
SequentialMode

Public FileStatus as Boolean

BeginProg
Scan (1,Sec,3,0)
If FileStatus = 1 Then PulsePort(C1,10)'Trigger Camera
NewFile(FileStatus,"CPU:*.JPG")
Do:Loop Until FileStatus = 0'Wait until file update is complete
'<other measurement instructions>
NextScan
EndProg
```

### Example #2

The following example searches for a program called second_program.CRB on the card. If found, the program is run.

```
Public file_action As Long
Public new_file_name As String

BeginProg
Scan (1,Sec,3,0)
file_action = -2
NewFile(file_action,"CRD:second_program.crb",new_file_name)
If new_file_name ="CRD:second_program.crb"Then
FileManage ("CRD:second_program.crb",4)'run second_program.crb
Else
new_file_name ="File not found on drive"
EndIf
NextScan
EndProg
```

## Remarks

If the file being monitored has a newer timestamp than the last time the instruction was run, the NewFileVar is set to 0. This instruction was developed specifically for applications where a PC is used to trigger file creation in the datalogger (though it may have other applications as well). The PC monitors the NewFile variable to determine when a file has finished being written so it can then be retrieved.

## Parameters

# NewFileVar (NewFile Variable)

The NewFileVar is a variable formatted as Boolean, Long, or Float that will hold the result of the instruction. If a new file has been stored, the variable will be set to 0. If a new file has not been stored, NewFileVar will be set to a non-zero value. If the variable is a Boolean, it is set to -1. If the variable is a Long or Float, each time the instruction is executed and a new file is not detected, it will be incremented by 1, unless the value is 0. (This allows the NewFile value to be set externally by some other means to 1, and then the value is monitored to determine if a file has been written. See the example describing an application for triggering a camera.) **Find File Option** When the NewFileVar is type Long and set to the value of -2, the file timestamps are ignored. The data logger will search for a filename on the drive that matches the name set in the FileName parameter. When a file is found, the name of the file that was detected is returned in the NewFileName parameter. The value of the NewFileVar will return -2 (remain unchanged).

Type: Variable, formatted as Boolean, Long, or Float

# "FileName"

The name of the file on the datalogger to be monitored. FileName must be enclosed in quotes. It is entered in the format of "Device:FileName" where device = CPU,CRD (memory card), USR (user-defined drive),or USB (SC115)

The wildcard characters ? and \* can be used in the FileName.

Type: Quoted Textor Variable of Type String

# NewFileName (Name of the New File)

Optional parameteravailable in Operating System2and later that returns the name of the new file. NewFileName is a variable of type String. If a new file is found, its name will be returned into the NewFileName variable. If no new file is found, a NULL string is written to the variable.

Type: Variable formatted as String
