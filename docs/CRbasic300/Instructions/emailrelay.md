# EmailRelay (Email Relay)

The EmailRelay function securely delivers an email message to one or more email addresses via aCampbell Scientificservice, which is preferred over using a third party service such as Gmail or Yahoo. EmailRelay requires access to public internet andDNS

**Note:** Domain name server. A TCP/IP application protocol.

. **CAUTION:** Email-to-text services provided by cellular carriers such as vtext.com (Verizon), txt.att.net (AT&T), and tmomail.net (T-Mobile) are becoming increasingly unreliable due to stricter filtering and high-volume blocking. AT&T has also announced the complete discontinuation of its Email-to-Text gateway service effective June 17, 2025. Customers using these gateways in conjunction with EmailRelay() for alarm notifications may experience delivery issues. Campbell Scientific recommends using standard email addresses whenever possible to ensure reliable message delivery. Please consult [Alternatives to email-to-text services](../Info/alternatives-to-email-to-text-services.md) for guidance on updating your notification configuration.

## Syntax

- Result \* = EmailRelay(ToAddr,Subject,Message,ServerResponse,Attach[optional],NumRecs/TimeIntoInterval[optional],Interval[optional],IntervalUnits[optional],FileOption[optional],TimeOut[optional] ) **Example 1** In the following example program, an email message is sent when the datalogger panel temperature is greater than 30 degrees. A constant, \_CR_LF, is declared to represent the carrige return and line feed characters, (CHR(13) and CHR(10)), for ease of use in the email message. Case statements are used to convert server-response-error codes into human-readable text.

Using this program, the email message text is similar to:

Warning!

This is a automatic email message from the datalogger station 1010. An alarm condition has been identified. The temperature is 30.50768 degrees C.

Datalogger time is 7/12/202013:52:27.04

```
'Main program variables
Public battery_voltage
Public panel_temperature

'EmailRelay() constants
Const _EMAIL_ATTACHMENT = ""
Const _EMAIL_SUBJECT = "Email Message"
Const _EMAIL_TO_ADDRESS = "some.user@campbellsci.com"
Const _CR_LF = CHR(13) & CHR(10)

'EmailRealy() variables
Public email_message As String * 300
Public email_relay_server_response As String * 100
Public email_relay_results_human_readable As String * 100
Public email_trigger As Boolean
Public email_tx_success

BeginProg
Scan (1,Sec,3,0)
Battery (battery_voltage)
PanelTemp (panel_temperature,4000)
NextScan

SlowSequence
Scan (10,Sec,3,0)
If email_trigger = True Then'Manually set trigger true for testing
email_trigger = False'Reset trigger
email_message = "Warning!" & _CR_LF & _CR_LF
email_message = email_message & "This is a automatic email message from the datalogger station " & Status.StationName & ". "
email_message = email_message & "An alarm condition has been identified." & _CR_LF
email_message = email_message & "The temperature is " & panel_temperature + " degrees C." & _CR_LF
email_message = email_message & "Datalogger time is " & Status.Timestamp

'1st attempt
email_tx_success =EmailRelay(_EMAIL_TO_ADDRESS,_EMAIL_SUBJECT,email_message,email_relay_server_response,_EMAIL_ATTACHMENT)
```

'If EmailRelay was not successful, lets try one more time.
If email_tx_success <> -1 Then
'2nd attempt
email_tx_success =EmailRelay(\_EMAIL_TO_ADDRESS,\_EMAIL_SUBJECT,email_message,email_relay_server_response,\_EMAIL_ATTACHMENT)
EndIf

'Human readable error messages
Select Case email_tx_success
Case -1
email_relay_results_human_readable = "EmailRelay server received the message from the datalogger successfully!"
Case 0
email_relay_results_human_readable = "The connection to the EmailRelay server failed."
Case -2
email_relay_results_human_readable = "Execution of the EmailRelay function did not occur due to lack of records or not enough time."
Case -3
email_relay_results_human_readable = "A connection to the EmailRelay server was made but there was an error in communication."
EndSelect
EndIf
NextScan
EndProg

```
**Example 2**

The following example program demonstrates how to use**EmailRelay**to send a text message(SMS)to a phone on a Verizon network. The email needs to be converted to a text message via an email-to-text message gateway. Most cellular providers have an email-to-text message service for their phone subscribers. Just substitute a 10-digit cell number for 'number for each carrier.

- AT&T: number@txt.att.net

- T-Mobile: number@tmomail.net

- Verizon: number@vtext.com (text-only), number@vzwpix (text + photo)

- Sprint: number@messaging.sprintpcs.com or number@pm.sprint.com

```

