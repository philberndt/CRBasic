# TCPOpen (Set Up TCP/IP Communication Port)

The TCPOpen function is used to set up aTCP/IP

**Note:** Transmission Control Protocol / Internet Protocol.

socket for communication.

## Syntax

- Result\* = TCPOpen(IPAddr,TCPPort,IPBuffer,IPTimeOut[optional],ConnectHandle[optional],MaxConnect[optional] )

The following program examples were written for aCR350. Other dataloggers would use a similar program (range codes for measurement instructions might need to be changed).

### Example #1

```
'Datalogger will attempt to create a PakBus
'connection to a LoggerNet, datalogger, or other
'PakBus device at the specified IP address on port
'6785.

Public Handle As Long

BeginProg
Scan (1,Min,0,0)
TCPOpen("1.1.1.1",6785,0,5000,Handle,1)
NextScan
EndProg
```

### Example #2

```
'Datalogger will attempt to create multiple PakBus
'connections to the specified IP addresses on port
'6785.

Const MaxConnect = 3
Public IPAddr(MaxConnect) As String * 16
Public Handle(MaxConnect) As Long
Dim i

BeginProg
IPAddr(1) = "1.1.1.1"
IPAddr(2) = "2.2.2.2"
IPAddr(3) = "3.3.3.3"

Scan (1,Min,0,0)
For i = 1 To MaxConnect
TCPOpen(IPAddr(i),6785,0,5000,Handle(i),MaxConnect)
Next i
NextScan
EndProg
```

### Example #3

```
'Datalogger initates a call-back to LoggerNet
'every 1 minute. It triggers LoggerNet to perform
'a data collection by initiating a SendVariables
'transaction. In this example, the TCPOpen and
'SendVariables process is handled in a SlowSequence
'as not to hold up the faster execution of the main
'scan.

Public RefTemp, BattV
Public Callback As Boolean
Public Handle As Long
Public Result

DataTable (Test,1,-1)
DataInterval (0,15,Sec,10)
Minimum (1,BattV,FP2,0,False)
Sample (1,RefTemp,FP2)
EndTable

BeginProg
Scan (1,Sec,0,0)
PanelTemp (RefTemp,4000)
Battery (BattV)
CallTable Test
NextScan

SlowSequence
Scan (1,Min,3,0)
TCPOpen("1.1.1.1",6785,0,5000,Handle,1)
SendVariables (Result,Handle,-1,4094,0,0,"Public","Callback",1,1)
NextScan
EndSequence

EndProg
```

## Remarks

TCPOpen is used to initiate a TCP client socket connection or set up a listening TCP server. Common uses of TCPOpen include:

- Initiating a PakBus/TCP client connection to LoggerNet for the purposes of call-back or routing

- Initiating a PakBus/TCP client connection to another datalogger for the purpose of data exchange or routing

- Initiating a TCP client connection to another device for the purpose of data exchange using Modbus TCP or DNP3

- Initiating a TCP client or server connection for the purposes of sending and receiving custom messages over standard sockets, when coupled with CRBasic s serial I/O commands

When the datalogger acts as a TCP client, it will initiate a socket connection to the destination address and port number when TCPOpen is executed. If the specified connection already exists, TCPOpen will quickly return; it will not close the existing good connection and then reopen it. However, TCPOpen will not return a result until the connection is successful or until a timeout period has expired, potentially delaying the scan unpredictably. Placing it in a slow sequence will allow TCPOpen to run in the background and not hold up the main program while it waits for the socket to be opened successfully. If using a TCP socket for non-PakBus communications (for example, as an extended serial port) the datalogger will be unable to ensure the connection is functional as it does not know the frequency or nature of the expected data. If possible, your program should either monitor for unexpected inactivity (no data received) and then close and reopen the socket, or simply close and reopen the socket at regular intervals.

When the datalogger acts as a TCP server (i.e., when IPAddr = ), it will begin listening for incoming connections on the specified port after the first time TCPOpen is executed. Each time TCPOpen is called, it will return either 0 (no connection yet) or a connection handle if the datalogger has accepted a connection from a remote client. By default TCPOpen only allows a single active socket connection per listening port. To allow multiple concurrent connections, use the optional MaxConnect parameter. The connection handles from multiple connections are returned in the ConnectHandle variable (which, therefore, should be an array if multiple connections are expected). A new connection is stored in a vacant element in the array. The TCPOpen function will continue to return a connection handle as long as the connection remains open. If the connection is closed for any reason, the datalogger will immediately start listening again for a new connection, and until a new connection has been accepted, the function will again return 0.

If you have set up the datalogger to listen for an incoming TCP/IP connection, the TCPPort parameter should be different than the PakBus/TCP Service Port setting in the datalogger (6785 by default), since the datalogger is already listening on that port for PakBus packets.

A datalogger will maintain only one direct neighbor route to another PakBus address. Thus, intermittent communication can be caused if the datalogger program has two TCPOpen instructions to the same PakBus device or if two PakBus devices use TCPOpen to maintain a connection to each other. The established route to the PakBus device is dropped in order to set up the new route, causing communication failures.

