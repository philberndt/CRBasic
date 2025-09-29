# HTTPPost (Send to the Web)

The HTTPPost function is used to send files or text strings to a URL. This instruction is not supported in Operating System v.3 or earlier.

## Syntax

_Result_ = HTTPPost(URI,Contents,Response,Header,NumRecs/TimeIntoInterval[optional],Interval,IntervalUnits,FileOption, [optional],TimeOut[optional] )

The following example datalogger program uses the HTTPPost function to post a file to a free access server.

This program was written for aCR300, but other dataloggers can use similar code (voltage ranges, channel numbers, or other parameters may need to be changed to reflect the specifications of the datalogger).

```
Public PTemp
Public batt_volt
Public file_handle
Public http_header As String * 100
```

Public http*header_content As String * 100
Public http*post_response As String * 200
Public http_post_tx

'Define Data Tables
DataTable (Test,1,1000)
DataInterval (0,15,Sec,10)
Minimum (1,batt_volt,FP2,0,False)
Sample (1,PTemp,FP2)
EndTable

'Main Program
BeginProg
Scan (1,Sec,0,0)
PanelTemp (PTemp,4000)
Battery (batt_volt)

'post current batt_volt to a url
'note below is just an example of what a URL might look like. It is not a real URL.
Http_header = "X-ApiKey: "'enter unique ApiKey here
Http_header_content = "Content-Type:application/json"
File_handle = FileOpen("CPU:Batt_Volt.csv", "w", -1)
FileWrite (file_handle, "2010-05-20T11:01:43Z," + batt_volt + CHR(10), 0)
FileClose (file_handle)

http_post_tx =HTTPPost("http://api.placeholder.com/v2/feeds/12345/datastreams/Batt_volt/datapoints/", "CPU:Batt_Volt.csv", http_post_response, http_header + CHR(13)+ CHR(10) + http_header_content)

CallTable Test
NextScan
EndProg

Example 2

```
The following example program can be used to test server settings and payload formats when using HTTPPost and HTTPGet. The streaming option for HTTPPost is also demonstrated. There are several free online resources available for HTTP endpoint testing.

```

' Constants

```
'----------------------------
```

Const MAINSCANRATE = 1
Const SERVER_URI = "https://xxxxxxxxxxxxxx.m.pipetest.net"'Enter your URI here
Const HEADER = "Content-Type: text/plain"

' Public Variables
'----------------------------
Public PTemp
Public Batt*Volt
Public Text_to_Send As String \_30 = "Hello World"
Public Data_Record As String * 100
Public Go As Boolean
Public PostResult As Long
Public Get As Boolean
Public GetResult As Long
Public Manual_Stream As Boolean
Public Manual_S_Result As Long
Public Stream As Boolean
Public S_Result As Long
Public Response As String \*500

DataTable (Test,1,-1)
DataInterval (0,15,Sec,10)
Minimum (1,Batt_Volt,FP2,False,False)
Sample (1,PTemp,FP2)
EndTable

' Main Program
BeginProg
Scan (MAINSCANRATE,Sec,0,0)
PanelTemp (PTemp,50)
Battery (Batt_Volt)
CallTable Test
NextScan

SlowSequence
'Manually set Go = true to test HTTPPost; view results in online test bin.
Scan(1,Sec,0,0)
If Go = True Then
PostResult =HTTPPost(SERVER_URI,Text_to_Send,Response,HEADER)
Go = False
EndIf
'Manually set Get = True to test HTTPGet; view results in online test bin.
If Get = True Then
GetResult =HTTPGet(SERVER_URI,Response,HEADER)
Get = False
EndIf
'Manually set Stream = True to test auto-streaming of data from the Test data table each time a new record is written.
If Stream = True Then
S_Result =HTTPPost(SERVER_URI,"Test",Response,"",0,0,Sec,32)
Stream = False
EndIf
'Set Manual_Stream = true to test sending only one table record at a time based on a user-configured trigger.
If Manual_Stream = True Then
GetRecord (Data_Record,Test,1)
Manual_S_Result =HTTPPost(SERVER_URI,Data_Record,Response,HEADER)
Manual_Stream = False
EndIf
NextScan
EndProg

