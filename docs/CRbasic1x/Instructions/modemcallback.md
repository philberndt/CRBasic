# ModemCallBack (Call Computer via Phone Modem)

The ModemCallback instruction is used to initiate a call to a computer via a phone modem

## Syntax

ModemCallback(Result,COMPort,BaudRate,Security,DialString,ConnectString,Timeout,RetryInterval,AbortExp)

In the following example program, a callback is initiated via a COM220 modem when a thermocouple value goes above 30. ModemCallBack assumes that LoggerNet resides at the phone number and works with LoggerNet of any PakBus Address.

```
'Declare Variables
Public Ptemp, TCTemp
Public Result as Long, Abort as Boolean, Trigger as Boolean

DataTable (Temp,True,-1)
DataInterval (0,1,Sec,10)
Sample (1,TCTemp,FP2)
EndTable

'Main Program
BeginProg
Timer(1,Min,0)'initiate/start timer
Scan (1,sec,3,0)
PanelTemp (PTemp,15000)
TCDiff (TCTemp,1,mv200C,1,TypeT,PTemp,True,0,15000,1.0,0)

'Initiates a callback every 10 minutes as long as TCTemp>30
If TCTemp>30 AND Timer(1,Min,4)>10 Then
Abort = False
Timer(1,Min,2)
EndIf

ModemCallBack(Result,COMSDC7,115200,0,"97550000","",40,60,Abort)
CallTable(Temp)
Nextscan
EndProg
```

## Remarks

