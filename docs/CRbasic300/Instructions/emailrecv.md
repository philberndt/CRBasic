# EmailRecv (Email Receive)

The EMailRecv function is used to poll a POP (post office protocol) server for email messages and store the message portion of the email in a string variable. This instruction is not supported in Operating System v.3 or earlier.

## Syntax

_variable_ = EMailRecv(ServerAddr,ToAddr,FromAddr,Subject,Message,Authen,UserName,PassWord,Result,RecvFrom[optional],RecvSubj[optional],RecvDate[optional],TimeOut[optional] )

In the following program, the datalogger will poll an email server every 5 seconds. If an email with the subject of "Datalogger Message" is found, the datalogger will retrieve the email and store the message part of the transmission in a variable, then write the message to a data table (if a poll is successful, but there was no message received, the table is not called. If it were called, an empty string would be written to the table for the Message variable).

The EMailRecv function is run in a slow sequence so that it does not interfere with the main program.

```
'Main program variables
Public Batt, RefTemp, Temp
'declare Email parameter strings (as constants)

'Note: Change Const values to work with user-specific email and POP Server
Const ServerAddr="POPServerDomainNameOrIPAddress:NewPortValue"
Const ToFilter=""
Const FromFilter=""
Const SubjectFilter="Datalogger Message"
Const UserName="myusername"
Const Password="mypassword"

'Message String & Result Variable
Public Result as String * 50
Public Message AS String * 250
Public EmailSuccess As Boolean

DataTable (Msgs,True,-1)
Sample (1,Message,String)
EndTable

BeginProg
Scan (1,Sec,3,0)
Battery (Batt)
PanelTemp (RefTemp,4000)
TCDiff (Temp,1,mv34,1,TypeT,RefTemp,True,0,4000,1.0,0)

NextScan

SlowSequence
Scan(5,sec,1,0)
EmailSuccess =EmailRecv(ServerAddr,ToFilter,FromFilter,SubjectFilter,Message,"Plain",UserName,Password,Result)
'If a message was received, write it to a table
If Message <> "" Then CallTable (Msgs)
NextScan
EndProg
```

## Remarks

The EMailRecv function uses POP3 to query a mail server for messages. One or more filters can be set to limit the email messages the datalogger will retrieve. The function returns True (-1) if the mailbox was accessed successfully, otherwise it returns False (0). If the datalogger successfully retrieves a message from the mailbox, the message is deleted from the email server. The email to be retrieved must be formatted in plain text (not HTML).

The datalogger waits for the instruction to return a result; thus, it can hold up the scan. However, it can be placed in a slow sequence scan. In a slow sequence, it will run in the background and will not hold up the main program waiting for the transaction to be completed.

Each time the POP3 server is queried, the Message string is replaced with either a new message or a null string. Therefore, you will most likely want to save the message to a data table after it is retrieved because it will be overwritten by the next query (see the example program).

Some email servers require the use ofTLS

**Note:** Transport Layer Security. An Internet communication security protocol.

protocol. When an EmailSend or EmailRecv instruction is in a program, TLS is automatically set to true.

The last three parameters are optional. Type them into the CRBasic program directly if they are needed (they are not included in the parameter window). Some practical uses are: send a return email to the sender, calculate the date/time of the email to determine if some other action needs to be taken, or perform some action based on the subject in the email.

AnIPTrace

**Note:** Function associated with IP data transmissions. IP trace information was originally accessed through the CRBasic instruction IPTrace() and stored in a string variable. Files Manager setting is now modified to allow for creation of a file on a datalogger memory drive, such as USR:, to store information in ring memory.

code of 4 in the datalogger's settings table can be used to analyze messages during an email send or receive to troubleshoot errors.

## Parameters

# ServerAddr (Server IP Address)

