# TableFile (Table File)

The TableFile instruction creates a file from a datalogger's data table and writes the file to theCPU or a USB-C flash drive.

## Syntax

DataTable (TableName, TriggerVariable, Size)

```
TableFile
```

(FileName,Options,MaxFiles,NumRecs/TimeIntoInterval,Interval,Units,OutStat,LastFileName)

_'Output processing instructions_

EndTable

### Example #1

```
'Measurement Variables
Public Bat, RefTemp, TCTemp

'TableFile variables
Public OutStat As Boolean, LastFileName as String * 25

DataTable (Test,True,-1)
DataInterval (0,1,Min,10)
TableFile("CPU:Test",8,-1,0,1,Hr,OutStat,LastFileName)'Store a TOA5 (ASCII) file to the CPU once per hour.
Sample (1,Bat,FP2)
Sample (1,TCTemp,FP2)
EndTable

BeginProg
Scan (1,Sec,3,0)
Battery(bat)
PanelTemp (RefTemp,4000)
TCDiff (TCTemp,1,mv34,1,TypeT,RefTemp,True,0,4000,1.0,0)
CallTable (Test)
NextScan
EndProg
```

### Example #2

In the following program, data is output to a table every five minutes. A TableFile instruction is used within the data table to store records to the CPU drive once per hour. When a new file is written, it is emailed to a recipient under program control.

**NOTE:** As an alternative to this example,[EmailRelay](emailrelay.md) can be used to send data directly from a data table as new data is written to the table, rather than writing a file with TableFile and then sending it.

```
'Measurement Variables
Public Bat, RefTemp, TCTemp

'TableFile variables
Public OutStat As Boolean, LastFileName as String * 25

'Email parameter strings (as constants), Message String & Result Variable

Const ToAddr="yourname@domain.com"
Const Subject="New Data File"
Const Message= "See new data file attached"
Public EmailResult as String * 50

DataTable (Test,True,-1)
DataInterval (0,5,Min,10)
TableFile("CPU:Test",8,-1,0,60,Min,OutStat,LastFileName)
Sample (1,Bat,FP2)
Sample (1,TCTemp,FP2)
EndTable

BeginProg

Scan (1,Sec,3,0)
Battery(bat)
PanelTemp (RefTemp,4000)
TCDiff (TCTemp,1,mv34,1,TypeT,RefTemp,True,0,4000,1.0,0)
CallTable (Test)
'Monitor the OutStat variable,and if a new file has been stored
'email that file
If OutStat Then EmailRelay (ToAddr,Subject,Message,EmailResult,LastFileName)
NextScan

EndProg
```

### Example #3

Plug, retrieve data since last plug, pull.

In this example,a USB-C flash driveis connected to the datalogger to retrieve the data written to the datalogger memory after the last connection. It only retrieves data that was already in datalogger memory when it was connected. To retrieve subsequent data, theUSB drivemust be disconnected then re-connected to the datalogger. Note that in this example, the file name incorporates the system Status.SerialNumber variable, which helps avoid file overwrites when collecting data from multiple dataloggers.

```
'Measurement Variables
Public RefTempC, BattVolt

DataTable (Test,1,-1)
DataInterval (0,60,Min,0)
TableFile("USB:"+Status.SerialNumber+"_Filename",8,-1,0,0,Min,0,0) 'option 8 is ASCII format
Sample (1,RefTempC,FP2)
Minimum (1,BattVolt,FP2,0,False)
EndTable

BeginProg
Scan (10,Sec,3,0)
PanelTemp (RefTempC,4000)
Battery (BattVolt)
CallTable Test
NextScan
EndProg
```

## Remarks

Tablefile may be used to write data files to either the CPU or a USB-C flash drive. The TableFile instruction must be placed inside a data table declaration for the table you wish to write to file. When writing to the CPU, the TableFile instruction writes a file based on a specified number of records or on a time interval. The resulting file is saved with a .dat extension, and can be saved as TOA5, binary, CSIXML, or CSIJSON.

When writing to a USB flash drive, one of two data "plug-and-pull" modes must be used: 1) standard or 2) enhanced. With both plug-and-pull modes, data collection is automatically initiated by connecting the USB flash drive to the datalogger. In standard mode, only the newest data (that is, data written to datalogger memory since the last time the drive was connected) is collected. In enhanced mode, all data stored in the datalogger's memory is collected every time the drive is reconnected.

The standard plug-and-pull mode is enabled by entering 0 for both the NumRecs (Number of Records) and the Interval parameter.

```
TableFile
```

("USB:"+Status.SerialNumber+"\_FileName",8,-1,0,0,Hr,0,0)'standard mode

Enhanced mode is enabled by entering 0 for the NumRecs parameter and -1 for the Interval parameter.

```
TableFile
```

("USB:"+Status.SerialNumber+"\_FileName",8,-1,0,-1,Hr,0,0)'enhanced mode

**NOTE:** File type declared in the`Options`parameter can affect plug-and-pull data collection speed. Data is buffered in the datalogger as binary. While data is collected, the datalogger converts the binary data to the declared data type. TOB1 options are binary and require very little processor overhead to convert from the binary buffer. TOA5, CSIXML, and JSON are ASCII options and are far slower since they consume significant processor overhead to convert to ASCII from the binary buffer. CSIXML is especially slow. The effects on collection time will be particularly noticeable if the datalogger is running a long or complex program. In short, if large files need collection, using the TOB1 format may save considerable time. Consult the LoggerNet manual for options available to convert TOB1 data files on the computer to easier-to-read formats.