The computer must be running LoggerNet configured to accept a callback from the datalogger. When LoggerNet receives a callback attempt from the datalogger, it will collect any new table data that has been stored since the last data collection (the data collection could be a scheduled call, a manual call initiated from LoggerNet's Connect window, or from another callback).

ModemCallback can also be used to initiate a dialed call to another datalogger. The datalogger initiating the call will attempt to set a variable named Callback in the datalogger's Public table. This variable can then be used as a flag to trigger some other action.

After the modems successfully connect, the datalogger will issue a broadcast Hello Request to determine the address of the neighbor to which it has connected. If a neighbor is found, the datalogger will send a SendVariable() callback message (i.e., it will set a Public variable named CallBack to -1). If no neighbor is discovered, it will continue to send the Hello Request until a neighbor is discovered or the timeout period expires.

Note that once executed, this instruction runs in the background. The datalogger will not wait for successful communication before moving on to the next instruction.

## Parameters

# Result

The Result parameter is the variable in which a response code for the instruction will be stored. This response indicates the status of the dialing attempt (codes -30 through -35) and ultimately, whether or not the attempt was successful. A positive value indicates that there was no response to the request from the remote, and thus, the attempt was unsuccessful. A negative value indicates some other type of error occurred. A zero indicates a successful transaction. Because the support software will set the Abort variable to True to hang up the call, a -33 indicates that the callback attempt was successful or was aborted by the program or user in some other way. It will remain -33 until the Abort variable is set False by the program or some other means. The codes that can be returned are:

| Code    | Description                                                                                                                                                                                   |
| ------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| 0       | Successful.                                                                                                                                                                                   |
| 1, 2..n | The number of timeouts waiting for a response. The value will increment with each successive failure. After a 0 or negative response, the value will start over at 1.                         |
| -1      | Response received but permission denied.                                                                                                                                                      |
| -16     | Table name and/or field name not present in the source datalogger, or the field is read only in the destination datalogger.                                                                   |
| -17     | Data type not supported.                                                                                                                                                                      |
| -18     | Array in the source datalogger is not dimensioned large enough to accommodate the values to be sent or array in the destination datalogger is not large enough to accommodate values received |
| -20     | Out of Comms memory.                                                                                                                                                                          |
| -21     | Failed to route packet when routing is set to auto-discover and route is not yet known.                                                                                                       |
| -22     | Communication port buffer exceeded.                                                                                                                                                           |
| -27     | DialSequence/EndDialSequence returned False so communication did not occur.                                                                                                                   |
| -30     | Dialing                                                                                                                                                                                       |
| -31     | Dialed successfully, waiting for connection                                                                                                                                                   |
| -32     | Connected, sending message to LoggerNet                                                                                                                                                       |
| -33     | Attempt aborted by abort expression                                                                                                                                                           |
| -34     | ComPort is busy with some other function                                                                                                                                                      |
| -35     | Modem not echoing AT commands, sending +++ and toggling power to modem                                                                                                                        |

Type: Variable

# ComPort

The ComPort parameter specifies the communication port and mode that will be used for this instruction.

| Alphanumeric | Description                           |
| ------------ | ------------------------------------- |
| ComRS232     | RS232 port of the datalogger          |
| ComME        | Datalogger CS I/O port; modem enabled |
| Com310       | Datalogger CS I/O port; COM310 modem  |
| Com320       | Datalogger CS I/O port, COM320 modem  |
| ComSDC7      | Datalogger CS I/O port; SDC7          |
| ComSDC8      | Datalogger CS I/O port; SDC8          |
| ComSDC10     | Datalogger CS I/O port; SDC10         |
| ComSDC11     | Datalogger CS I/O port; SDC11         |
| ComC1        | Datalogger control terminals 1 & 2    |
| ComC3        | Datalogger control terminals 3 & 4    |
| ComC5        | Datalogger control terminals 5 & 6    |
| ComC7        | Datalogger control terminals 7 & 8    |

A variable can be used in the ComPort parameter for use with functions that return a communication port. If communication occurs using TCP/IP, enter the variable for the socket returned by TCPOpen.

Type: Constant

# BaudRate (Data Transmit Rate)

The rate, in bps, at which data is transmitted. The options are 300,600,1200, 2400, 4800, 9600, 19200, 38400, 57600, and 115200. Selecting one of these options fixes the baud rate at that rate of communication.If a negative baud rate is entered, the first communication attempt will be at the specified baud rate, but if communication fails at that rate, the datalogger will go into autobaud mode (where it will try different rates until successful or until the instruction times out).

**NOTE:** Autobaud is not available on control ports used as com ports. Baud rate for SDC ports must be 9600 or greater.If a serial port is opened, it must be closed before changing the port baud rate.

**NOTE:** If you are using SerialOpen to control a SDM-SIOx (SDM-SI01A, SDM-SIO1A, SDM-SIO2R) automatic baud rate detection is not supported. Rather, setting the baud rate to a negative value enables automatic flow control (RTS/CTS).Click herefor additional information.

Right-click this parameter to display a list.

**Type:** Constant. In [SerialOpen](serialopen.md), BaudRate can be a variable.

# Security

The security code of the remote datalogger. 0 is entered for this parameter if no security is set in the destination datalogger.

Type: Integer

If security is enabled, it must be unlocked to level 3.

**NOTE:** If other data logger security settings, such as TCP password and PakBus Encryption are set, these must also match between remote and local data loggers for successful data logger to data logger communications to occur.

# DialString

The telephone number and any other codes used to dial the modem. A semi-colon is used to separate multiple commands. Two semi-colons in a row will insert a 1 second delay before continuing to the next characters in the string. If only a phone number is used for the DialString parameter the datalogger will automatically issue an ATDT prior to issuing the number. If an AT command is added to the DialString parameter (e.g., ATV1ATDT) the automatic ATDT is not issued.

Type: String

# ConnectString

Used to specify the response code expected back from the modem when a connection is made. The ConnectString will succeed even when a partial response string is entered (for instance, a ConnectString of "Connect" will be successful if the response code is "Connect", "Connect 9600", or "Connect 1200").

When using the COM220 modem, enter a null string (""). The COM220 can return one of eight responses, depending upon the baud rate at which the connection is made (1 Connect, 5 Connect1200, 10 Connect2400, 13 Connect9600, 18 Connect4800, 20 Connect7200, 21 Connect12000, 25 Connect14400). When a null string is entered, the datalogger will compare the modem's response with these eight known responses in an attempt to establish communication.

Type: String

# Timeout

Used to specify the maximum time, in seconds, for dialing and connecting to the LoggerNet server (or remote datalogger). Valid entries for Timeout are 10 through 120 seconds. If the Timeout is exceeded and no connection has been made the datalogger will enter its Retry mode. When the Timeout period expires, the modem is turned off until the next retry. After 10 unsuccessful retries, the instruction will abort the attempt to connect.

Type: Constant

# Retry

The amount of time, in seconds, that the datalogger should wait before attempting another callback. The retry will take place at a random interval that falls between RetryInterval and 2\* RetryInterval.

Type: Constant

# AbortExp

Used to abort the callback attempt and stop any further attempts. If AbortExp is a variable the datalogger will immediately set its AbortExp parameter to True when a callback attempt succeeds. The instruction will not execute when AbortExp is True, and it will abort in the middle of dialing if the expression turns True.

Type: Variable or expression
