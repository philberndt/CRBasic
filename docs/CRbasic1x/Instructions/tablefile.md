# TableFile (Table File)

The TableFile instruction creates a file from a datalogger's data table and writes the file to theCRD (memory card), USR (user-defined drive),or USB (SC115).

**NOTE:** The [CardOut](cardout.md) instruction is an alternative method for writing data to a memory card. Click [here](cardmethods.md) for additional information.

## Syntax

DataTable (TableName, TriggerVariable, Size)

```
TableFile
```

(FileName,Options,MaxFiles,NumRecs/TimeIntoInterval,Interval,Units,OutStat,LastFileName)

_'Output processing instructions_

EndTable

### Example #1

Plug, retrieve data since last plug, pull.

In this example,an SC115is connected to the datalogger to retrieve the data written to the datalogger memory after the last connection. It only retrieves data that was already in datalogger memory when it was connected. To retrieve subsequent data, theSC115must be disconnected then re-connected to the datalogger. Note that in this example, the file name incorporates the system Status.SerialNumber variable, which helps avoid file overwrites when collecting data from multiple dataloggers.

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
PanelTemp (RefTempC,15000)
Battery (BattVolt)
CallTable Test
NextScan
EndProg
```

### Example #2

Enable greater than 2 GB of data from a single table to be stored toa memory card.

In this example, data from the Test table are baled into individual files on thememory cardevery 60 minutes. The MaxFiles parameter is set to -1, so there is no limit set for the maximum number of files that can be written. Rather, new files are written until the storage device is full. Once the device is full, the oldest file on thememory cardwill be deleted prior to writing a new one (i.e., thememory cardmemory will ring).

```
Public RefTempC, BattVolt

DataTable (Test,1,-1)
DataInterval (0,1,Min,0)
TableFile("CRD:Test",64,-1,0,60,Min,0,0) 'option 64 is TOB3 format
Sample (1,RefTempC,FP2)
Minimum (1,BattVolt,FP2,0,False)
EndTable