'Main program variables
Public PTemp, batt_volt, Alarm As Boolean

'EmailRelay() constants
Const ToAddr As String = "number@vtext.com"'10 digit phone number on Verizon network

'EmailRelay() variables
Public Subject As String _ 50
Public Message As String _ 200
Public EmailSuccess
Public ServerResp As String \* 100

BeginProg
Scan (10,Sec,0,0)
PanelTemp (PTemp,4000)
Battery (batt_volt)
NextScan

SlowSequence'Alarm Messages
Do
If Alarm = True Then
Subject = status.stationname & " Alarm"
Message = "Alarm Triggered " & Status.TimeStamp & CHR(13) & CHR(10)
EmailSuccess =EmailRelay(ToAddr,Subject,Message,ServerResp)
Alarm = False
EndIf
Loop
EndSequence
EndProg

```
**Example 3**

The following example program demonstrates the difference between using a positive or negative streaming interval. The program also demonstrates how to stream a data table, or just one variable from a data table. With a streaming interval of -10, this program will send 10 min of data even if some records have already been sent. If there is a lost connection that is re-established, you will not get multiple attachments because you are only asking for the last 10 minutes of records. With a streaming interval of plus 10, the program will send 10 min of only new records. If there is a lost connection that is reestablished, you may get multiple attachments for records that were not sent previously.

```

'Main program variables
Public Battery_voltage
Public Panel_temperature
Public Counter(2) as Long

'EmailRelay() constants
Const \_Email_Subject_1 = "Table_One Data From Datalogger_1"
Const \_Email_Subject_2 = "Panel Temperature Data From Datalogger_1"
Const \_Email_To_Address = "SomeUser@cambellsci.com"
Const \_CR_LF = CHR(13) & CHR(10)

'EmailRelay() variables
Public Email*message(2) As String * 150
Public Email*relay_server_response(2) As String * 100
Public Email_tx_success(2)

'Data Tables
DataTable (Table_One,True,-1)
Sample (1,Battery_voltage,FP2)
Sample (1,Panel_temperature,FP2)
Sample (1,Counter(1),Long)
EndTable

DataTable (Table_Two,True,-1)
Sample (1,Battery_voltage,FP2)
Sample (1,Panel_temperature,FP2)
Sample (1,Counter(2),Long)
EndTable

BeginProg

Scan (1,Min,3,60)

Email_Message(1)= "This is an automatic email message from the datalogger station " & Status.StationName & ". "
Email_Message(1)= Email_message(1) & "Table_One data are attached."

Email_Message(2)= "This is an automatic email message from the datalogger station " & Status.StationName & ". "
Email_Message(2)= Email_message(2) & "Panel temperature data are attached."

Counter(1) = Counter() +1
Counter(2) = Counter(2) +2

Battery (Battery_Voltage)
PanelTemp (Panel_temperature,4000)

CallTable Table_One
CallTable Table_Two
NextScan

'Interval testing of email streaming
'Placing EmailRelay In SlowSequence within a Do/Loop allows EmailRelay
'to execute whenever the datalogger is not busy with other tasks and
'prevents EmailRelay from holding up the main scan if a result is not
'returned immediately.

SlowSequence
Do

'If the attachment is a table name or a field from a table, put table name
'or tablename.fieldname in quotes. These are constants, not variables.

'-10 interval = send most recent 10 min of records from Table_One, even if
'some have already been sent. If connection is lost, then multiple attachments
'will not be sent upon recovery of connection because we are only asking
'for 10 min interval of most recent records; not all records.
'Note that the server response will be -2: Not enough data stored ,
'until the streaming interval is true.
Email_tx_success(1) =EmailRelay(\_EMAIL_TO_ADDRESS,\_EMAIL_SUBJECT_1,Email_message(1),Email_relay_server_response(1),"Table_One",0,-10,min,8)

