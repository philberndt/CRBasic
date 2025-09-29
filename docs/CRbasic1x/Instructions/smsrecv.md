# SMSRecv (SMS Receive)

The SMSRecv function is used to poll a CELL2XX-SERIES cellular modem for an SMS message and store the message in a string variable.

**NOTE:** FOR USE WITH CAMPBELL SCIENTIFIC CELL2XX-SERIES CELLULAR MODEMS ONLY. ALSO REQUIRES A CELLULAR ACCOUNT THAT INCLUDES TEXT MESSAGING CAPABILITIES.

## Syntax

- Result =\*SMSRecv(PhoneNumber,Message,TimeStamp)

The following program can be used to test the SMSRecv function.

```
'Declare Public Variables
Public RefTemp, batt_volt
Public Go_Rcv As Boolean'Boolean variable to toggle test
Public SMS_Recv_Res As Long'Variable to hold SMSRecv response
Public rec_phone As String * 30'Variable to hold message phone number
Public rec_msg As String * 400'Variable to hold message
Public rec_time_stamp As String * 40'Variable to hold timestamp of message

'Define Data Tables.
DataTable (SMS_Rcv,1,500)'Set table size to # of records, or -1 to autoallocate.
Sample (1,rec_time_stamp,String)
Sample (1,rec_phone,String)
Sample (1,rec_msg,String)
Sample (1,SMS_Recv_Res,Long)
EndTable

'Main Program
BeginProg
Scan (15,Sec,0,0)
PanelTemp (RefTemp,15000)
Battery (batt_volt)
CallTable SMS_Rcv
NextScan

SlowSequence
Scan ( 30, Sec, 0, 0 )
If Go_Rcv Then'Set Go_Recv true to test SMSRecv
Do
SMS_Recv_Res =SMSRecv(rec_phone,rec_msg,rec_time_stamp)
If SMS_Recv_Res = -1 Then'SMSRecv returns -1 if successful
CallTable SMS_Rcv'If SMSRecv issuccessful, then call SMS_Rcv table
EndIf
Loop Until SMS_Recv_Res = -1'Keep trying until SMSRecv is successful
EndIf
NextScan
EndSequence

EndProg
```

## Remarks

**NOTE:** Note that SMSRecv requires a cellular account that includes text messaging capabilities. Many cellular accounts do not include text messaging.

The SMSRecv function queries the modem for new SMS messages. The function returns True (-1) if a message is successfully retrieved. If no messages are found, the function returns False (0) and the PhoneNumber, Message and TimeStamp variables will be cleared. If the datalogger successfully retrieves a message, the message will be deleted from the modem.

**NOTE:** SMSRecv has a 10 second timeout. It is recommended to place SMSRecv in a slow sequence scan where it will run in the background and not hold up the main program while the datalogger waits for the transaction to complete.

If SMSRecv encounters an error, an error code is returned. The following error codes are possible:

| Error Code | Description                                                                  |
| ---------- | ---------------------------------------------------------------------------- |
| 0          | No message was found                                                         |
| -1         | Message was successfully retrieved.                                          |
| -2         | Busy (SMSRecv called too often)                                              |
| -3         | Message was read but the datalogger was unable to parse the SMS message.     |
| -4         | The datalogger failed to switch the modem to command mode.                   |
| -5         | Message read was successful but the datalogger failed to delete the message. |
| -6         | No response from the modem.                                                  |
| -7         | Timed out waiting to pause cellular modem task.                              |
| -8         | No internal cellular modem Installed.                                        |
| -10        | Communications failure with the external Cell2XX                             |
| -11        | Receive failure. External Cell2XX is powered down                            |

SMSRecv requires the modem to enter command mode. While the modem is in command mode, cellular IP communication will be paused for a minimum of 2 seconds. When SMSRecv is called in a loop, the first call will change the modem state to command mode and immediate subsequent calls will complete faster because the modem is already in command mode.

## Parameters

# Result_Code

A variable of Type Long or Float that holds the result code. See the list above for a description of possible result codes.

# PhoneNumber (Phone Number)

The phone number associated with the received message.

Type: String Variable

# Message (Email or Text Message)

A string expression containing the text used in the body of the email or text message. For messages being sent, variables can be included in the message using CRBasic's standard string syntax (for example, "The temperature is " + TempVar + " degrees C").

Type: String expression

# TimeStamp (Time Stamp)

A string variable that holds the received message s timestamp in the format ddd,MM/DD/YYYY HH:MM:SS

Where:

- ddd = three-letter abbreviation for the day of week (for example, Mon)

- MM/DD/YYYY = Month, day, year (for example, 12/31/2010)

- HH:MM:SS = Hour, minutes, seconds (for example, 12:00:00)

**Usage Notes**:

- MMS messages are not supported.

- The [SMSRecv](#) function pauses cellular IP communication. To minimize communication interuptions, wait between calls of the SMSRecv function. If SMS messages should have a higher priority than IP communication, then SMSRecv may be called more frequently.

- [TableName.FieldName](tablenamefieldname.md) syntax can be used in CRBasic programs to monitor daily and monthly data usage in kilobytes.[SeeMonitoring Cellular Diagnostics and Data Usage Programmaticallyfor more information.](Monitoring%20Cellular.md)

- Some internal cellular modem settings may be set programmatically using the [SetSetting](setstatussetsetting.md) instruction.[SeeCellular Modem Settingsfor more information.](cellmodemsettings.md)
