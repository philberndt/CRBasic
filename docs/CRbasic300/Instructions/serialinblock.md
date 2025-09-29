# SerialInBlock (Store Incoming Serial Data)

The SerialInBlock function stores incoming serial data. This function returns the number of bytes received.

## Syntax

```
SerialInBlock
```

(ComPort,Dest,MaxNumberBytes)

In the following example, the datalogger is put into a transparent communication mode. Any data coming in on the datalogger's RS232 port are sent out the datalogger's COM1. Any data coming in on the datalogger's COM1 are sent out the datalogger's RS232 port.

The SerialInBlock function is used to set the NumberBytes parameter in the SerialOutBlock instruction.

```
Public Str as STRING * 1000

'Main Program
BeginProg
'Open RS232 port at 115.2K baud
SerialOpen (ComRS232,115200,0,0,1000)
'Open COM1 at 9600 baud
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

Incoming serial data, up to the value defined in MaxNumberBytes, is stored in the Dest parameter. SerialInBlock will not wait for the return of characters. If no new characters are received since the last execution of the function, 0 is returned by the function. This function can be used as the expression for the NumberBytes parameter in the [SerialOutBlock](serialoutblock.md) function. For incoming data, extended ASCII characters (128-255) are supported only by SerialOpen format options 1-7 and 17-23.

## Parameters

# ComPort_Serial_In (Communications Port Serial In)

The COM port that will be used by the instruction.Note thatbeginning with OS 6.0,control ports C1 and C2 can be configured individually for transmit (Tx) or receive (Rx).Right-click to display a list.

| Alphanumeric | Description                                |
| ------------ | ------------------------------------------ |
| ComUSB       | USB port of the datalogger                 |
| ComRS232     | RS232 port of the datalogger               |
| Com1         | Datalogger's control ports 1 (TX) & 2 (RX) |
| ComC1_Rx     | Configures control port 1 for receive only |
| ComC2_Rx     | Configures control port 2 for receive only |
| ComRF        | Integrated RF communication                |

Type: Constant. For all instructions except SerialOpen, this parameter can also be a variable. However, if a variable is used, the port must be opened previously by [SerialOpen (Open Communication Port)](serialopen.md)

For the SerialInBlock instruction, the ComPort parameter specifies the communication port and mode that is used when receiving the binary data. A variable can be used in the ComPort parameter for use with functions that return a communication port. If communication occurs using TCP/IP, enter the variable for the socket returned by [TCPOpen](tcpopen.md).

# Dest (Destination)

The Variable in which to store the results of the instruction. Right-click the parameter to display a list of defined variables.

If this instruction has a Repetitions parameter and it is greater than 1, the results are stored in an array with the variable name. The array must be dimensioned large enough to hold all of the values returned from all of the Reps.

Type: Variable or Array

For the SerialInBlock instruction, the Dest parameter is the variable in which to store the incoming serial data. Once the MaxNumberBytes of incoming data is received, no more data is stored. Note that Dest is not reset to null if no new characters are received. The program can be written to ignore Dest if the instruction returns 0, or to process Dest for the number of characters received at each execution.

# MaxNumberBytes (Maximum Number of Bytes)

The maximum number of bytes of binary data that will be stored in the Dest parameter.

Type: Constant (or variable or expression evaluated as a constant)
