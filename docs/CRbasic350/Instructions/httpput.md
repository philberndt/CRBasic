# HTTPPut (Send to Web)

The HTTPPut function is used to send files or text strings to a URL.

## Syntax

_Result_ = HTTPPut(URI,Contents,Response,Header,NumRecs/TimeIntoInterval[optional],Interval[optional],IntervalUnits[optional],FileOption, [optional],TimeOut[optional] )

### Example #1

The following example datalogger program uses the HTTPPut function to post a file to an example server.

```
Public PTemp
Public batt_volt
Public file_handle
Public http_header As String * 100
Public http_put_response As String * 200
Public http_put_tx

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

http_header = ""'Indicates additional header information if required. An empty string may be used if no additional header information is needed.
file_handle = FileOpen("CPU:Batt_Volt.csv", "w", -1)
FileWrite (file_handle, "2010-05-20T11:01:43Z," + batt_volt + CHR(10), 0)
FileClose (file_handle)

http_put_tx =HTTPPut("http://example.com/v2/feeds/12345/datastreams/datapoints", "CPU:Batt_Volt.csv", http_put_response, http_header)

CallTable Test
NextScan
EndProg
```

### Example #2

```
'In this example, data is streamed from the HTTPTest table to multiple files on a time
'interval. Each file is appended with a number in order to create unique file names.
'These files are sometimes referred to as bales.
Public PTemp
Public batt_volt
Public http_header As String * 100
Public http_put_response As String * 200
Public SERVER_PATH As String * 200
Public http_put_tx

'Define Data Tables
DataTable (HTTPTest,1,-1)'Set table size to -1 to autoallocate
DataInterval (0,15,Sec,10)
Minimum (1,batt_volt,FP2,0,False)
Sample (1,PTemp,FP2)
EndTable

'Main Program
BeginProg

http_header = ""'Indicates additional header information if required. An empty string may be used if no additional header information is needed.
SERVER_PATH = "http://example.com/v2/feeds/12345/datastreams/datapoints"'enter unique server path here

Scan (1,Sec,0,0)
PanelTemp (PTemp,4000)
Battery (batt_volt)
CallTable HTTPTest
NextScan

'Running HTTPPUT in a SlowSequence means the instruction will
'not interrupt or delay instructions in the main scan. The Do/Delay/Loop runs at
'an approximate interval as specified in the Delay()instruction, every 10 seconds
'in this example, but not necessarily at the top of the interval.

SlowSequence
Do
Delay(1,10,Sec)
'Create individual files named HTTPTest0, HTTPTest1, HTTPTest2, etc.
'every hour.
http_put_tx =HTTPPut(SERVER_PATH,"HTTPTest", http_put_response, http_header,0,1,Hr,8)
Loop

EndProg
```

### EXAMPLE #3

```
'In this example data is streamed from the HTTPTest table to a single data file on a
'server. New data will be appended to that file on a time interval. This is very
'similar to the way LoggerNet collects data.

Public PTemp
Public batt_volt

Public http_header As String * 100
Public http_put_response As String * 200
Public SERVER_PATH As String * 200
Public http_put_tx

'Define Data Tables
DataTable (HTTPTest,1,-1) 'Set table size to -1 to autoallocate.
DataInterval (0,15,Sec,10)
Minimum (1,batt_volt,FP2,0,False)
Sample (1,PTemp,FP2)
EndTable

'Main Program
BeginProg

http_header = ""'Indicates additional header information if required. An empty string may be used if no additional header information is needed.
SERVER_PATH = "http://example.com/v2/feeds/12345/datastreams/datapoints"'enter unique server path here

Scan (1,Sec,0,0)
PanelTemp (PTemp,4000)
Battery (batt_volt)
CallTable HTTPTest
NextScan

'Running HTTPPUT in a SlowSequence means the instruction will
'not interrupt or delay instructions in the main scan. The Do/Delay/Loop runs at
'an approximate interval as specified in the Delay()instruction, every 10 seconds
'in this example, but not necessarily at the top of the interval.

SlowSequence
Do
Delay(1,10,Sec)
'File format is TOA5 (FileOption 8) with header & record number.
'The header is written to the file with the first write of the file.
'Because 1000 has been added to the FileOption number (e.g.,1008), subsequent
'writes will not include header info unless the file has been deleted. In this
'example, 8 is the file format option (TOA5); 1000 indicates static file name;
'negating the value means do not write header with each write of a new
'record e.g., -1008.
http_put_tx =HTTPPut(SERVER_PATH,"HTTPTest", http_put_response, http_header,0,5,Min,-1008)
Loop

EndProg
```