'Positive 10 interval = send last 10 min of unsent data; only previously
'unsent records will be sent. If connection is lost, multiple attachments
'of previously unsent records my be sent on reconnection.
'Send only panel temperature from Table_Two; not the entire table.
'Note that the server response will be -2: Not enough data stored , until
'the streaming interval is true.
Email_tx_success(2) =EmailRelay(\_EMAIL_TO_ADDRESS,\_EMAIL_SUBJECT_2,Email_message(2),Email_relay_server_response(2),"Table_Two.Panel_Temperature",0,10,min,8)
Loop

EndProg

```

## Remarks


The EmailRelay function returns -1 if the relay server has received and sent the message, 0 if the connection to the server fails, -2 if execution of the function did not occur (for instance if the time or number of records interval are not met), or -3 if a connection was made but the relay server returns an error.

The email can be sent to one or more recipients and it can include attachments. The ServerResponse variable can be used to monitor the status of the email transmission. Note that with all communication functions, the transmission time will vary as a function of network bandwidth as well as datalogger type.

**NOTE:** When multiple IP interfaces (for example, Ethernet port, CS I/O port, Wi-Fi, PPP) are active, the [IPRoute()](iproute.md) instruction is used to direct outgoing IP traffic.

If a connection to the email relay server is not made after 75 seconds, the instruction will time out. The optional TimeOut parameter can be used to change the default 75 second timeout. The datalogger waits for the instruction to return a result; thus, it can hold up the scan. However, the instruction can be placed in a slow sequence scan. In a slow sequence, EmailRelay will run in the background and will not hold up the main program waiting for the transaction to be completed. The ServerResponse variable can be used to monitor the status of email transmission.

The last several parameters are optional. The optional parameters can be used to send data from a data table directly to a server, without the datalogger first having to write the data to a file.

AnIPTrace

**Note:** Function associated with IP data transmissions. IP trace information was originally accessed through the CRBasic instruction IPTrace() and stored in a string variable. Files Manager setting is now modified to allow for creation of a file on a datalogger memory drive, such as USR:, to store information in ring memory.

code of 4800 in the datalogger's settings table can be used to analyze messages during an email send or receive to troubleshoot errors.

In addition to sending emails, EmailRelay can also send a text message (SMS) to a phone using an email-to-text message gateway, which is provided by most cellular providers.[For more information, seeText Message(SMS) via EmailRelay.](../Info/textmessagesmsviaemailrelay.md)


## Parameters



# Result


The Result variable indicates if execution succeeded (-1), failed (0), did not occur when called (-2), or the server response contains an error message (-3). If using optional parameters to stream data from a data table/field, result is set to -2 if the instruction is called but does not execute because the number of records or time into interval conditions are not met.


# ToAddr (Email "To" Address)


A string expression containing the recipient email address. Multiple recipients are specified as a comma separated list of addresses; for example jack@email.com,jill@email.com .

Type: String expression


# Subject (Message Subject)


A string expression containing the text used in the Subject field of the message.

Type: String expression


# Message (Email or Text Message)


A string expression containing the text used in the body of the email or text message. For messages being sent, variables can be included in the message using CRBasic's standard string syntax (for example, "The temperature is " + TempVar + " degrees C").

Type: String expression


# ServerResponse (Server Response Messages)


A variable formatted as String that will be populated with mail server response messages during email transmission. Look at the content of this variable when monitoring the email send process or diagnosing problems.

Type: Variable formatted as a string


## Optional PARAMETERS



# Attach (File Attachment)


A string expression containing a local file name, comma separated list of local file names, data table name, or data table field name. Local files are generally created by TableFile or aCampbell Scientificcamera. Multiple local file names can be specified as a comma separated list (for example,CPU:file1.dat,CPU:image.jpg ). Make sure to include the file directory along with the file name.The only device choice for this datalogger is CPU.

If streaming data directly from a data table or table field to an email attachment, this parameter should be a constant specifying the table name (for example, TableOne ) or table field name (for example, TableOne.BattV\_Min ) that is used as the data source. Or, to stream all data tables, specify the optional data table parameters required for streaming and specify the data source as an empty string ( ").

If an attachment is not required, specify this parameter as an empty string ( ) and do not specify the optional data table parameters.

Type: Variable formatted as a string


# NumRecs/TimeIntoInterval (Time into Interval to Write the Number of Records)


Used only when streaming data directly from a data table or data table field.

**NOTE:** For more details, see the [Data Streaming document](https://s.campbellsci.com/documents/us/technical-papers/ftp-streaming.pdf).

If Interval is greater than 0, the NumRecs/TimeIntoInterval parameter specifies the time into the interval at which previously unsent records should be written to file on the server.

If Interval is equal to 0, the NumRecs/TimeIntoInterval parameters specifies the number of previously unsent records that will be written to file on the server. If Interval is equal to 0, a negative NumRecs/TimeIntoInterval parameter will specify the number of records that will be written to file on the server each time the function is called.

Type: Constant, optional parameter


# Interval


Used only when streaming data directly from a data table or data table field. If greater than 0, the Interval parameter determines the interval at which previously unsent data will be written to file. If equal to zero, the NumRecs parameter will control when data is written. A negative Interval will cause the datalogger to write the most recent records within this time interval each time the function is called.

| NumRecs/ TimeIntoInterval | Interval | Sent |
| --- | --- | --- |
| 0 | 0 | All new (unsent) data in the data table is sent. |
| >=0 | >0 | Interval s worth of previously unsent records. Only previously unsent data will be sent. If multiple intervals have passed between calls to this function, multiple files will be written to the server and each file will contain interval s worth of records. |
| >0 | 0 | Number of previously unsent records. Only previously unsent data will be sent. If multiples of NumRecs have been recorded between calls to this function, multiple files will be written to the server and each file will contain NumRecs of records. |
| <0 | 0 | Each time this function is called, the most recent records up to NumRecs will be sent. |
| 0 | <0 | Each time this function is called, the most recent records within this time interval will be sent. |

Type: Constant


# Units


Used only when streaming data directly from a data table or data table field. It is used to specify the units on which the TimeIntoInterval and Interval parameters will be based. The options are microseconds, milliseconds, seconds, minutes, hours, or days.

Type: Constant


# FileOption (File Format)


Used only when streaming data directly from a data table or data table field. It specifies the format of the file sent in the email. The file will automatically be appended with an incrementing file number and a .dat file extension. If 1000 is added to the format (for example, 1008), the datalogger will not automatically append the incrementing number or .dat extension to the file. Options 0, 8, 16, and 32 correspond toCampbell Scientific's defined formats for TOB1, TOA5, CSIXML, and CSIJSON, respectively.

| Code | Description |
| --- | --- |
| 0 | TOB1, Header, TimeStamp, Record# |
| 1 | TOB1, Header, TimeStamp |
| 2 | TOB1, Header, Record# |
| 3 | TOB1, Header |
| 4 | TOB1, TimeStamp, Record# |
| 5 | TOB1, TimeStamp |
| 6 | TOB1, Record# |
| 7 | TOB1 |
| 8 | TOA5, Header, TimeStamp, Record# |
| 9 | TOA5, Header, TimeStamp |
| 10 | TOA5, Header, Record# |
| 11 | TOA5, Header |
| 12 | TOA5, TimeStamp, Record# |
| 13 | TOA5, TimeStamp |
| 14 | TOA5, Record# |
| 15 | TOA5 |
| 16 | CSIXML, TimeStamp, Record# |
| 17 | CSIXML, TimeStamp |
| 18 | CSIXML, Record# |
| 19 | CSIXML |
| 32 | CSIJSON, TimeStamp, Record# |
| 33 | CSIJSON, TimeStamp |
| 34 | CSIJSON, Record# |
| 35 | CSIJSON |

Type: Constant, optional parameter


# TimeOut (Response Wait Time)


An optional parameter that specifies how long the datalogger will wait (specified in 0.01 seconds) before considering the attempt to connect to the server failed and returning the appropriate result code. The default TimeOut in the absence of this parameter is 7500 (i.e., 75 seconds). To specify a custom timeout without using the other optional parameters (that is, if not streaming data), use commas as placeholders for the unused parameters. For example,**EmailRelay **( ToAddr, "Subject", "Message", "Attach", Result,,,,,,500). In this example, 6 commas precede 500, which sets the timeout to 5 seconds.

Type: Constant

** WARNING:**The datalogger must have a working Ethernet, Wi-Fi, or PPP/cellular interface for email functions to work. ACR300datalogger is capable of email functionality, such as EmailRelay and EmailRecv, when it is capable of accessing a compatible email server via an internal or external Ethernet interface. When using an external serial device to access the email server, the external device must serve as a PPP host. The datalogger must obtain or be assigned a valid IP address, subnet mask, IP gateway, and in nearly all cases a DNS address. In order to resolve a qualified domain name, access to a domain name server is required.
```
