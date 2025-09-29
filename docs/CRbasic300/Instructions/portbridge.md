# PortBridge (Bridge Two Comports)

The PortBridge instruction installs a "bridge" between two comports (a comport can be a TCP connection handle). Incoming bytes from one of the ports are directed out the other.

## Syntax

PortBridge(Enable,Bridge)

The test program below shows how the PortBridge instruction can be used to configure the datalogger as an IP serial server.

```
'Declare Variables
Public Enable As Boolean

Public Handle1 As Long

'The zeros below are place holders for port 1 received bytes, port 1 sent bytes,
'port 2 TCP connection number, port 2 received bytes, and port 2 sent bytes.
'When bytes are successfully sent and received, the zeros will be replaced with
'the number of bytes received and sent. Also, when a successful TCP connection is made,
'the zero for comport 2 will be replaced by a socket number
'(connection number returned by TCPOpen).

Public Bridge1(2,3) As Long = {COM1,0,0,0,0,0}

PortBridge(Enable,Bridge1)

BeginProg

SerialOpen (COM1,115200,3,0,1000)

Scan(1,Sec,0,0)
Handle1 = TCPOpen ("",3000,1000,6000,Handle1,1)

Bridge1(2,1) = Handle1

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

| Alphanumeric | Description                                 |
| ------------ | ------------------------------------------- |
| ComUSB       | USB port of the datalogger                  |
| ComRS232     | RS232 port of the datalogger                |
| Com1         | Datalogger's control ports 1 (TX) & 2 (RX)  |
| ComC1_Tx     | Configures control port 1 for transmit only |
| ComC1_Rx     | Configures control port 1 for receive only  |
| ComC2_Tx     | Configures control port 2 for transmit only |
| ComC2_Rx     | Configures control port 2 for receive only  |
| ComRF        | Integrated RF communication                 |

ComPort can also be a virtual ComPort, such as the result of a TCPOpen instruction.

Type: Variable as 2-by-3 array of type Long
