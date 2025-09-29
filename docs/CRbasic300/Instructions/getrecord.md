# GetRecord (Retrieve Record)

The GetRecord instruction retrieves one entire record from a Data table and stores the results in an array.

## Syntax

GetRecord(Dest,TableName,RecsBack,DataFormat[optional] )

In the following example, when the variable TCTemp rises above 30, the GetRecord instruction is executed and the variable Temp30 is set to 1 to trigger data storage of the records to the Data table BackData.

```
'Declare Variables
Public Tref'TC Temp Reference
Public TCTemp'Thermocouple Temp
Public BackRec(2)'Array to hold GetRecord Results
Public Temp30'Test for Trigger of GetRecord & Data Storage
Public TempNot30

'TCTemp DataTable
DataTable (Temp,1,1000)
DataInterval (0,1,Sec,10)'Store data every second
Sample(1,Tref,FP2)
Sample(1,TCTemp,FP2)
EndTable

'BackData DataTable
DataTable( BackData,Temp30,1000)
DataEvent (0,Temp30,TempNot30,0)'Store data when Temp30 = 1
Sample(2,BackRec(),FP2)
EndTable

'Main Program
BeginProg
Scan (1,Sec,3,0)
PanelTemp (TRef,4000)
TCDiff (TCTemp,1,mv34,1,TypeT,TRef,True,200,4000,1.0,0)

'Test TCTemp to see if Trigger for GetRecord and Data Storage should be set
IF TCTemp < 30 Then
TempNot30 = 1
Else
TempNot30 = 0
EndIf
IF TCTemp > 30 THEN
Temp30 = 1
'Back up 3 records in Temp datatable and store the record in "BackRec"
GetRecord(BackRec,Temp,3)
Else
Temp30 = 0
ENDIF
CallTable Temp
CallTable BackData
NextScan
EndProg
```

## Remarks

If a time of minimum or maximum is returned by the GetRecord instruction, the time is reflected in seconds since 1990. A NAN is returned if the datalogger attempts to retrieve a record that does not exist. A record can also be retrieved based on time by entering a negative value in the RecsBack parameter.

Records cannot be retrieved from the Public or TableInfo tables.

## Parameters

# Dest (Destination)

The destination variable array in which to store the fields of the record. The array must be dimensioned large enough to hold all of the fields in the record. However, if the Dest parameter is a single-dimensioned variable formatted as a string, all data values will be stored in the one variable. The data values will be separated by commas and the string will have a <crlf> termination. The record's time stamp will be the first data value. All time stamps and variables declared as strings will be enclosed in quotes. Time stamps will be displayed in International ISO format "yyyy-mm-dd hh:mm:ss" with a fractional seconds if present. Right-click the parameter to display a list of defined variables.

Type: Array

# TableName (Table Name)

The name of the DataTable from which to retrieve the record. Records cannot be retrieved from the Public or TableInfo tables. The TableName can reference a variable name of type String that will hold the name of the table at run-time.

Type: Name

# RecsBack (Records to Search)

The number of records to back up and retrieve data (the most recent record is considered record 1). A negative number can be entered for the RecsBack parameter to specify the time, in seconds since 1990, for the record to be retrieved (refer to the [SecsSince1990](secssince1990.md) example program for this usage)

Type: Constant or Variable

For additional data table access functionality, see [Data Table Access](../Info/datatableaccess.md).

## Optional Parameter

# DataFormat (Data Format)

The DataFormat parameter is used to define the format of the data sent to the transmitter (GOESData) or retrieved from the datalogger (GetRecord). A numeric value is entered:

| Code | Description                                                                                                                                                                                                                                                                                                                                                                    |
| ---- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| 0    | Campbell ScientificFP2 **Note:** Two-byte floating-point data type. Default datalogger data type for stored data. While IEEE four-byte floating point is used for variables and internal calculations, FP2 is adequate for most stored data. FP2 provides three or four significant digits of resolution, and requires half the memory as IEEE4. data; 3 bytes per data point. |
| 1    | Floating point ASCII; 7 bytes per data point.                                                                                                                                                                                                                                                                                                                                  |
| 2    | 18-bit binary integer; 3 bytes per data point, numbers to the right of the decimal are truncated.                                                                                                                                                                                                                                                                              |
| 3    | RAWS7; 7 data points: - 1: Total rainfall in inches, format = xx.xxx - 2: Wind speed MPH, format = xxx - 3: Vector average wind direction in degrees, format = xxx - 4: Air temperature in degrees F, format = xxx - 5: RH percentage, format = xxx - 6: Fuel stick temperature in degrees F, format = xxx - 7: Battery voltage in VDC, format = xx.x                          |
| 4    | Fixed decimal ASCII xxx.x                                                                                                                                                                                                                                                                                                                                                      |
| 5    | Fixed decimal ASCII xx.xx                                                                                                                                                                                                                                                                                                                                                      |
| 6    | Fixed decimal ASCII x.xxx                                                                                                                                                                                                                                                                                                                                                      |
| 7    | Fixed decimal ASCII xxx                                                                                                                                                                                                                                                                                                                                                        |
| 8    | Fixed decimal ASCII xxxxx                                                                                                                                                                                                                                                                                                                                                      |

For DataFormat options 1, 4, 5, 6, 7, and 8, if the data being transmitted is formatted as a string, the datalogger will send the string. For DataFormat options 0, 2, and 3, the datalogger will search for numeric values, convert them to the appropriate format, and send them to the transmitter.

Type: Integer
