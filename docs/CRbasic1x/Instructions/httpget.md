# HTTPGet (Retrieve from Web)

The HTTPGet function is used to send a request to an HTTP server using the Get method.

## Syntax

HTTPGet(URI,Response,Header,TimeOut[optional]) **Example 1** The following example program demonstrates using HTTPGet to retrieve an image from a web site and store the image as a file on the datalogger's CPU drive.

```
'Declare Public Variables
'Example:
Public PTemp, batt_volt
Public get_success'returns 0 if not successful, otherwise it should be a positive number (file handle)

'Define Data Tables
DataTable (Test,1,1000)
DataInterval (0,60,Sec,10)
Minimum (1,batt_volt,FP2,0,False)
Sample (1,PTemp,FP2)
EndTable

'Main Program
BeginProg
Scan (1,Min,0,0)
PanelTemp (PTemp,15000)
Battery (batt_volt)

'HTTPGet Instruction/Function
'get an image file from an internet server
get_success =HTTPGet( "http://www.mysite.com/images/myimage.png", "CPU:myimage.png", "")

CallTable Test

NextScan
EndProg
```

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


The HTTPGet function returns aLong

**Note:** Data type used when declaring a variable as an integer.

value that represents the socket reference used by the datalogger for communication. When the instruction is successful, the value is greater than 100.

HTTPGet supportsHTTP

**Note:** Hypertext Transfer Protocol. A TCP/IP application protocol.

and HTTPS. When this function is in the program,TLS

**Note:** Transport Layer Security. An Internet communication security protocol.

is enabled in the datalogger automatically, unless theURI

**Note:** Uniform Resource Identifier

is a constant and the first 5 characters are **HTTP:**.

By default the datalogger includes the method, requested URI, HTTP version, user agent, and host in its request. The Header parameter can be used to specify additional header information. Many applications will not require additional header information, in which case, the Header parameter can be an empty string. If additional header information is sent with the request, the string needs to be set before sending each request since the server will return header information into the string as well.

If the header in the response from the server specifies Connection: close , the connection is closed; otherwise, it is left open. If the connection is left open, this same connection is used when the instance of HTTPGet that opened the connection is executed again.

A PakBus address of 3213 in the [Files Manager](filesmanager2.md) setting of the datalogger can be used to enumerate/manage files written by HTTPGet.


## Parameters



# URI (Uniform Resource Identifier)


A string that contains the uniform resource identifier (typically a URL) of the HTTP server to be accessed. A username and password can be passed into a URL using http://username:password@http.server.address.

Type: String or variable formatted as a string


# Response


Indicates where the results of the request will be written. The parameter can be a variable or the name of a valid file on the datalogger.

Type: String or variable formatted as a string


# Header


A string that indicates the additional header information to include in the request. As a result of the request, the server may return header information that is stored in the string. The Header parameter can be an empty string if no additional header information is required.

In the case that more than one header is required, headers can be separated by & CHR(13) & CHR(10) &. For example:

HTTPPost(https://api.placeholder.com/v1/chat/completions,Query\_str,Response,"Content-Type: application/json"**& CHR(13) & CHR(10) &**"Authorization: Bearer xx-XXXXXX")

Type: String or variable formatted as a string

- Text (description)


# TimeOut (Response Wait Time)


Specifies a time period, in 0.01 seconds, that the datalogger will wait for input after a connection is made, before considering the attempt failed and incrementing Result. The default TimeOut in the absence of this parameter is 7500 (i.e., 75 seconds).

Type: Constant

More information on the Get method, including URIs and valid header fields can be found on the [W3C web site](https://www.w3.org/Protocols/rfc2616/rfc2616.html).
```