## Remarks

This function returns the TCP socket that was created to communicate with the HTTP server. This function returns 0 if it fails or -2 if execution did not occur when the instruction was called (for instance, when using the optional parameters to stream data from a table and the number of records or time into interval conditions are not met). If this value is zero when the function is called, the datalogger will create a new connection to the web server.

**NOTE:** When multiple IP interfaces (for example, Ethernet port, CS I/O port, Wi-Fi, PPP) are active, the [IPRoute()](iproute.md) instruction is used to direct outgoing IP traffic.

You may also consider [EmailyRelay](emailrelay.md) or [FTPClient](ftpclient.md) if your server is not configured for HTTP.

HTTPPut supportsHTTP

**Note:** Hypertext Transfer Protocol. A TCP/IP application protocol.

and HTTPS. When this function is in the program,TLS

**Note:** Transport Layer Security. An Internet communication security protocol.

is enabled in the datalogger automatically, unless theURI

**Note:** Uniform Resource Identifier

is a constant and the first 5 characters are **HTTP:**.

If the header in the response from the server specifies Connection: close , the connection is closed; otherwise, it is left open. If the connection is left open, this same connection is used when the instance of HTTPPut that opened the connection is executed again.

[SemaphoreGet/SemaphoreRelease](semaphoregetsemaphorerelease.md)- Semaphore number 4 can be used by the CRBasic program to determine if a file is currently opened for rename via FTP or for sending/serving up via HTTP. SemaphoreGet(4) should only be used if the desire is to manage CRBasic processes around these type of events.

An example application is a datalogger generating a data file or image that is also served up via HTTP. SemaphoreGet(4) / SemaphoreRelease(4) can be used around the data file generation so that the datalogger does not change the contents of the file in the middle of it being downloaded over HTTP.

The last several parameters are optional. They can be used to send data from a data table directly to a server, without the datalogger first having to write the data to a file.

## Parameters

# URI (Uniform Resource Identifier)

A string that contains the uniform resource identifier (typically a URL) of the HTTP server to be accessed. A username and password can be passed into a URL using http://username:password@http.server.address.

Type: String or variable formatted as a string

# Contents

A string expression that contains the contents to be sent with the request. If streaming data directly from a data table or data table field, this parameter should be a constant specifying the table name (for example, TableOne ) or table field name (for example, TableOne.BattV_Min ) that will be used as the data source.

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

# NumRecs/TimeIntoInterval (Time into Interval to Write the Number of Records)

Used only when streaming data directly from a data table or data table field.

** NOTE:**For more details, see the [Data Streaming document](https://s.campbellsci.com/documents/us/technical-papers/ftp-streaming.pdf).

If Interval is greater than 0, the NumRecs/TimeIntoInterval parameter specifies the time into the interval at which previously unsent records should be written to file on the server.

If Interval is equal to 0, the NumRecs/TimeIntoInterval parameters specifies the number of previously unsent records that will be written to file on the server. If Interval is equal to 0, a negative NumRecs/TimeIntoInterval parameter will specify the number of records that will be written to file on the server each time the function is called.

Type: Constant, optional parameter

NumRecs/TimeIntoInterval

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

Used only when streaming data directly from a data table or data table field. It specifies the format used when writing data to the server. The file created on the server will automatically be appended with an incrementing file number and a .dat file extension. If 1000 is added to the format (for example, 1008), the datalogger will not automatically append the incrementing number or .dat extension to the uploaded file. Options 0, 8, 16, and 32 correspond toCampbell Scientific's defined formats for TOB1, TOA5, CSIXML, and CSIJSON, respectively.

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
