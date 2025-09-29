# ComPortIsActive (Detect if COM Port is Active)

The ComPortIsActive function returns aBoolean

**Note:** Data type used to represent conditions or hardware that have only two states (true or false) such as flags and control ports.

value, based on whether or not activity is detected on the specified COM port.

## Syntax

- variable\* = ComPortIsActive(ComPort)

In the following example, a socket is opened using TCPOpen. ComPortIsActive is used to monitor the socket for activity. Once activity is no longer detected, the socket is closed.

```
Public ComActive As Boolean, Socket

BeginProg
Scan (1,Sec,3,0)
Socket = TCPOpen ("192.168.1.23",3000,0)
ComActive =ComPortIsActive(Socket)

'other instructions

NextScan

SlowSequence
Do
If ComActive = False Then TCPClose(Socket)
Loop

EndProg
```

## Parameter

# ComPort (Communications Port)

The communications port that will be used by the instruction. Right-click to display a list. Options vary depending on the instruction.

| Alphanumeric | Description                           |
| ------------ | ------------------------------------- |
| ComUSB       | Datalogger USB port                   |
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

If the instruction has a ResultCode parameter and a negative value is entered for the ComPort, the datalogger will not wait on a response from the destination device before proceeding to the next instruction.

Type: Constant. For all instructions except SerialOpen, this parameter can also be a variable.

ComPort can also be a virtual ComPort, such as the result of a [TCPOpen](tcpopen.md) instruction. This can be used to determine if the socket connection is still available.