A string expression containing the IP address (for example, 100.10.200.20 ) or fully-qualified domain name (for example, smtp.server.com ) of the mail server to be used for message delivery. If a different port number is required, follow the server address with a colon and the desired port number (for example, smtp.server.com:587 ).The IP address for a DNS must be set in the datalogger (using Device Configuration Utility) to use a qualified domain name.

Type: String expression

For the EmailRcv instruction, the default port number used is 110.

# "ToAddr" (Email "To" Address)

A filter string that can be used to control the emails the datalogger will retrieve. The string must be enclosed in quotes. Only emails with a To address that match the string are retrieved; all other messages are ignored. If a null string is entered for this parameter, "" , the filter is not used.

Type: String, enclosed in quotes (or "" for null string)

# "FromAddr" (Email "From" Address)

A filter string that can be used to control the emails the datalogger will retrieve. The string must be enclosed in quotes. Only emails with a From address that match the string are retrieved; all other messages are ignored. If a null string is entered for this parameter, "" , the filter is not used.

Type: String, enclosed in quotes (or "" for null string)

# "Subject" (Email Subject Line)

A filter string that can be used to control the emails the datalogger will retrieve. The string must be enclosed in quotes. Only emails with a Subject line that match that string are retrieved; all other messages are ignored. If a null string is entered for this parameter, "" , the filter is not used.

Type: String, enclosed in quotes (or "" for null string)

# Message (Email Message)

A variable formatted as a string into which the incoming message will be written. The variable must be sized large enough to accommodate the incoming message

Type: Variable, formatted as a string

# "Authen" (Email Authentication Type)

A string specifying the type of authentication to be used when logging in to the email server. The datalogger supports APOP, Plain, andTLS

**Note:** Transport Layer Security. An Internet communication security protocol.

authentication. APOP is recommended if it is supported by the mail server. APOP uses theMD5

**Note:** 16 byte checksum of the TCP/IP VTP configuration.

hash function to encrypt the user name and password. Gmail requires TLS.

Type: String, "APOP", "Plain", or "TLS"

# UserName (Account Username)

A string expression specifying the account username required to access to the mail server.

Type: String expression

# Password (Account Password)

A string expression specifying the account password required to access the mail server.

Type: String expression

# Result (Messages Returned)

A variable formatted as a string that holds messages returned by the mail server as a result of the email transmission. If the result is "+OK" then the email server accepted the command. If the result is "-ERR" or "Error" followed by an explanation then some error has occurred.

Type: Variable, formatted as a string

### Optional Parameters

# RecvFrom (Email Header "From" Information)

A string that holds the email header's From information.

Type: Optional Parameter, String

# RecvSubj (Email Header "Subject" Information)

A string that holds the email header's subject information.

Type: Optional Parameter, String

# RecvDate (Email Header Date Information)

A string that holds the email header's date information, in the format ddd, MM/DD/YYYY HH:MM:SS GMT. Where:

- ddd = three-letter abbreviation for the day of week (for example, Mon)

- MM/DD/YYYY = Month, day, year (for example, 12/31/2010)

- HH:MM:SS = Hour, minutes, seconds (for example, 12:00:00)

- GMT = the offset from GMT (for example, -0600)

Type: Optional Parameter, String

# TimeOut (Response Wait Time)

Specifies a time period, in 0.01 seconds, that the datalogger will wait for input after a connection is made, before considering the attempt failed and incrementing Result. The default TimeOut in the absence of this parameter is 7500 (i.e., 75 seconds).

Type: Constant

**NOTE:** The datalogger must have a working Ethernet, Wi-Fi, or PPP/cellular interface for email functions to work.CR300dataloggers are capable of email functionality, such as EmailSend and EmailRecv, when they are capable of accessing a compatible email server via an internal or external Ethernet interface. When using an external serial device to access the email server, the external device must serve as a PPP host. The datalogger must obtain or be assigned a valid IP address, subnet mask, IP gateway, and in nearly all cases a DNS address. In order to resolve a qualified domain name, access to a domain name server is required.
