# GOESTable (Format Output to GOES)

The GOESTable and [GOESField](goesfield.md) instructions are used within in a DataTable/EndTable declaration to format data output to a TX325 GOES satellite data transmitter. When a table containing GOESTable and GOESField stores a new record, then the datalogger will send the specified data to the satellite transceiver.

## Syntax

GOESTable(Result,Comport,Model,BufferControl,Fields_Scan_Order,Newest_First,Format)

The following program can be used to test the GOESTable and GoesField instructions.

```
'This example demonstrates using GOESTable and GOESField with a TX325 to send
'Self-Timed Data, Random Data, and Force Random Tx.

Const GOES_COMPORT = ComRS232'Can be other comport depending on datalogger
Const DATA_FORMAT = 3'ASCII comma separated values
Const NUMBER_OF_VALUES_TO_TX = 4

Public battery_voltage
Public panel_temperature
Public warning_message As String * 13'Max size string allowed in GOESTable
Public reported_transmitter_id As String
Public st_table_results As String * 400
Public rt_table_results As String * 400
Public tx_now As Boolean
Public force_random_tx_now As Boolean

Dim temp_random_transmission_interval As String

'Hourly data
DataTable (HR_DATA, TRUE, -1)
DataInterval(0, 1, Hr, 4)
Average (1, battery_voltage, IEEE4, False)
Average (1, panel_temperature, IEEE4, False)
EndTable

'Self-timed data
DataTable (ST_DATA, TRUE, -1)
DataInterval(0, 15, Min, 4)
GOESTable(st_table_results,GOES_COMPORT,DATA_FORMAT, 0, TRUE, TRUE, 3)
GOESField(NUMBER_OF_VALUES_TO_TX, 1, 3, 6,"")
Sample (1, battery_voltage, IEEE4)
GOESField(NUMBER_OF_VALUES_TO_TX, 1, 3, 6,"")
Sample (1, panel_temperature, IEEE4)
GOESField(1, 1, 3, 6,"")
Sample (1, HR_DATA.battery_voltage_Avg, IEEE4)'Only send 1 value from hourly table
GOESField(1, 1, 3, 6,"")
Sample (1, HR_DATA.panel_temperature_Avg, IEEE4)'Only send 1 value from hourly table
EndTable

'Random data
DataTable (RT_DATA, tx_now, -1)
GOESTable(rt_table_results, GOES_COMPORT, DATA_FORMAT, tx_now, TRUE, FALSE, 3)
GOESField(1, 1, 1, 13,"")
Sample (1, warning_message, String)
GOESField(1, 1, 3, 4,"")
Sample (1, battery_voltage, FP2)
GOESField(1, 1, 3, 4,"")
Sample (1, panel_temperature, FP2)
Sample (1, reported_transmitter_id, String)'no GOESField in the line prior to this so it is only stored in the logger and not sent via GOES
EndTable

'Force Random TX table
DataTable (FORCE_RANDOM_TX, True, 1000)
GOESTable(rt_table_results, GOES_COMPORT, DATA_FORMAT, 1, TRUE, FALSE, 3)
GOESField(1, 1, 0, 12,"")
Sample (1,"Forced RT: ", String)
GOESField(1, 1, 3, 5,"")
Sample (1, battery_voltage, FP2)
EndTable

SequentialMode

BeginProg
Scan (1, Min, 0, 0)
Battery (battery_voltage)
PanelTemp (panel_temperature,15000)

If ((panel_temperature > 29) OR (tx_now = TRUE)) Then
tx_now = TRUE
warning_message ="ALARM EVENT!"
EndIf

CallTable HR_DATA
CallTable ST_DATA
CallTable RT_DATA

'reset tx_now after we have the data recorded
tx_now = FALSE

If (force_random_tx_now = True) Then
TriggerSequence (1, 0)
EndIf
NextScan

'Force a Random Transmission when needed for testing, etc.
SlowSequence
Do
WaitTriggerSequence
Battery (battery_voltage)
temp_random_transmission_interval = Settings.GoesRTInterval
SetSetting ("GOESRTInterval","00:00:05")
CallTable FORCE_RANDOM_TX
force_random_tx_now = False
Delay (0, 10, Sec)
SetSetting ("GOESRTInterval", temp_random_transmission_interval)
Loop
EndSequence
EndProg
```

## Remarks

The GOESTable instruction resides within the data table declaration.

**NOTE:** There can only be one GOESTable() instruction per self-timed or random table.

The followingtableprovides the maximum data bytes for an assigned time slot duration. Note that if transmitted output is configured (via GOESField instructions) to have a size near to or exceeding the specified limits, there will be automatic truncation of the outputs that get transmitted.

| GOES self-timed-message maximum data bytes and assigned time-slot duration |                                               |                                                |
| -------------------------------------------------------------------------- | --------------------------------------------- | ---------------------------------------------- |
| Assigned time-slot duration (seconds)                                      | GOES 300 bps maximum data per message (bytes) | GOES 1200 bps maximum data per message (bytes) |
| 5                                                                          | 153                                           | 694                                            |
| 10                                                                         | 340                                           | 1444                                           |
| 15                                                                         | 528                                           | 2194                                           |
| 20                                                                         | 715                                           | 2944                                           |
| 25                                                                         | 903                                           | 3694                                           |
| 30                                                                         | 1090                                          | 4444                                           |
| 35                                                                         | 1278                                          | 5194                                           |
| 40                                                                         | 1465                                          | 5944                                           |
| 45                                                                         | 1653                                          | 6694                                           |
| 50                                                                         | 1840                                          | 7444                                           |
| 55                                                                         | 2028                                          | 8194                                           |
| 60                                                                         | 2215                                          | 8944                                           |

