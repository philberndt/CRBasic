# ComPortIsActive (Detect if COM Port is Active)

The ComPortIsActive function returns aBoolean

**Note:** Data type used to represent conditions or hardware that have only two states (true or false) such as flags and control ports.

value, based on whether or not activity is detected on the specified COM port.

## Syntax

- variable\* = ComPortIsActive(ComPort)

In the following example, when the RS232 port is active, ComActive will report true. This program can be tested by connecting to the datalogger via RS232, disconnecting from the datalogger, and collecting data. When you are connected ComActive = -1; otherwise, it is 0.

```
Public ComActive As Boolean

DataTable (CheckCom,True,-1 )
Sample (1,ComActive,Boolean)
EndTable

BeginProg
Scan (1,Sec,3,0)
ComActive =ComPortIsActive(ComRS232)
'other instructions
CallTable CheckCom
NextScan
EndProg
```

## Parameter

# ComPort (Communications Port)

The communications port that will be used by the instruction. Right-click to display a list. Options vary depending on the instruction.

| Alphanumeric | Description                                   |
| ------------ | --------------------------------------------- |
| ComUSB       | USB port of the datalogger                    |
| ComRS232     | RS232 port of the datalogger                  |
| Com1         | Datalogger control terminals C1(TX) & C2 (RX) |
| Com2         | COM2 (TX,RX) of the datalogger                |
| Com3         | COM3 (TX,RX) of the datalogger                |
| ComRF        | Integrated RF communication                   |

If the instruction has a ResultCode parameter and a negative value is entered for the ComPort, the datalogger will not wait on a response from the destination device before proceeding to the next instruction.

Type: Constant. For all instructions except SerialOpen, this parameter can also be a variable.

ComPort can also be a virtual ComPort, such as the result of a [TCPOpen](tcpopen.md) instruction. This can be used to determine if the socket connection is still available.
