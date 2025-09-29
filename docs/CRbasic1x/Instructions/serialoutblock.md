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
SerialOpen (COMC1,9600,0,0,1000)

Scan (1,Sec,3,0)
'Send data out COM1; set NumberBytes to the size of the incoming string
SerialOutBlock(COMC1,Str,SerialInBlock(ComRS232,Str,1000))
'Send data out RS232 port, set NumberBytes to the size of the incoming string
SerialOutBlock(ComRS232,Str,SerialInBlock(COMC1,Str,1000))
NextScan
EndProg
```

## Remarks

This instruction is needed when the data to be transmitted contains a null value. (The [SerialOut](serialout.md) instruction is terminated with a null value, thus, the transmission of binary data is required.) It can also be used when the number of bytes to be output is variable, or when the device receiving the transmitted data requires that data to be in a binary format.

**NOTE:** This instruction normally runs sequentially from the processing task sequencer, regardless of whether the datalogger is in pipeline or sequential mode. However, when running in pipeline mode, if the COMPort parameter is a constantset to COMC1, COMC3, COMC5, or COMC7and NumberBytes is a constant, the instruction will run from the digital task sequencer. This instruction will also run from the digital task sequencer when used with the SDM-SIO1 (SerialOpen ComPort parameter set to 32..47). When run from the digital task in these instances, care must be taken while using other serial I/O instructions, which may run in the processing task and be out of sequence with SerialOutBlock.

This instruction can also be used along with other serial instructions to set up and control the [SDM-SIO1A or SDM-SIO4A](sdmsio1a_sdmsio4a.md)[or SDM-SIO2R](sdmsio2r.md).

## Parameters

# ComPort_Serial_Out (Communications Port Serial Out)

The COM port that will be used by the instruction. Right-click to display a list.

| Alphanumeric | Description                                |
| ------------ | ------------------------------------------ |
| ComRS232     | RS232 port of the datalogger               |
| ComME        | Datalogger's CS I/O port; modem enabled    |
| Com310       | Datalogger's CS I/O port; COM310 modem     |
| Com320       | Datalogger's CS I/O port; COM320 modem     |
| ComSDC7      | Datalogger's CS I/O port; SDC 7            |
| ComSDC8      | Datalogger's CS I/O port; SDC 8            |
| ComSDC10     | Datalogger's CS I/O port; SDC10            |
| ComSDC11     | Datalogger's CS I/O port; SDC 11           |
| ComC1        | Datalogger's control ports 1 (TX) & 2 (RX) |
| ComC3        | Datalogger's control ports 3 (TX) & 4 (RX) |
| ComC5        | Datalogger's control ports 5 (TX) & 6 (RX) |
| ComC7        | Datalogger's control ports 7 (TX) & 8 (RX) |

SerialOpen, SerialClose, SerialOut, and SerialOutBlock also support output via SDE pin enabled or any SDC address by using extended ComPorts. Valid extended ComPorts are &H1F through &Hff (31..255, SDC address including mode bits. Note, however, that 31 is SDE pin enabled and 32 through 47 are used to address the SDM-SIO1A). When an extended ComPort is used, the output instructions will turn on SDE or address the CS I/O port with the specified SDC address, delay the amount of time specified by TXDelay, output asynchronously at the specified baud rate in the SerialOpen() instruction, delay the amount of time specified by TXDelay, then reset the CS I/O port.

**NOTE:** The Argos and GOES satellite extended ComPort address is &H41 (65). To specify an SDC address, the most significant bit is the address; e.g., SDC 7 would be &H70 (112).

**NOTE:** When using the ComME ComPort with non-PakBus protocols (PPP, ModBus, DNP3, or generic serial applications), incoming characters can be corrupted by concurrent use of the CS I/O port for SDC communication (e.g., keyboard display).

Type: Constant. For all instructions except SerialOpen, this parameter can also be a variable.

A variable can be used in the ComPort parameter for use with functions that return a communication port. If communication occurs using TCP/IP, enter the variable for the socket returned by [TCPOpen](tcpopen.md).

# Expression (Data Being Transmitted)

The data that is being transmitted over the specified COM port.

Type: Public variable, string, or the results of an evaluated expression

# NumberBytes (Number of Bytes)

The number of bytes of binary data from the Expression that should be transmitted. An expression or the [SerialInBlock](serialinblock.md) function can also be used in this parameter.

Type: Constant or expression that evaluates as a constant
