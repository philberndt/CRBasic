# UserStr and UserVal User Settings

Two types of User Settings can be used to hold values that are persistent between program changes or program restarts. These user settings persist when sending datalogger operating systems, if the OS download is done via the **Connect** window and no other changes in the OS cause all of the settings in the datalogger to revert to the defaults. The user settings include an array of 12 floating point values called`UserVal()`and 12 strings called`UserStr()`. In many instances the PreserveVariables instruction can be used instead of user settings.

Values can be assigned to user settings usingDevice Configuration Utility

**Note:** Configuration tool used to set up dataloggers and peripherals, and to configure PakBus settings before those devices are deployed in the field and/or added to networks.

Settings Editor or using the [SetSetting](setstatussetsetting.md) instruction in the datalogger.

In the following program, if the user toggles a flag, the values from several user settings are written to a table called MetaData. The program shows the user settings being set by the program itself to provide an example of how a setting can be set under program control. However, in practical use you would likely want to set these values using software, since setting these values under program control would mean that they are set back to the original value when the program is compiled.

```
Public Batt_Voltage, IntTemp, Flag(2) As Boolean, Sensor, MetaTableTrig As Boolean
Public DateTime As String
Alias Flag(1) = TriggerCal

DataTable (SensorData,True,-1 )
DataInterval (0,1,Min,10)
Average (1,Sensor,FP2,False)
EndTable

DataTable (MetaData, MetaTableTrig,500 )
DataInterval (0,1,Sec,10)
Sample (1,Settings.UserStr(1),String)'Write Site ID to data file
FieldNames ("Site_ID")
Sample (1,Settings.UserStr(2),String)'site owner
FieldNames ("Site_Owner")
Sample (1,Settings.UserStr(3),String)'last calibration
FieldNames ("Last_Calibration")
Sample (1,Settings.UserVal(2),FP2)'latitude
FieldNames ("Site_Latitude")
Sample (1,Settings.UserVal(3),FP2)'longitude
FieldNames ("Site_Longitude")
Sample (1,Settings.UserVal(1),FP2)'sensor offset
FieldNames ("Sensor_Offset")
EndTable

BeginProg
SetSetting ("UserStr(1)", "SiteID#1")'set Site ID in datalogger
SetSetting ("UserStr(2)", "Owner - John Doe")'station metadata
SetSetting ("UserVal(2)", 41.7378)'Site Latitude
SetSetting ("UserVal(3)", 111.8308)'Site Longitude
SetSetting ("UserVal(1)",-2.5)'set offset for sensor
SetSetting ("UserStr(3)", "")'enter last calibration

Scan (1,Sec,3,0)
Battery (Batt_Voltage)
PanelTemp (IntTemp,4000)
'Measure Sensor
VoltDiff (Sensor,1,mv2500,1,True ,0,4000,1.0,"UserVal(3)")'Offset is based on UserVal(3)

'Set Calibration Date Time for use in Meta Data Table
'and Trigger storage of data to metadata table
If Flag(1) Then'triggercal variable
DateTime=Public.TimeStamp(1,1)
SetSetting("UserStr(3)", DateTime)
MetaTableTrig=True
Flag(1) = False
EndIf

CallTable SensorData
CallTable MetaData
MetaTableTrig=False
NextScan
EndProg
```
