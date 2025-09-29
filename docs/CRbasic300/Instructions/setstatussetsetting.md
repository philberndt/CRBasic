# SetStatus, SetSetting (Set Status, Set Setting)

The SetStatus and SetSetting instructions are used to change the value for a field in the datalogger's **Status** or **Settings** table.

## Syntax

SetStatus("FieldName",Value)

SetSetting("FieldName",Value)

The following example program demonstrates using the SetSetting instruction to set the baud rate of COM1 to 115200. In addition, the pakbus address of the datalogger is read from the settings table using Tablename.Fieldname syntax.

```
Public BattVolt, Reftemp, PBA as Long

BeginProg
SetSetting("Baudrate(COM1)",115200)
Scan (1,Sec,3,0)
PBA=Settings.pakbusaddress'read PakBus address from Settings table
Battery (BattVolt)
PanelTemp (RefTemp,4000)
NextScan
EndProg
```

In the following datalogger program, three levels of security are set during program run-time using the SetStatus instruction and variables for the three security levels.

```
'Example 2
Public RefTemp, Batt_Volt
Public SecLev1, SecLev2, SecLev3

BeginProg
SecLev1=111
SecLev2=222
SecLev3=333
Scan (1,Sec,0,0)
SetStatus ("Security(1)",SecLev1)
SetStatus ("Security(2)",SecLev2)
SetStatus ("Security(3)",SecLev3)
PanelTemp (RefTemp,4000)
Battery (Batt_volt)
NextScan
EndProg
```

## Remarks

Most fields in the **[** Status **](StatusTable.md)** table are Read Only. However, some fields, such as StationName, may be set with the SetStatus instruction. Also, error counters in the Status table (for example, WatchdogErrors or SkippedScan) may be reset to **0** for troubleshooting purposes. Status table values may be accessed programmatically using [Tablename.Fieldname](tablenamefieldname.md) syntax. E.g. Variable = Status.fieldname. For example, status fields may be used to monitor [CS CELL2XX-Series](Monitoring%20Cellular.md) diagnostic information and data usage programmatically.

Many of the fields in the [Settings](SettingsTable.md) table may be changed with the SetSetting instruction. A list of setting field names is also available from the datalogger's terminal mode using command "**F**". Note that some settings are Read Only and cannot be set. However, Read Only setting values may be accessed programmatically using [Tablename.Fieldname](tablenamefieldname.md) syntax (for example,`Variable= settings.fieldname`).

The FieldName parameter is the name of the field to be changed; the name must be enclosed in quotes. The Value parameter is the value to which that field should be set. If the value being set is a string (such as StationName), it must be enclosed in quotes. If the value to be set is Long (such as baud rate) quotes are not used. If the field that is being set is defined as aBoolean

**Note:** Data type used to represent conditions or hardware that have only two states (true or false) such as flags and control ports.

, the string "TRUE" will be interpreted as True (-1) and the string "FALSE" will be interpreted as False (0). Any other string variable will flag an error in the compile results. Click here to see the proper syntax for [SetSetting](SettingsTable.md)() with different field types.

Some setting changes force the datalogger program to recompile. Operations that cause the datalogger to recompile its program are those that affect memory usage, such as the allocation ofPakBus

**Note:** PakBus is a proprietary communication protocol developed by Campbell Scientific to facilitate communications between Campbell Scientific devices. Similar in concept to IP (Internet Protocol), PakBus is a packet-switched network protocol with routing capabilities. A registered trademark of Campbell Scientific, Inc.

nodes, changing PPP and IP settings (for example, addresses, user names, passwords), or a full memory reset. If the values to be set are constants and the SetSetting/SetStatus instruction appears between the BeginProg and Scan instructions, the datalogger will queue these changes and recompile only once. If the values to be set are variables, or the instruction appears after the Scan instruction, the change in the value will occur during run time (when the instruction is executed), and multiple program recompiles may occur. Care should be taken to not put the datalogger into an endless loop of rebooting when changing settings. Other instances where the SetSetting/SetStatus will occur during run-time rather than compile time are when the instruction is inside a subroutine, function, data table, dial sequence, modem hang up or comms shutdown sequence, voice sequence, or web page sequence.

Settings that affect memory usage force the datalogger program to recompile, which may cause loss of data. Before changing settings, it is a good practice to collect your data. Examples of settings that force the datalogger program to recompile:

|                                                                                                                                                                                      |                                                                                                                                                                              |
| ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| - IP address - IP default gateway - Subnet mask - PPP interface - PPP dial string - PPP dial response - Baud rate change on control ports - Maximum number of TLS server connections | - PakBus encryption key - PakBus/TCP server port - HTTP service port - FTP service port - PakBus/TCP service port - PakBus/TCP client connections - Communication allocation |

**SetSetting ** can also be used to set the value of a [User Setting](usersettings.md) under program control.

An alternative to the**SetSetting** instruction is to use [Tablename.Fieldname](tablenamefieldname.md) syntax to set setting values. For example, the IP address used for PPP communications could be changed with :`Settings.PPPIPAddr="10.10.10.10"`.

**WARNING:** SetSettings may cause the datalogger to reboot! Take care not to put the datalogger into an endless loop of rebooting when changing settings within a program scan.
