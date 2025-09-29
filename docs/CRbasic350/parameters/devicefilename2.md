# "Device:FileName"

Used to specify the file that contains the sections of code that should be executed. The Device on which the file is stored must be specified and the entire string must be enclosed in quotation marks. Device is CPU(the only valid device option for this datalogger).

The USR device is an area of memory that can be set up by the user by assigning a value to the datalogger UsrDriveSize setting in the Status table. This drive must be set to at least 8192 bytes, in 512-byte increments (if the value entered is not a multiple of 512 bytes, the size will be rounded up).

The Include filename can also be an expression, such as "CPU:" + StationNameSetting + ".CRB", where StationNameSetting inserts the station name of the datalogger.

Type: Variable