**NOTE:** CR350 datalogger programs run in sequential mode. In sequential mode, the TableFile() instruction must finish before continuing to the next instruction, resulting in skipped scans unless the scan interval is long enough to handle all communication, measurement processing, and TableFile() tasks. Sequential mode ignores any scan buffers that may be assigned.

To avoid skipped scans, ensure the scan interval in the datalogger program is long enough to include writing to the USB thumb drive. A good rule of thumb is to add at least 100 ms to the program scan interval for each instance of the Tablefile instruction in order to allow sufficient time to execute and process each Tablefile instruction.

**NOTE:** When writing to files under program control, take care to write infrequently to the CPU to prevent premature failure of serial flash memory. Internal chip manufacturers specify the flash technology used in Campbell Scientific CPU: drives at about 100,000 write/erase cycles. While Campbell Scientific's in-house testing has found the manufacturers' specifications to be very conservative, it is prudent to note the risk associated with repeated file writes via program control.

**NOTE:** Data table access syntax such as TableName.FieldName is not available for TableFile tables.

## Parameters

# FileName (Device and File Name)

Specifies the device to write to and the name of the file to create. The created file will have a suffix of X.dat, where X is a number that will be incremented each time a new file is written.

FileName must be a constant and enclosed in quotes. It is entered in the format of "Device:FileName".The devices supported are CPU or USB (USB-C flash drive).

Type: Constant

# Options (Save Options)

Specifies the type of file to be saved and whether or not to include the header information, timestamp, and/or record number. Right-click the parameter for a list of valid options.

The Options parameter is used to specify the type of file to be saved and whether or not to include the header information, timestamp, and/or record number. Options 0, 8, 16, and 32 correspond toCampbell Scientificdefined formats for TOB1, TOA5, CSIXML, and CSIJSON, respectively._ Choosing an option that is different than these defined formats may make the file incompatible with otherCampbell Scientificapplications designed to read or process those files _(**for instance, any option that does not include the Header will make the file unreadable by CardConvert or View Pro**).

| Code | Description                      |
| ---- | -------------------------------- |
| 0    | TOB1, Header, TimeStamp, Record# |
| 1    | TOB1, Header, TimeStamp          |
| 2    | TOB1, Header, Record#            |
| 3    | TOB1, Header                     |
| 4    | TOB1, TimeStamp, Record#         |
| 5    | TOB1, TimeStamp                  |
| 6    | TOB1, Record#                    |
| 7    | TOB1                             |
| 8    | TOA5, Header, TimeStamp, Record# |
| 9    | TOA5, Header, TimeStamp          |
| 10   | TOA5, Header, Record#            |
| 11   | TOA5, Header                     |
| 12   | TOA5, TimeStamp, Record#         |
| 13   | TOA5, TimeStamp                  |
| 14   | TOA5, Record#                    |
| 15   | TOA5                             |
| 16   | CSIXML, TimeStamp, Record#       |
| 17   | CSIXML, TimeStamp                |
| 18   | CSIXML, Record#                  |
| 19   | CSIXML                           |
| 32   | CSIJSON, TimeStamp, Record#      |
| 33   | CSIJSON, TimeStamp               |
| 34   | CSIJSON, Record#                 |
| 35   | CSIJSON                          |

# MaxFiles (Maximum Number of Files)

Used to specify the maximum number of files to retain on the storage device. When the MaxFiles is reached, the oldest file will be deleted prior to writing the new one. If the destination drive is not large enough to accommodate MaxFiles, the datalogger will adjust MaxFiles internally (though the parameter will not be changed in the instruction). If MaxFiles is set to -1, then no limit will be set for the maximum number of files that can be written, until the storage device is full. Once the device is full, the oldest file will be deleted prior to writing the new one. If MaxFiles is set to -2, there is no limit set for the maximum number of files that can be written, but once the storage device is full, no new files will be written. Thus, -1 is analogous to an auto-allocated ring memory mode, and -2 is analogous to an auto-allocated fill and stop mode. If MaxFiles is set to 0, FileName will not be incremented and the file will be overwritten with a new file each time.

Type: Constant

# NumRecs/TimeIntoInterval (Number of Records/Time to Interval)

The function of this parameter depends upon whether or not Interval is a non-zero value. If Interval is set to 0, enter the number of records to include in each file. A new file will not be written until enough records have been written to the datalogger's table to satisfy the NumRec parameter. If Interval is a non-zero value, enter the time into the interval (or offset) that a file should be written. For instance, if Interval is set to 60, Units is set to minutes, and this parameter is set to 15, records will be written at 15 minutes past the hour, each hour.

Type: Constant or Variable

# Interval (Write Options)

Determines whether the instruction will write files based on a specified number of records or on a time interval. If Interval is set to 0, files will be written once a specified number of records is available in the datalogger's data table. If Interval has a non-zero value, files will be written based on a time interval (which is determined by using three parameters: TimeIntoInterval, Interval, and Units).

Type: Constant or Variable

# Units

Specifies the units on which the TimeIntoInterval and Interval parameters will be based. The options are microseconds, milliseconds, seconds, minutes, hours, or days. Right-click the parameter to display a list.

# OutStat (Output Status)

A variable that holds a value indicating whether or not a new file has been stored. If a new file is written when the instruction is executed, a -1 will be stored. If a new file is not written, a 0 will be stored. If this parameter is set to 0 instead of a variable, it will be ignored. Note that this variable is updated one scan behind when the file is written.

Type: Variable

# LastFileName (Name of Last File Written)

A variable that contains the name of the last file written. It must be defined as a string and sized large enough to accommodate the saved file name. If 0 is entered for this parameter, it is ignored. Note that this variable is updated the next time the data table is called.

Type: Variable formatted as a string or 0
