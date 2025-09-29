# EmailSend (Email Send)

The EmailSend function is used to send an email message to one or more email addresses via anSMTP

**Note:** Simple Mail Transfer Protocol. A TCP/IP application protocol.

server.

**NOTE:** Before using this instruction, or if you are having trouble using this instruction, see the newer [EmailRelay()](emailrelay.md) instruction. EmailRelay() securely delivers email using a service provided by Campbell Scientific that is preferred over third-party services such as Gmail or Yahoo.

## Syntax

- Result \* = EmailSend(ServerAddr,ToAddr,FromAddr,Subject,Message,Attach,UserName,Password,ServerResponse,NumRecs/TimeIntoInterval[optional],Interval[optional],IntervalUnits[optional],FileOption, [optional],TimeOut[optional] )

In the following exampleCR1000Xprogram, an email message is sent when the temperature is greater than 28 degrees. Note that the two "If" statements prior to the alarm condition trigger ensure that only one email is sent when the condition first becomes true (instead of an email being sent each time the instruction is executed while the condition remains true). A constant, CRLF, is declared to represent the carrige return and line feed characters (CHR(13) and CHR(10)) for ease of use in the email message.

**NOTE:** This test program does not account for failures of the EmailSend function. In real-world situations you would likely include logic to send the email again in the event EmailSend failed (for example, EmailSuccess = False).

Using this program, the email message text is similar to:

Warning!

This is a automatic email message from the datalogger station 1010. An alarm condition has been identified. The temperature is 30.50768 degrees C.

Datalogger time is 7/12/202013:52:27.04

This program was written for aCR1000Xdatalogger. If using this example for a different datalogger type, some measurement or instruction parameters may need to be modified to accommodate the capabilities of that datalogger.

```
'Main program variables
Public Batt, RefTemp, Temp

'declare Email parameter strings (as constants), Message String & Result Variable
Const ServerAddr="smtp.gmail.com:587"'gmail requires port 587 to send
Const ToAddr="some.user@campbellsci.com"
Const FromAddr="mydatalogger@gmail.com"
Const Subject="Email Message Test"
Const Attach=""
Const UserName="mydatalogger@gmail.com"
Const Password="MyC00lGmailPa$$w0Rd"
Const CRLF = CHR(13)+CHR(10)

Public Result as String * 50
Public AlarmTrigger As Boolean
Public Message As String * 250
Public EmailSuccess

BeginProg
Scan (1,Sec,3,0)
Battery (Batt)
PanelTemp (RefTemp,15000)
TCDiff (Temp,1,mv200C,1,TypeT,RefTemp,True,0,15000,1.0,0)

NextScan

SlowSequence
Scan(1,sec,1,0)
If AlarmTrigger = False Then
If Temp > 28 THEN AlarmTrigger = True
If AlarmTrigger Then
Message = "Warning!" + CRLF + CRLF
Message = Message + "This is a automatic email message from the datalogger station " + Status.StationName + ". "
Message = Message + "An alarm condition has been identified. "
Message = Message + "The temperature is " + Temp + " degrees C." + CRLF + CRLF + CRLF
Message = Message + "Datalogger time is " + Status.Timestamp
EmailSuccess=EmailSend(ServerAddr,ToAddr,FromAddr,Subject,Message,Attach,UserName,Password,Result)
EndIf
EndIf
If Temp < 28 then AlarmTrigger=False
NextScan
EndProg
```

## Remarks

The EmailSend function returns -1 if successful, 0 if the email transmission fails, or -2 if execution did not occur when the instruction was called. The email can be sent to one or more recipients and it can include attachments. The ServerResponse variable can be used to monitor the status of the email transmission.

If a response back from the email server is not received after 75 seconds from sending the email (or sending the last attachment of the email), the instruction will time out. The datalogger waits for the instruction to return a result; thus, it can hold up the scan. However, it can be placed in a slow sequence scan. In a slow sequence, it will run in the background and will not hold up the main program waiting for the transaction to be completed.

The type of email authorization to be used can be specified by appending a type to the user name. Authorizations supported are CRAM-MD5, PLAIN, STARTTLS, and LOGIN. The syntax is username, followed by a semi-colon, followed by authorization type; for example, MyUserName;PLAIN

The last several parameters are optional. They can be used to send data from a data table directly to a server, without the datalogger first having to write the data to a file.

An IPTrace code of 4 in the datalogger's settings table can be used to analyze messages during an email send or receive to troubleshoot errors.

## Parameters

# Result

The Result variable indicates if execution succeeded (-1) or failed (0), or if execution did not occur when called (-2). If using optional parameters to stream data from a data table/field, result is set to -2 if the instruction is called but does not execute because the number of records or time into interval conditions are not met.

# ServerAddr (Server IP Address)

A string expression containing the IP address (for example, 100.10.200.20 ) or fully-qualified domain name (for example, smtp.server.com ) of the mail server to be used for message delivery. If a different port number is required, follow the server address with a colon and the desired port number (for example, smtp.server.com:587 ).The IP address for a DNS must be set in the datalogger (using Device Configuration Utility) to use a qualified domain name.

Type: String expression

For the EmailSend instruction, the default port number used is 25.

# ToAddr (Email "To" Address)

A string expression containing the recipient email address. Multiple recipients are specified as a comma separated list of addresses; for example jack@email.com,jill@email.com .

