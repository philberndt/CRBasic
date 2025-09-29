# SerialOutBlock (Send Data Out a Serial Port)

The SerialOutBlock instruction is used to send binary data out a serial port.

## Syntax

```
SerialOutBlock
```

(ComPort,Expression,NumberBytes)

In the following example, the datalogger is put into a transparent communication mode. Any data coming in on the datalogger's RS232 port are sent out the datalogger's COM1. Any data coming in on the datalogger's COM1 are sent out the datalogger's RS232 port.

The SerialInBlock function is used to set the NumberBytes parameter in the SerialOutBlock instruction.

```
Public Str as STRING * 1000

'Main Program
BeginProg
'Open RS232 port at 115.2K baud
SerialOpen (ComRS232,115200,0,0,1000)
'Open COM1 at 115200 baud
SerialOpen (COM1,9600,0,0,1000)

Scan (1,Sec,3,0)
'Send data out COM1; set NumberBytes to the size of the incoming string
SerialOutBlock(COM1,Str,SerialInBlock(ComRS232,Str,1000))
'Send data out RS232 port, set NumberBytes to the size of the incoming string
SerialOutBlock(ComRS232,Str,SerialInBlock(COM1,Str,1000))
NextScan
EndProg
```

## Remarks

This instruction is needed when the data to be transmitted contains a null value. (The [SerialOut](serialout.md) instruction is terminated with a null value, thus, the transmission of binary data is required.) It can also be used when the number of bytes to be output is variable, or when the device receiving the transmitted data requires that data to be in a binary format.

## Parameters

# ComPort_Serial_Out (Communications Port Serial Out)

The COM port that will be used by the instruction.Note that control ports C1 and C2 can be configured individually for transmit (Tx) or receive (Rx).Right-click to display a list.

| Alphanumeric | Description                                 |
| ------------ | ------------------------------------------- |
| ComUSB       | USB port of the datalogger                  |
| ComRS232     | RS232 port of the datalogger                |
| Com1         | Datalogger's control ports 1 (TX) & 2 (RX)  |
| Com2         | COM2 (TX & RX) port of the datalogger       |
| Com3         | COM3 (TX & RX) port of the datalogger       |
| ComC1_Tx     | Configures control port 1 for transmit only |
| ComC2_Tx     | Configures control port 2 for transmit only |
| ComRF        | Integrated RF communication                 |

Type: Constant. For all instructions except SerialOpen, this parameter can also be a variable.

A variable can be used in the ComPort parameter for use with functions that return a communication port. If communication occurs using TCP/IP, enter the variable for the socket returned by [TCPOpen](tcpopen.md).

# Expression (Data Being Transmitted)

The data that is being transmitted over the specified COM port.

Type: Public variable, string, or the results of an evaluated expression

# NumberBytes (Number of Bytes)

The number of bytes of binary data from the Expression that should be transmitted. An expression or the [SerialInBlock](serialinblock.md) function can also be used in this parameter.

Type: Constant or expression that evaluates as a constant
