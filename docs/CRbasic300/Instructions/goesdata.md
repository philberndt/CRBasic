# GOESData (Copy Data to GOES)

The GOESData instruction is used to copy data to a GOES satellite data transmitter (that is, TX321, TX320, or TX312)._This instruction is not compatible with the TX325._

## Syntax

GOESData(ResultCode,Table,TableOption,BufferControl,DataFormat)

The following sample program makes a few simple measurements and stores them in the table named Tempdata. All new data from this table is transmitted hourly.

```
'declarations
PUBLIC TCTemp
PUBLIC PanelT
PUBLIC battery1
DIM SATData
DIM SATStatus(13)

'program table
DataTable (Tempdata,1,1000)
DataInterval (0,1,hr,10)
Sample (1,TCTemp,FP2)
Sample (1,PanelT,FP2)
Sample (1,battery1,FP2)
EndTable

BeginProg
Scan (10,Min,3,0)
Battery (battery1)
PanelTemp (PanelT,4000)
TCDiff (TCTemp,1,mv34,1,TypeT,PanelT,True,0,4000,1.8,32)

CallTable TempData
If TimeIntoInterval (0,1,Hr)
GOESData(SATData,TempData,0,0,1)
EndIF
If TimeIntoInterval (0,1,Day)
GOESStatus (SATStatus,1)
EndIF
NextScan
EndProg
```

## Remarks

The GOESData instruction resides within the body of the main program.

## Parameters

# ResultCode (Result Code)

The Variable in which to store the results indicating the success of the program instruction. For the GOESData and GOESSetup instructions, this array is dimensioned to 1. For the GOESStatus instruction, the size of the array required will depend upon the option chosen for the StatusCommand. Right-click the parameter to display a list of defined variables.

| Code | Description                                                                     |
| ---- | ------------------------------------------------------------------------------- |
| 0    | Command executed successfully.                                                  |
| 2    | Timed out waiting for STX character from transmitter after SDC addressing.      |
| 3    | Wrong character received after SDC addressing                                   |
| 4    | Something other than ACK returned when select data buffer command was executed. |
| 5    | Timed out waiting for ACK.                                                      |
| 6    | Port not available; GOES not attached.                                          |
| 7    | ACK not returned following data append or overwrite command.                    |
| -11  | Buffer control error.                                                           |
| -12  | Message window error.                                                           |
| -13  | Channel error.                                                                  |
| -14  | Baud error.                                                                     |
| -15  | RCount error.                                                                   |
| -16  | Illegal data format.                                                            |
| -17  | Data format 0 or 1 was chosen, but table values were not FP2 or ASCII.          |
| -18  | STInterval error.                                                               |
| -19  | STOffset error.                                                                 |
| -20  | RInterval error.                                                                |
| -21  | Platform ID error.                                                              |
| -22  | Transmitter not set up.                                                         |

Type: Variable or Array

# Table

The data table from which record(s) should be transmitted.

Type: Text

# TableOption (Records to Send)

Indicates which records should be sent from the data table. A numeric value is entered. Right-click to display a list.

| Code | Description                                               |
| ---- | --------------------------------------------------------- |
| 0    | Send all records since last execution.                    |
| 1    | Send only the most recent record stored in the table.     |
| x    | Send a specified number of records (enter a value for x). |

If there are fewer records than the number specified, all available records will be sent.

Type: Integer

# BufferControl (Control Buffer)

Used to specify which buffer should be used (random or self-timed) and whether data should be overwritten or appended to the existing data. Data stored in the self-timed buffer is transmitted only during a predetermined time frame. Data is erased from the transmitter's buffer after each transmission.

Data in the random buffer is transmitted immediately after a threshold has been exceeded. The transmission is randomly repeated to insure it is received. Data in the random buffer must be erased using buffer control, code 9, after random transmissions are finished.

A numeric value is entered for this parameter. Right-click to display a list.

| Code | Description                  |
| ---- | ---------------------------- |
| 0    | Append to self-timed buffer. |
| 1    | Overwrite self-timed buffer. |
| 2    | Append to random buffer.     |
| 3    | Overwrite random buffer.     |
| 9    | Clear random buffer          |

The datalogger orders the data from the newest to the oldest. If oldest to newest is needed, execute the instruction in Append mode each time data is written to the data record.

Type: Integer

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

**NOTE:** When the datalogger sends a command, further processing tasks are performed only after a response has been received from the GOES Transmitter.

The [FieldNames](fieldnames.md) instruction can be used to support SHEF PE codes and insert printable characters within the data set.[For more information, seeUsing FieldNames for SHEF PE Codes.](../Info/usingfieldnamesforshefpecodes.md)