```

## Remarks


This function returns the TCP socket that was created to communicate with the HTTP server. This function returns 0 if it fails or -2 if execution did not occur when the instruction was called (for instance, when using the optional parameters to stream data from a table and the number of records or time into interval conditions are not met). If this value is zero when the function is called, the datalogger will create a new connection to the web server.

**NOTE:** When multiple IP interfaces (for example, Ethernet port, CS I/O port, Wi-Fi, PPP) are active, the [IPRoute()](iproute.md) instruction is used to direct outgoing IP traffic.

You may also consider [HTTPPut](httpput.md). The difference between HTTPPost and HTTPPut is that HTTPPost prefaces the content with "POST /" and HTTPPut prefaces the content with "Put /".

[EmailRelay()](emailrelay.md) or [FTPClient()](ftpclient.md) may also be used for data streaming if a server is not configured for HTTP.

HTTPost supports HTTP and HTTPS. When this function is in the program, TLS is enabled in the datalogger automatically, unless the URI is a constant and the first 5 characters are **HTTP:**.

If the header in the response from the server specifies Connection: close , the connection is closed; otherwise, it is left open. If the connection is left open, this same connection is used when the instance of HTTPPost that opened the connection is executed again.

[SemaphoreGet/SemaphoreRelease](semaphoregetsemaphorerelease.md)- Semaphore number 4 can be used by the CRBasic program to determine if a file is currently opened for rename via FTP or for sending/serving up via HTTP. SemaphoreGet(4) should only be used if the desire is to manage CRBasic processes around these type of events.

An example application is a datalogger generating a data file or image that is also served up via HTTP. SemaphoreGet(4) / SemaphoreRelease(4) can be used around the data file generation so that the datalogger does not change the contents of the file in the middle of it being downloaded over HTTP.

The last several parameters are optional. They can be used to send data from a data table directly to a server, without the datalogger first having to write the data to a file.


## Parameters



# URI (Uniform Resource Identifier)


A string that contains the uniform resource identifier (typically a URL) of the HTTP server to be accessed. A username and password can be passed into a URL using http://username:password@http.server.address.

Type: String or variable formatted as a string


# Contents


A string expression that contains the contents to be sent with the request. If streaming data directly from a data table or data table field, this parameter should be a constant specifying the table name (for example, TableOne ) or table field name (for example, TableOne.BattV\_Min ) that will be used as the data source.

Type: String


# Response


Indicates where the results of the request will be written. The parameter can be a variable or the name of a valid file on the datalogger.

Type: String or variable formatted as a string


# Header


A string that indicates the additional header information to include in the request. As a result of the request, the server may return header information that is stored in the string. The Header parameter can be an empty string if no additional header information is required.

In the case that more than one header is required, headers can be separated by & CHR(13) & CHR(10) &. For example:

HTTPPost(https://api.placeholder.com/v1/chat/completions,Query\_str,Response,"Content-Type: application/json"**& CHR(13) & CHR(10) &**"Authorization: Bearer xx-XXXXXX")

Type: String or variable formatted as a string

- Text (description)


## Optional Parameters


** NOTE:**Before using this instruction for data streaming, or if you are having trouble using this instruction, see [HTTPPut()](httpput.md),[EmailRelay()](emailrelay.md), or [FTPClient()](ftpclient.md), which may also be used to stream data.


# NumRecs/TimeIntoInterval (Time into Interval to Write the Number of Records)


Used only when streaming data directly from a data table or data table field.

** NOTE:**For more details, see the [Data Streaming document](https://s.campbellsci.com/documents/us/technical-papers/ftp-streaming.pdf).

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


Used only when streaming data directly from a data table or data table field. It specifies the format used when writing data to the server. The file created on the server will automatically be appended with an incrementing file number and a .dat file extension. If 1000 is added to the format (for example, 1008), the datalogger will not automatically append the incrementing number or .dat extension to the uploaded file. Options 0, 8, 16, and 32 correspond toCampbell Scientific's defined formats for TOB1, TOA5, CSIXML, and CSIJSON, respectively.

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


Specifies a time period, in 0.01 seconds, that the datalogger will wait for input after a connection is made, before considering the attempt failed and incrementing Result. The default TimeOut in the absence of this parameter is 7500 (i.e., 75 seconds).

Type: Constant
```