BeginProg
Scan (10,Sec,3,0)
PanelTemp (RefTempC,15000)
Battery (BattVolt)
CallTable Test
NextScan
EndProg
```

## Remarks

The TableFile instruction must be placed inside a data table declaration for the table you wish to write to file.The values set for the TablefileNumRecsandIntervalparameters depend on the desired data storage mode. For the SC115, two data "plug-and-pull" modes are available: standard and enhanced. With both plug-and-pull modes, data collection is automatically initiated by connecting the SC115 to the datalogger. In standard mode, only the newest data (that is, data written to datalogger memory since the last time the drive was connected) is collected. In enhanced mode, all data stored in the datalogger's memory is collected every time the drive is reconnected.

The standard plug-and-pull mode is enabled by entering 0 for both theNumRecs(Number of Records) and theIntervalparameter.

```
TableFile
```

("USB:"+Status.SerialNumber+"\_FileName",8,-1,0,0,Hr,0,0)'standard mode

Enhanced mode is enabled by entering 0 for theNumRecsparameter and -1 for theIntervalparameter.

```
TableFile
```

("USB:"+Status.SerialNumber+"\_FileName",8,-1,0,-1,Hr,0,0)'enhanced mode

In addition to these plug-and-pull modes, a resident mode is available for the SC115. In the resident mode, the SC115 remains attached to a single datalogger allowing it to be used as resident external memory. The datalogger can be programmed to bale data to the SC115 at regular intervals or at uniform bale sizes. See the SC115 manual for details.

The resulting file can be copied manually to a PC (using File Control or Windows Explorer), copied to an FTP site using the FTPClient function, or sent as an attachment to an SMTP mail server using the EMailRelay function. The value of the OutStat variable can be monitored to determine when a new file has been written, and thus control the transfer of the file via email or FTP. If this functionality is used, the value should be checked after the CallTable instruction that refers to the data table containing the TableFile instruction. Because the file write operation is executed by a separate task within the datalogger, the OutStat and LastFileName variables will not be updated until the next execution of the EndTable instruction after the scan in which the file was written. This results in a one-scan lag of when the file is written and the variables are updated. The effect of this lag can be lessened with a faster scan rate. Note, however, that if the table is called rapidly (for example, 1 second interval), when viewing the variable on a numeric display, you may never see the value update to -1.

Note that when the Interval is true, because the active file is closed, and a new file is subsequently opened, the two files may appear to have the same timestamp when viewed in a directory listing (such as LoggerNet or PC400's File Control). The newly opened file will be assigned a final timestamp when it is closed, and the next new file is opened.

**NOTE:** Data table access syntax such as TableName.FieldName is only available with the open or most recent Tablefile that is created with option 64. Data table access is not accessible with other options of Tablefile.

## Parameters

# FileName (Device and File Name)

Specifies the device to write to and the name of the file to create. The created file will have a suffix of X.dat, where X is a number that will be incremented each time a new file is written.

FileName must be a constant and enclosed in quotes. It is entered in the format of "Device:FileName".The device choices areCRD (memory card), USR (user-defined drive),or USB (SC115).

The USR drive is an area of memory that can be set up by the user by assigning a value to the datalogger's UsrDriveSize setting in the Status table. This drive must be set to at least 8192 bytes, in 512 byte increments (if the value entered is not a multiple of 512 bytes, the size will be rounded up. It may also be rounded up if additional bytes are needed for overhead).

Type: Constant

# Options (Save Options)

Specifies the type of file to be saved and whether or not to include the header information, timestamp, and/or record number. Right-click the parameter for a list of valid options.

The Options parameter is used to specify the type of file to be saved and whether or not to include the header information, timestamp, and/or record number. Options 0, 8, 16, and 32 correspond toCampbell Scientificdefined formats for TOB1, TOA5, CSIXML, and CSIJSON, respectively._ Choosing an option that is different than these defined formats may make the file incompatible with otherCampbell Scientificapplications designed to read or process those files _(**for instance, any option that does not include the Header will make the file unreadable by CardConvert or View Pro**).

Negating the Options parameter for all options except 0 and 64, results in each new record being appended to the file at the time it is written, rather than waiting for the next interval to "bale" the data. This "append" mode may have advantages for long intervals that are vulnerable to loss of data in the event that an interval is missed due to power loss or a program restart. However, append mode is not recommended for "fast" data storage (e.g., intervals less than 5 min). See [Table File Append Mode](tablefileappendmode.md) for more information.

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
| 259  | AmeriFlux, Header                |
| 64   | TOB3 (CRD Drive Only)\*          |

With any of the options above, when a card is removed from the datalogger, any records waiting to be written at the next output interval will be written immediately to the card. If 100 is added to the code above, the records are not written immediately; they will be written at the next output interval during which a card is present.

If the TableFile instruction is writing to a memory card, and the program uses the CardOut instruction as well, then prior to creating the fixed size CardOut tables the required card space will be calculated and reserved for all fixed size TableFile files. Space is reserved by subtracting the estimated space required by the instruction from the available memory on the card (however, space is not preallocated). If the TableFile instruction uses auto-allocation then no space is reserved for its files and the MaxFiles value will be set once the card is full. If both the TableFile and the CardOut instruction attempt to use auto-allocation, a compile error will be returned. With Options 0 through 35, before a memory card is removed (when the removal button is pressed), all TableFiles will be written to the card, regardless of whether the output condition (time interval or fixed number of records) has been met. With Options 100 to 135, any outstanding records are not written to the card immediately; they will be written to the card on the next output interval at which a card is present.

\* Option 64 applies only to files being written to the CRD drive. Additionally, CardOut cannot be used in the same table that is using TableFile to write TOB3 files.

# MaxFiles (Maximum Number of Files)

Used to specify the maximum number of files to retain on the storage device. When the MaxFiles is reached, the oldest file will be deleted prior to writing the new one. If the destination drive is not large enough to accommodate MaxFiles, the datalogger will adjust MaxFiles internally (though the parameter will not be changed in the instruction). If MaxFiles is set to -1, then no limit will be set for the maximum number of files that can be written, until the storage device is full. Once the device is full, the oldest file will be deleted prior to writing the new one. If MaxFiles is set to -2, there is no limit set for the maximum number of files that can be written, but once the storage device is full, no new files will be written. Thus, -1 is analogous to an auto-allocated ring memory mode, and -2 is analogous to an auto-allocated fill and stop mode. If MaxFiles is set to 0, FileName will not be incremented and the file will be overwritten with a new file each time.

Type: Constant

# NumRecs/TimeIntoInterval (Number of Records/Time to Interval)

The function of this parameter depends upon whether or not Interval is a non-zero value. If Interval is set to 0, enter the number of records to include in each file. A new file will not be written until enough records have been written to the datalogger's table to satisfy the NumRec parameter. If Interval is a non-zero value, enter the time into the interval (or offset) that a file should be written. For instance, if Interval is set to 60, Units is set to minutes, and this parameter is set to 15, records will be written at 15 minutes past the hour, each hour.

Type: Constant or Variable

# Interval (Write Options)

Determines whether the instruction will write files based on a specified number of records or on a time interval. If Interval is set to 0, files will be written once a specified number of records is available in the datalogger's data table. If Interval has a non-zero value, files will be written based on a time interval (which is determined by using three parameters: TimeIntoInterval, Interval, and Units).

When using an SC115, if Interval is set to -1, all data or a specified number of records will be written when the media is plugged into the datalogger or when CardFlush is executed. All data is set by using 0 in the NumRecs parameter, and a specified number of records is set by putting the desired number of records in NumRecs.

Type: Constant or Variable

# Units

Specifies the units on which the TimeIntoInterval and Interval parameters will be based. The options are microseconds, milliseconds, seconds, minutes, hours, or days. Right-click the parameter to display a list.

# OutStat (Output Status)

A variable that holds a value indicating whether or not a new file has been stored. If a new file is written when the instruction is executed, a -1 will be stored. If a new file is not written, a 0 will be stored. If this parameter is set to 0 instead of a variable, it will be ignored. Note that this variable is updated one scan behind when the file is written.

Type: Variable

# LastFileName (Name of Last File Written)

A variable that contains the name of the last file written. It must be defined as a string and sized large enough to accommodate the saved file name. If 0 is entered for this parameter, it is ignored. Note that this variable is updated the next time the data table is called.

Type: Variable formatted as a string or 0