When the function is executed, TCPOpen will return a non-zero integer value if a socket connection is successfully established. This value represents the datalogger resource handle used to reference the connection and can be treated like an extended ComPort. The non-zero value returned will start at 101 and will increment by 1 each time a new connection is created. If the socket fails to be created or is closed, a 0 is returned. If an established route is terminated unexpectedly and then restablished, the value of the returned integer is incremented by 1. The result variable can be used for determining if a connection has successfully been established or not. Additionally, it can often be used to specify the communications port (ComPort) in another function, such as [SerialOut()](serialout.md) or [ModbusClient()](modbusclient.md).

## Parameters

# IPAddr (IP Address)

The IP address for the socket you are trying to open. This is a string variable, which can be entered as a numeric address (for example, "xxx.xxx.xxx.xxx", with each xxx being a value of 0 to 255), or an IPv6 address, or a fully-qualified domain name (for example, "computer-name.domain.com"). Numeric IPv4 addresses should be entered in decimal notation, with no leading zeros (i.e., 192.168.**1 **.123, not 192.168.** 001 **.123). If you use a domain name, the address of aDNS

** Note:**Domain name server. A TCP/IP application protocol.

server must be specified in datalogger settings. For all instructions except UDPOpen and IPRoute, the IPAddr can also be set to a null string (""), in which case the datalogger will listen for an incoming TCP/IP connection on the specified port. The entry for the IPAddr must be enclosed in quotes.

Type: Variable defined as a string, enclosed in quotes

# TCPPort (TCP Port)

An integer value ranging from 1 to 65535 and may be specified as a constant or variable. When acting as a TCP client, this parameter specifies the destination port the datalogger will connect to on the remote device. When acting as a server, this parameter specifies the port the datalogger will listen on for incoming TCP connections. When acting as a server, care should be taken to not use a port number already in use by other datalogger services such as FTP (20/21), Telnet (23), HTTP (80), NTP (123), SNMP (161), HTTPS (443), Modbus (502), PakBus (6785), or DNP3 (20000).Caution: If you have set up the datalogger to listen for an incoming TCP/IP connection, the TCPPort parameter should be different than the TCPort setting in the datalogger (6785 by default), since the datalogger is already listening on that port.

Type: Variable or Constant

# IPBuffer (Input Buffer)

If the socket is being opened for communication with a serial (non-PakBus

** Note:**PakBus is a proprietary communication protocol developed by Campbell Scientific to facilitate communications between Campbell Scientific devices. Similar in concept to IP (Internet Protocol), PakBus is a packet-switched network protocol with routing capabilities. A registered trademark of Campbell Scientific, Inc.

) device, enter a value for the size of the input serial buffer that should be created in datalogger memory. Set this parameter to 0 for communication with a PakBus device. When this parameter is set to 0, the port will be opened as a PakBus port and PakBus beaconing will take place every 3 minutes. For serial communications with non-PakBus devices, specify a buffer size large enough to hold the number of bytes that will arrive between serial input reads. Note that oversizing the buffer needlessly takes away from datalogger memory. For non-PakBus communication, do not set this parameter to 0 or PakBus beacons initiated by the datalogger may interfere with communications. For non-PakBus communication that does require a user-specified buffer (such as when used with [ModbusClient()](modbusclient.md), set this parameter to 1 so that beaconing does not occur.

Type: Constant

## Optional parameters

# IPTimeOut (IP Connection Timeout)

When acting as a TCP client, the IPTimeOut parameter specifies the amount of time the datalogger will spend attempting to initiate the socket connection before failing. The value is entered in centiseconds (i.e., 0.01 seconds). If omitted, the timeout default is 75 seconds (7500). When acting as a TCP server, this parameter specifies the maximum amount of time allowed to elapse without active communications once a connection is accepted. If the timeout expires, the datalogger will close the connection. If omitted, a timeout will not be used and the datalogger will not close the connection due to lack of communications.

Type: Constant Integer

# ConnectHandle (Connect Handle)

ConnectHandle is a variable or array to which resource handles associated with the socket connection(s) will be stored. The argument should be declared as a variable or array of type LONG. This parameter must be used if a single TCPOpen instruction is to be used in a loop structure (typically) for the purpose of opening or serving multiple concurrent connections; the alternative would be to use multiple independent TCPOpen instructions within the program. This feature makes it easier to loop through an array of IPAddr and TCPPort combinations for the purpose of initiating multiple TCP client connections. When used for listening (IPAddr = ), it allows for MaxConnect clients to connect to a single TCPPort number. Unlike the value returned by TCPOpen, the ConnectHandle argument is updated in the background, asynchronous to the execution of TCPOpen. If the connection is closed for any reason, its corresponding ConnectHandle will immediately be set to a value of 0.

Type: Variable declared as Long

# MaxConnect (Maximum Number of Connections)

The maximum number of connections that can be created with a single instance of the TCPOpen function. MaxConnect must be a constant integer with a value of 1 or greater. Set this value to the size of the ConnectHandle array, or 1 if ConnectHandle is a single variable.

Type: Constant Integer

Optional parameters can be omitted in the program without consequence if they are not needed.

** NOTE:**The datalogger has a PakBus/TCP Client Connection setting, which can be used instead of the TCPOpen function, to specify up to four outgoing TCP/IP connections that the datalogger should attempt to maintain. In this instance, any PakBus instructions in the datalogger program should use a COMPort parameter of 0 and an invalid NeighborAddr (for example, -1 or 4099).
