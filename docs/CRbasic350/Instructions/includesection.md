# IncludeSection

IncludeSection() reduces the number of Include files needed by allowing the declaration of program sections that can be defined in one Include file.

## Syntax

IncludeSection("SectionName","Device:Filename")

Following is an example of using two IncludeSection() instructions to specify two sections of program code named Constants and Variables. These sections of code are located in an Include file named File_Lib.crb. Without IncludeSection(), two separate Include files would be required to define these two sections of code.

### Main Program

```
ConstTable(Const_Table)
IncludeSection("Constants","CPU:File_Lib.crb")
EndConstTable

IncludeSection("Variables","CPU:File_Lib.crb")

DataTable (Test,1,-1)
DataInterval (0,15,Sec,10)
Minimum (1,batt_volt,FP2,False,False)
Sample (1,PTemp,FP2)
EndTable

BeginProg
Scan (1,Sec,0,0)
PanelTemp (PTemp,15000)
Battery (Batt_volt)
CallTable Test
NextScan
EndProg
```

### File_Lib.crb

```
#If Section ="Constants"Then
Const ExampleConst = 100
#EndIf

#If Section ="Variables"Then
Public PTemp, Batt_volt
#EndIf
```

## Remarks

IncludeSection is commonly used to stitch together libraries or sections of code located in [Include](include.md) files. This approach is commonly used for systems with many components and measurements that require large programs. These system programs often use multiple Include files for specific system-component configurations. IncludeSection() reduces the number of Include files needed by allowing the declaration of program sections that can be defined in one Include file.

**NOTE:** Beginning with OS2.0,, the datalogger can extract a main CRBasic program and several include files packaged in a web.obj.gz file. See [web.obj.gz files](../Info/web_obj_gz_files.md) for details.

## Parameters

# "SectionName"

The name of the section of code to be included in the main program. The name must be enclosed in quotation marks.

# "Device:FileName"

Used to specify the file that contains the sections of code that should be executed. The Device on which the file is stored must be specified and the entire string must be enclosed in quotation marks. Device is CPU(the only valid device option for this datalogger).

The USR device is an area of memory that can be set up by the user by assigning a value to the datalogger UsrDriveSize setting in the Status table. This drive must be set to at least 8192 bytes, in 512-byte increments (if the value entered is not a multiple of 512 bytes, the size will be rounded up).

The Include filename can also be an expression, such as "CPU:" + StationNameSetting + ".CRB", where StationNameSetting inserts the station name of the datalogger.

Type: Variable
