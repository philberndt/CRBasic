# SMSSend (SMS Send)

The SMSSend function is used to send SMS messages through a CELL2XX-SERIES cellular modem. Common use cases include threshold alerts, regular device status updates (for example, data logger., battery voltage), and remote event notifications (for example, when a door opens).

**NOTE:** FOR USE WITH CAMPBELL SCIENTIFIC CELL2XX-SERIES CELLULAR MODEMS ONLY. ALSO REQUIRES A CELLULAR ACCOUNT THAT INCLUDES TEXT MESSAGING CAPABILITIES.

**Caution:** CR1000X OS 5.0updated SMSSend() to handle arrays. Upgrading to this OS will require CRBasic programs running old instances of SMSSend() to be updated. SMSSend() is now an instruction rather than a function. That is, the ResultCode is included as a parameter. Previously, SMSSend() was set equal to a variable that would hold the ResultCode. There is a new Swath parameter to specify how many messages to send. The Result, PhoneNumber, and Message parameters can all now be arrays which allows multiple messages to be sent to multiple phone numbers with a single SMSSend() instruction.

## Syntax

SMSSend(ResultCode,Swath,PhoneNumber,Message)

The following program can be used to test the SMSSend function.

```
'CR1000X SeriesDatalogger

'Declare Public Variables
Public PTemp, batt_volt

Public Go_Rcv As Boolean
Public Go_Send As Boolean'Boolean variable to toggle test
Public SMS_Send_Res(3) As Long'Array to hold SMSSend responses
Public snd_phone(3) As String'Array to hold SMS recipient phone numbers
Public snd_msg(3) As String * 50'Variable to hold the SMS message to send
Public send_swath
Public send_total

'Main Program
BeginProg
send_swath = 3'send the same message to 3 different phone numbers
snd_phone(1) ="5555551234"
snd_phone(2) ="5555554321"
snd_phone(3) ="5555556789"
snd_msg(1) = "Test Msg to" & snd_phone(1)
snd_msg(2) = "Test Msg to" & snd_phone(2)
snd_msg(3) = "Test Msg to" & snd_phone(3)

Go_Send = 1

Scan (15,Sec,0,0)
PanelTemp (PTemp,15000)
Battery (batt_volt)
NextScan

SlowSequence
Scan ( 30, Sec, 0, 0 )
If Go_Send Then'Set Go_Send to true to test SMSSend
SMSSend( SMS_Send_Res(),send_swath,snd_phone(),snd_msg )
If SMS_Send_Res = -1 Then'SMSSend returns -1 if successful
send_total = send_total + 1'Increment counter if message send is successful
EndIf
Go_Send = false'Reset Go_Send test flag
EndIf
NextScan
EndSequence

EndProg
```

## Remarks

The SMSSend function generates SMS messages and sends them out on the cellular network. Use cases include alerting on thresholds, sending regular device status updated (e.g., batter voltage) sending remote event notifications (e.g, a door opens).

SMSSend requires the modem to enter command mode. While the modem is in command mode, cellular IP communication will be paused.

**NOTE:** SMSSend can take up to 5 minutes to send one message. It is recommended to place SMSSend in a slow sequence scan where it will run in the background and not hold up the main program while the datalogger waits for the transaction to complete.

## Parameters

# ResultCode

A variable of Type Long or Float that holds the result of the instruction.

| Result Code | Description                                                                                                    |
| ----------- | -------------------------------------------------------------------------------------------------------------- |
| 0           | Timed out waiting for a response from the modem.                                                               |
| -1          | Message successfully sent to the network.                                                                      |
| -3          | Error response from modem.                                                                                     |
| -5          | PhoneNumber or Message parameter is an empty string.                                                           |
| -6          | Message sent to the modem, no confirmation response received from the modem.                                   |
| -7          | Timed out waiting for response from the modem.                                                                 |
| -8          | No internal cellular modem installed or modem is off/disabled                                                  |
| -9          | Datalogger timed out waiting for modem to send the message. Message is in Cell2XX queue and may still be sent. |
| -11         | Send failure. External Cell2XX is powered down                                                                 |
| -12         | Array out of bounds                                                                                            |

Type: Long or Float Variable or Variable Array

# Swath

The number of SMS messages to be sent. The maximum number of messages possible is 60.

Type: Variable, constant, or expression

# PhoneNumber (Phone Number)

A string variable or string array containing the SMS recipient phone number including the country code and area code. The country code for the United States is 1, so the Phone Number is 1+three-digit area code+seven-digit phone number. Do not include dashes or + symbols in the phone number. Enclose the phone number in quotes.

For Example:

Public Tx_Number As String = "14351234567"

Type: Variable or variable array formatted as a string

**NOTE:** If a message needs to be sent to multiple recipients, the size of the`Message()`array must match the size of the`PhoneNumber()`array. Otherwise, the message will be sent only to the first phone number.

# Message

A string variable or string array containing the text used in the body of the text message. For messages being sent, variables can be included in the message using CRBasic's standard string syntax (for example, "The temperature is " + TempVar + " degrees C").

Type: Variable or variable array formatted as a string

**Usage Notes**:

- MMS messages are not supported.

- The SMSSend and [SMSRecv](smsrecv.md) functions pause cellular IP communication. To minimize communication interruptions, wait between calls of the SMSSend/SMSRecv function. If SMS messages should have a higher priority than IP communication, then SMSSend/SMSRecv may be called more frequently.

- [TableName.FieldName](tablenamefieldname.md) syntax can be used in CRBasic programs to monitor daily and monthly data usage in kilobytes.[SeeMonitoring Cellular Diagnostics and Data Usage Programmaticallyfor more information.](Monitoring%20Cellular.md)

- Some internal cellular modem settings may be set programmatically using the [SetSetting](setstatussetsetting.md) instruction.[SeeCellular Modem Settingsfor more information.](cellmodemsettings.md)