## Parameters

# Result

The Result parameter is a string variable that holds either the data to be output in its specified format or a message indicating there are no data to output to the transmitter. If the Result parameter is used to view data that is output to the transmitter, then the string variable must be sized large enough to accommodate the data to be viewed. Following is a list of example messages that may be returned when there are no data output to the transmitter:

Examples of messages when there is no data output to the transmitter:

- No Output

- Write error

- Timed out wating for STX character from transmitter after SDC addressing

- Wrong character received after SDC addressing

- Timed out waiting for ACK or OK

- CS I/O port not available; GOES not attached

- Buffer Control Error

- COM9602 not communicating

- Illegal data format

- Transmitter not set up

Type: Variable of type string

# ComPort (Communication Port)

The ComPort parameter specifies the communication port that will be used for outputting data. When communicating over TCP/IP, use the variable returned by the TCPOpen function as the ComPort for GOESTable. If the PakBusTCPServer setting is being used rather than TCPOpen, use the Route instruction for the ComPort parameter.

| Alphanumeric | Description                        |
| ------------ | ---------------------------------- |
| ComRS232     | RS232 port of the datalogger       |
| ComC1        | Datalogger control terminals 1 & 2 |
| ComC3        | Datalogger control terminals 3 & 4 |
| ComC5        | Datalogger control terminals 5 & 6 |
| ComC7        | Datalogger control terminals 7 & 8 |
| COMSDC7      | Datalogger CS I/O port; SDC7       |
| COMSDC8      | Datalogger CS I/O port; SDC8       |
| COMSDC10     | Datalogger CS I/O port; SDC10      |
| COMSDC11     | Datalogger CS I/O port; SDC11      |

**NOTE:** When using an SDC comport, an SC105 with a female-to-female null modem cable should be used. The SC105 baud rates for both the CS I/O and RS-232 ports should be set to 9600 baud.

Type: Constant or Variable

# Model

The Model parameter indicates the model of satellite data transmitter to be used. Or, entering 0 for the model parameter specifies a No Connection protocol that may be used for a TCP/IP connection or to test output formats with no transmitter connected.

| Code | Description            |
| ---- | ---------------------- |
| 0    | No Connection Protocol |
| 2    | COM9602 (Irridium)     |
| 3    | TX325/TX326            |

Type: Constant

# BufferControl (Buffer to Use)

BufferControl is aBoolean

**Note:** Data type used to represent conditions or hardware that have only two states (true or false) such as flags and control ports.

parameter (0 or non-zero) that specifies which buffer in the transmitter to write to. The parameter may be a constant or a variable expression. If the parameter is a constant, then 0 specifies a self-timed buffer and 1 or non-zero specifies write to the random buffer. If outputting to the self-timed buffer, output is controlled by whether there is a new record or not. To conditionally output to the random buffer, specify the Buffer Control parameter as a variable expression. When the expression evaluates as True (non-zero), output is written to the random buffer. False (0) means do not output.

**NOTE:** The Self-Timed Interval must be at least 5 minutes greater than the Random Interval. Setting the Self-Timed Interval smaller than the Random Interval is atypical, but if done will result in a timing conflict between the two tasks.

| Parameter Type      | Value           | Function                                          |
| ------------------- | --------------- | ------------------------------------------------- |
| Constant            | 0               | Write to self-timed buffer when new record occurs |
| Constant            | 1 (non-zero)    | Write to random buffer when new record occurs     |
| Variable Expression | True (non-zero) | Output right now to random buffer                 |
| Variable Expression | False (0)       | No output                                         |

Type: Constant (always write to either buffer) or Variable as Boolean (conditionally output to random buffer)

# Fields_Scan_Order (Output Table Order)

This aBoolean

**Note:** Data type used to represent conditions or hardware that have only two states (true or false) such as flags and control ports.

parameter that determines how the output table is ordered.

| Code  | Description                                                 |
| ----- | ----------------------------------------------------------- |
| False | Output each record (scan) along the row of the output table |
| True  | Output each field along the row of the output table         |

Type: Constant or Variable as Boolean

# Newest_First (Output Order)

This aBoolean

**Note:** Data type used to represent conditions or hardware that have only two states (true or false) such as flags and control ports.

parameter that determines whether the newest or oldest data are output first.

| Code  | Description              |
| ----- | ------------------------ |
| False | Output oldest data first |
| True  | Output newest data first |

Type: Constant or Variable as Boolean

# Format

The Format parameter is used to define the format of the data sent to the transmitter. A numeric value is entered:

| Code | Description                                                                                                                                                                                                                                                                                                                                                                           |
| ---- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| 0    | Campbell ScientificFP2 **Note:** Two-byte floating-point data type. Default datalogger data type for stored data. While IEEE four-byte floating point is used for variables and internal calculations, FP2 is adequate for most stored data. FP2 provides three or four significant digits of resolution, and requires half the memory as IEEE4. data; 3 bytes per data point         |
| 1    | Campbell ScientificFP2 **Note:** Two-byte floating-point data type. Default datalogger data type for stored data. While IEEE four-byte floating point is used for variables and internal calculations, FP2 is adequate for most stored data. FP2 provides three or four significant digits of resolution, and requires half the memory as IEEE4. data; 7 bytes per data point (ASCII) |
| 2    | ASCII table space                                                                                                                                                                                                                                                                                                                                                                     |
| 3    | ASCII comma                                                                                                                                                                                                                                                                                                                                                                           |
| 4    | Pseudo-binary (PB)                                                                                                                                                                                                                                                                                                                                                                    |
| 6    | Line SHEF (Standard Hydrological Exchange Format)                                                                                                                                                                                                                                                                                                                                     |

Type: Variable or Constant
