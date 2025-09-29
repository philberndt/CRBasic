# IPTrace

The IPTrace function is used to write IP debug messages to a string variable. This instruction is not supported in Operating System v.3 or earlier.

## Syntax

IPTrace(Dest)

The following example program shows the use of IPTrace to store low level protocol information to a string.

```
Public Socket As Long, PTemp, TCTemp, Result, Flag(2)
Public IPDebug As String * 1000

DataTable (Temp,True,-1)
Sample (1,PTemp,FP2)
Sample (1,TCTemp,FP2)
EndTable

DataTable (Trace,IPTrace(IPDebug)>0,-1)
Sample (1,IPDebug,String)
EndTable

BeginProg
SetSetting ("IPTraceCode", "336")'Protocol level error, timeout & transport protocol msgs
Scan (1,Sec,3,0)
IPTrace(IPDebug)
PanelTemp (PTemp,4000)
TCDiff (TCTemp,1,mv34,1,TypeT,PTemp,True,0,4000,1.0,0)

If Flag(1) Then
Socket=TCPOpen ("192.168.26.93",3000,0)
SendVariables (Result,Socket,-1,4084,0000,0,"Public","Callback",TCTemp,1)
Flag(1)=False
EndIf

If Flag(2) Then TCPClose(Socket)
CallTable (Temp)
CallTable (Trace)
NextScan

EndProg
```

## Remarks

The IPTrace returns the number of characters written to the Dest variable. Dest should be formatted as a String variable dimensioned large enough to hold an error message (for example, Dim Dest As String \* 100).

The IP Trace Code in the datalogger s Settings must be enabled for this function to return a value or store messages to the destination variable.

| Code  | Description                                                   |
| ----- | ------------------------------------------------------------- |
| 0     | Trace is inactive                                             |
| 1     | Startup and watchdog messages only                            |
| 2     | Verbose PPP                                                   |
| 4     | General informational messages (including email send/receive) |
| 8     | Network interface error messages                              |
| 16    | Protocol level error messages                                 |
| 32    | Link level network layer messages                             |
| 64    | Timeout messages                                              |
| 128   | Application level packet messages                             |
| 256   | Transport protocol (UDP/TCP/RVD) trace                        |
| 512   | Internet layer packet messages                                |
| 1024  | Upcall progress messages                                      |
| 2048  | Lossy receive messages                                        |
| 4096  | Lossy transmit messages                                       |
| 8192  | FTP status messages                                           |
| 16384 | SMTP status messages                                          |
| 65535 | Trace everything                                              |

IPTrace can be used as the output trigger for a data table, thus setting up a data table exclusively for holding any error messages from TCP/IP communication.