Type: String expression

# FromAddr (Email "From" Address)

A string expression containing an email address identifying the author.

Type: String expression

# Subject (Message Subject)

A string expression containing the text used in the Subject field of the message.

Type: String expression

# Message (Email or Text Message)

A string expression containing the text used in the body of the email or text message. For messages being sent, variables can be included in the message using CRBasic's standard string syntax (for example, "The temperature is " + TempVar + " degrees C").

Type: String expression

# Attach (File Attachment)

A string expression containing a local file name, comma separated list of local file names, data table name, or data table field name. Local files are generally created by TableFile or aCampbell Scientificcamera. Multiple local file names can be specified as a comma separated list (for example,USR:file1.dat,USR:image.jpg ). Make sure to include the file directory along with the file name.The directories in the datalogger are CPU (datalogger CPU),CRD (memory card), USR (user-defined drive),and USB (SC115).

If streaming data directly from a data table or table field to an email attachment, this parameter should be a constant specifying the table name (for example, TableOne ) or table field name (for example, TableOne.BattV_Min ) that is used as the data source. Or, to stream all data tables, specify the optional data table parameters required for streaming and specify the data source as an empty string ( ").

If an attachment is not required, specify this parameter as an empty string ( ) and do not specify the optional data table parameters.

Type: Variable formatted as a string

# UserName (Account Username)

A string expression specifying the account username required to access to the mail server.

Type: String expression

For the EmailSend instruction, the UserName parameter is a string expression specifying the account username required to access to the SMTP server. The type of email authorization to use can be specified by appending the specification type to the user name. Options are CRAM-MD5, PLAIN, STARTTLS, and LOGIN; for example, MyUserName;STARTTLS

# Password (Account Password)

A string expression specifying the account password required to access the mail server.

Type: String expression

# ServerResponse (Server Response Messages)

A variable formatted as String that will be populated with mail server response messages during email transmission. Look at the content of this variable when monitoring the email send process or diagnosing problems.

Type: Variable formatted as a string

## Optional Parameters

# NumRecs/TimeIntoInterval (Time into Interval to Write the Number of Records)

Used only when streaming data directly from a data table or data table field.

**NOTE:** For more details, see the [Data Streaming document](https://s.campbellsci.com/documents/us/technical-papers/ftp-streaming.pdf).

If Interval is greater than 0, the NumRecs/TimeIntoInterval parameter specifies the time into the interval at which previously unsent records should be written to file on the server.

If Interval is equal to 0, the NumRecs/TimeIntoInterval parameters specifies the number of previously unsent records that will be written to file on the server. If Interval is equal to 0, a negative NumRecs/TimeIntoInterval parameter will specify the number of records that will be written to file on the server each time the function is called.

Type: Constant, optional parameter

# Interval

Used only when streaming data directly from a data table or data table field. If greater than 0, the Interval parameter determines the interval at which previously unsent data will be written to file. If equal to zero, the NumRecs parameter will control when data is written. A negative Interval will cause the datalogger to write the most recent records within this time interval each time the function is called.

| NumRecs/ TimeIntoInterval | Interval | Sent                                                                                                                                                                                                                                                            |
| ------------------------- | -------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| 0                         | 0        | All new (unsent) data in the data table is sent.                                                                                                                                                                                                                |
| >=0                       | >0       | Interval s worth of previously unsent records. Only previously unsent data will be sent. If multiple intervals have passed between calls to this function, multiple files will be written to the server and each file will contain interval s worth of records. |
| >0                        | 0        | Number of previously unsent records. Only previously unsent data will be sent. If multiples of NumRecs have been recorded between calls to this function, multiple files will be written to the server and each file will contain NumRecs of records.           |
| <0                        | 0        | Each time this function is called, the most recent records up to NumRecs will be sent.                                                                                                                                                                          |
| 0                         | <0       | Each time this function is called, the most recent records within this time interval will be sent.                                                                                                                                                              |

Type: Constant

# Units

Used only when streaming data directly from a data table or data table field. It is used to specify the units on which the TimeIntoInterval and Interval parameters will be based. The options are microseconds, milliseconds, seconds, minutes, hours, or days.

Type: Constant

# FileOption (File Format)

Used only when streaming data directly from a data table or data table field. It specifies the format of the file sent in the email. The file will automatically be appended with an incrementing file number and a .dat file extension. If 1000 is added to the format (for example, 1008), the datalogger will not automatically append the incrementing number or .dat extension to the file. Options 0, 8, 16, and 32 correspond toCampbell Scientific's defined formats for TOB1, TOA5, CSIXML, and CSIJSON, respectively.

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

Type: Constant, optional parameter

# TimeOut (Response Wait Time)

Specifies a time period, in 0.01 seconds, that the datalogger will wait for input after a connection is made, before considering the attempt failed and incrementing Result. The default TimeOut in the absence of this parameter is 7500 (i.e., 75 seconds).

Type: Constant

**WARNING:** The datalogger must be assigned an IP address or be set up for DHCP (datalogger IP address of 0.0.0.0). If a fixed IP address is assigned to the datalogger, a subnet mask and IP gateway address must also be defined. A DNS address must be set if you want to use a qualified domain name instead of the IP address of the mail server.
These settings, which are obtained from your network administrator, are set in the datalogger using Device Configuration Utility.
