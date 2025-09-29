# PortBridge (Bridge Two Comports)

The PortBridge instruction installs a "bridge" between two comports (a comport can be a TCP connection handle). Incoming bytes from one of the ports are directed out the other.

## Syntax

PortBridge(Enable,Bridge)

The test program below shows how the PortBridge instruction can be used to configure the datalogger as an IP serial server.

```
'Declare Variables
Public Enable As Boolean

Public Handle1 As Long
Public Handle2 As Long
Public Handle3 As Long
Public Handle4 As Long

'The zeros below are place holders for port 1 received bytes, port 1 sent bytes,
'port 2 TCP connection number, port 2 received bytes, and port 2 sent bytes.
'When bytes are successfully sent and received, the zeros will be replaced with
'the number of bytes received and sent. Also, when a successful TCP connection is made,
'the zero for comport 2 will be replaced by a socket number
'(connection number returned by TCPOpen).

Public Bridge1(2,3) As Long = {COMC1,0,0,0,0,0}
Public Bridge2(2,3) As Long = {ComC3,0,0,0,0,0}
Public Bridge3(2,3) As Long = {ComC5,0,0,0,0,0}
Public Bridge4(2,3) As Long = {ComC7,0,0,0,0,0}

PortBridge(Enable,Bridge1)
PortBridge(Enable,Bridge2)
PortBridge(Enable,Bridge3)
PortBridge(Enable,Bridge4)

BeginProg

SerialOpen (COMC1,115200,3,0,1000)
SerialOpen (ComC3,115200,3,0,1000)
SerialOpen (ComC5,115200,3,0,1000)
SerialOpen (ComC7,115200,3,0,1000)

Scan(1,Sec,0,0)
Handle1 = TCPOpen ("",3000,1000,6000,Handle1,1)
Handle2 = TCPOpen ("",3001,1000,6000,Handle2,1)
Handle3 = TCPOpen ("",3002,1000,6000,Handle3,1)
Handle4 = TCPOpen ("",3003,1000,6000,Handle4,1)

Bridge1(2,1) = Handle1
Bridge2(2,1) = Handle2
Bridge3(2,1) = Handle3
Bridge4(2,1) = Handle4
NextScan
EndProg
```

## Remarks

The Portbridge instruction can be used to configure a datalogger as an IP serial terminal server. The PortBridge instruction may also be used to bridge two or more serial ports or two or more IP ports.

## Parameters

# Enable

A variable of type Boolean that when TRUE enables the bridge.

Type: Variable as Boolean

# Bridge

A 2-by-3 variable array of type Long that holds the comport, received bytes, and sent bytes for each of the two ports in the bridge. Zeros are used as place holders for received and sent bytes until bytes are actually received or sent. If an error occurs sending bytes, sent bytes will increment negatively for each failure to send.

If a comport is a TCP connection handle, 0 is used as a place holder for the comport until a successful TCP connection is made. For example, Public Bridge1(2,3) As Long = {COMC1,0,0,0,0,0}. In this example, COMC1 is the first comport and a TCP connection handle is the second comport. When a successful TCP connection is made, the 0 for the second comport will be replaced with a socket/connection number.

Valid Comports include:

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

ComPort can also be a virtual ComPort, such as the result of a TCPOpen instruction.

Beginning withCR1000X OS 05, an SDM-SIO1A or SDM-SIO4A may be used as a ComPort. See the [SerialOpen (Open Communication Port)](serialopen.md) help for valid SDM-SIOxA port numbers.

Type: Variable as 2-by-3 array of type Long
