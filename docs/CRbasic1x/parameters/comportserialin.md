# ComPort_Serial_In (Communications Port Serial In)

The COM port that will be used by the instruction. Right-click to display a list.

| Alphanumeric | Description                                |
| ------------ | ------------------------------------------ |
| ComRS232     | RS232 port of the datalogger               |
| ComMe        | Datalogger's CS I/O port;modem enabled     |
| Com310       | Datalogger's CS I/O port; Com310 modem     |
| Com320       | Datalogger's CS I/O port; Com320 modem     |
| ComSDC7      | Datalogger's CS I/O port; SDC7             |
| ComSDC8      | Datalogger's CS I/O port; SDC8             |
| ComSDC10     | Datalogger's CS I/O port; SDC10            |
| ComSDC11     | Datalogger's CS I/O port; SDC11            |
| ComC1        | Datalogger's control ports 1 (TX) & 2 (RX) |
| ComC3        | Datalogger's control ports 3 (TX) & 4 (RX) |
| ComC5        | Datalogger's control ports 5 (TX) & 6 (RX) |
| ComC7        | Datalogger's control ports 7 (TX) & 8 (RX) |

Type: Constant. For all instructions except SerialOpen, this parameter can also be a variable. However, if a variable is used, the port must be opened previously by [SerialOpen (Open Communication Port)](../Instructions/serialopen.md)
